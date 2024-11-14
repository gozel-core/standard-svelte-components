<script lang="ts">
    import { sineIn, sineOut } from 'svelte/easing'
    import { fly } from 'svelte/transition'
    import { notifications } from '@gozel-core/standard-svelte'
</script>

<div aria-live="assertive" class="pointer-events-none fixed inset-0 flex items-end px-4 py-6 sm:items-start sm:p-6">
    <div class="flex w-full flex-col items-center space-y-4 sm:items-end">
        {#each $notifications as notification}

            {#if notification.kind === 'success'}
                <div in:fly={{ y: -200, duration: 500, easing: sineIn }} out:fly={{ y: 200, duration: 500, easing: sineOut }} class="pointer-events-auto w-full max-w-sm overflow-hidden bg-white shadow-lg ring-1 ring-black ring-opacity-5">
                    <div class="p-4">
                        <div class="flex items-center">
                            <div class="flex-shrink-0">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-lime-600">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12.75l6 6 9-13.5" />
                                </svg>
                            </div>
                            <div class="ml-3 w-0 flex-1 pt-0.5">
                                <p class="text-sm font-medium text-gray-900">{notification.title}</p>
                                {#if typeof notification.message === 'string'}
                                    <p class="mt-1 text-sm text-gray-500">{notification.message}</p>
                                {/if}
                            </div>
                            <div class="ml-4 flex flex-shrink-0">
                                <button type="button" class="inline-flex rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-stone-500 focus:ring-offset-2">
                                    <span class="sr-only">Close</span>
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                                    </svg>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            {/if}

            {#if notification.kind === 'error'}
                <div in:fly={{ y: -200, duration: 500, easing: sineIn }} out:fly={{ y: 200, duration: 500, easing: sineOut }} class="pointer-events-auto w-full max-w-sm overflow-hidden bg-white shadow-lg ring-1 ring-black ring-opacity-5">
                    <div class="p-4">
                        <div class="flex items-center">
                            <div class="flex-shrink-0">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-red-600">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                                </svg>
                            </div>
                            <div class="ml-3 w-0 flex-1 pt-0.5">
                                <p class="text-sm font-medium text-gray-900">{notification.title}</p>
                                {#if typeof notification.message === 'string'}
                                    <p class="mt-1 text-sm text-gray-500">{notification.message}</p>
                                {/if}
                            </div>
                            <div class="ml-4 flex flex-shrink-0">
                                <button type="button" class="inline-flex rounded-md bg-white text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-stone-500 focus:ring-offset-2">
                                    <span class="sr-only">Close</span>
                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-4 h-4">
                                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                                    </svg>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            {/if}

        {/each}
    </div>
</div>
