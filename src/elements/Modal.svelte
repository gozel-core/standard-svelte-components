<script lang="ts">
    import { sineIn, sineOut } from 'svelte/easing'
    import { fade } from 'svelte/transition'
    import { createEventDispatcher, onMount } from 'svelte'
    import { outsideClick } from '@gozeltr/standard-svelte'
    import { BROWSER } from 'esm-env'

    export let isActive = false

    export let domNode: HTMLElement | undefined = undefined

    const dispatch = createEventDispatcher()

    function registerCloseObservers (): void {
        if (!BROWSER) return

        setTimeout(() => {
            if (typeof domNode !== 'undefined') {
                const matches = domNode.querySelectorAll('[data-modal-close]')
                if (matches.length > 0) {
                    matches.forEach((match) => { match.addEventListener('click', onClose, true) })
                }
            }
        }, 300)
    }

    function removeCloseObservers (): void {
        if (typeof domNode !== 'undefined') {
            const matches = domNode.querySelectorAll('[data-modal-close]')
            if (matches.length > 0) {
                matches.forEach((match) => { match.removeEventListener('click', onClose, true) })
            }
        }
    }

    onMount(() => {
        if (isActive) open()
    })

    export function open (): void {
        registerCloseObservers()
        dispatch('open')
    }

    export function updateCloseObservers (): void {
        removeCloseObservers()
        registerCloseObservers()
    }

    function onClose (e: any): void {
        isActive = false
        removeCloseObservers()
        dispatch('close')
    }
</script>

{#if isActive}
    <div bind:this={domNode} class="relative z-20" aria-labelledby="modal-title" role="dialog" aria-modal="true">
        <div transition:fade={{ duration: 300, easing: sineOut }} class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>

        <div class="fixed inset-0 z-20 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
                <div use:outsideClick on:clickOutside={onClose} transition:fade={{ duration: 300, easing: sineIn }} class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-3xl">

                    <div class="flex flex-row justify-end">
                        <a class="p-8 pb-0" on:click|preventDefault={onClose} href="#close" title="Close">
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-12 h-12 text-brand-400">
                                <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                            </svg>
                        </a>
                    </div>

                    <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                        <div class="sm:flex sm:items-start">
                            <slot />
                        </div>
                    </div>

                </div>
            </div>
        </div>

    </div>
{/if}
