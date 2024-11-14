<script lang="ts">
    import { sineIn, sineOut } from 'svelte/easing'
    import { outsideClick } from '@gozeltr/standard-svelte'

    export let panelTitle = ''
    export let isActive: boolean = false

    function onClickCloseButton() {
        isActive = false
    }

    function onClickOutside() {
        isActive = false
    }

    function slideIn(node: HTMLElement) {
        return {
            //delay: 100,
            duration: 500,
            easing: sineIn,
            css: (t: number, u: number) => `transform: translateX(${u * 100}%)`,
        }
    }

    function slideOut(node: HTMLElement) {
        return {
            //delay: 100,
            duration: 500,
            easing: sineOut,
            css: (t: number, u: number) => `transform: translateX(${u * 100}%)`,
        }
    }
</script>

<div
    class="relative z-50"
    aria-labelledby="slide-over-title"
    role="dialog"
    aria-modal="true"
>
    <!-- Background backdrop, show/hide based on slide-over state. -->
    <div
        class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
    ></div>

    <div class="fixed inset-0 overflow-hidden">
        <div class="absolute inset-0 overflow-hidden">
            <div
                class="pointer-events-none fixed inset-y-0 right-0 flex max-w-full pl-10 sm:pl-16"
            >
                <div
                    use:outsideClick
                    on:clickOutside={onClickOutside}
                    in:slideIn
                    out:slideOut
                    class="pointer-events-auto w-screen max-w-2xl"
                >
                    <div
                        class="flex h-full flex-col overflow-y-scroll bg-white py-6 shadow-xl"
                    >
                        <div class="px-4 sm:px-6">
                            <div class="flex items-start justify-between">
                                <h2
                                    class="text-lg font-medium text-gray-900"
                                    id="slide-over-title"
                                >
                                    {panelTitle}
                                </h2>
                                <div class="ml-3 flex h-7 items-center">
                                    <button
                                        on:click|preventDefault={onClickCloseButton}
                                        type="button"
                                        class="rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
                                    >
                                        <span class="sr-only">Close</span>
                                        <!-- Heroicon name: outline/x-mark -->
                                        <svg
                                            class="h-6 w-6"
                                            xmlns="http://www.w3.org/2000/svg"
                                            fill="none"
                                            viewBox="0 0 24 24"
                                            stroke-width="1.5"
                                            stroke="currentColor"
                                            aria-hidden="true"
                                        >
                                            <path
                                                stroke-linecap="round"
                                                stroke-linejoin="round"
                                                d="M6 18L18 6M6 6l12 12"
                                            />
                                        </svg>
                                    </button>
                                </div>
                            </div>
                        </div>
                        <div class="relative mt-6 flex-1 px-4 sm:px-6">
                            <slot />
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
