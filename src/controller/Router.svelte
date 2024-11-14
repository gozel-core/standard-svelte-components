<script lang="ts">
    import { type ComponentType } from 'svelte'
    import { _, locale } from 'svelte-i18n'
    import { logger, initIndexedDb, os } from '@gozel-core/standard-js-frontend'
    import { metapatcher } from 'metapatcher'
    import { router, type RouteMap, type RouteOpts } from '@gozel-core/standard-svelte'
    import { BROWSER } from 'esm-env'

    export let path: string
    export let idbapi: AsyncReturnType<ReturnType<typeof initIndexedDb>>
    export let pageComponents: Record<string, ComponentType>
    export let siteUrl: string
    export let isHot: boolean
    export let isSsr: boolean
    export let mode: string

    const _routemap: RouteMap = []
    for (const page of os.getPages()) {
        if (!Object.hasOwn(pageComponents, page.svelte_component)) {
            throw new Error(`There is no page component:${page.svelte_component} for the page:${page.id}`)
        }

        const opts: RouteOpts = {}
        if (page.routing_identifier) opts.identifier = page.routing_identifier
        if (page.routing_identifier === 'not-found') opts.isFallback = true

        Object.keys(page.paths).map((_locale) => {
            const _path = page.paths[_locale as keyof typeof page.paths]
            _routemap.push([_path, pageComponents[page.svelte_component], Object.assign({}, opts, { locale: _locale })])
            // logger.info(`Registering route:${_path} with component:${page.svelte_component}`)
        })
    }

    router.configure(
        new URL(path, BROWSER ? window.location.href : siteUrl),
        _routemap,
        { preserveScrollOnRouteChange: false, isHot, isSsr }
    )

    if (isSsr) {
        renderPageMeta(os.getPage(path, $locale as string))
    }
    else {
        router.subscribe(({ current, prev }) => {
            if (!router.isPathExists(current.path)) {
                const notFound = os.getPage(router.getRoutePathById('not-found', $locale as string))
                renderPageMeta(notFound)
            }
            else {
                renderPageMeta(os.getPage(current.path, $locale as string))
            }

            logger.info(`Route changed to "${current.path}". Previous route is now "${prev ? prev.path : ''}"`)
        })
    }

    function renderPageMeta (page: ReturnType<typeof os.getPage>) {
        const pagepath = page.paths[$locale as keyof typeof page.paths]
        const pagemeta = page.translations.find((o) => o.locale === $locale)!
        const home = os.getPage(router.getRoutePathById('home', $locale as string), $locale as string)

        metapatcher.setPageDetails({
            title: pagemeta.title + ' | ' + $_('site_title'),
            description: pagemeta.excerpt,
            path: isSsr ? siteUrl + pagepath : window.location.href,
            image: page.head_image || home.head_image ? os.getMediaPreset((page.head_image ?? home.head_image) as string, '720pin').path : undefined,
            locale: $locale as string,
            canonical: siteUrl + (page.seo_canonical_url ?? pagepath),
            localVersions: Object.keys(page.paths).map((_locale) => ({ hreflang: _locale, href: siteUrl + page.paths[_locale as never] })),
            robots: mode === 'production' ? (page.seo_robots_directive ?? 'all') : 'noindex, nofollow',
            breadcrumb: page.breadcrumb.map((id) => {
                const _page = os.getPageById(id)
                return {
                    title: _page.translations.find((o) => o.locale === $locale)!.title,
                    url: siteUrl + _page.paths[$locale as keyof typeof _page.paths]
                }
            })
        })
    }

    type AsyncReturnType<T> = T extends Promise<infer U> ? U : T
</script>

<svelte:head>
    {@html isSsr ? metapatcher.dump() : ''}
</svelte:head>

<svelte:component this={$router.current.component} {idbapi} />
