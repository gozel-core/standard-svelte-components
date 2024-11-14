<script lang="ts">
    import { sineIn, sineOut } from 'svelte/easing'
    import { fade } from 'svelte/transition'
    import { createEventDispatcher } from 'svelte'
    import { prompt, outsideClick } from '@gozel-core/standard-svelte'

    const dispatch = createEventDispatcher()

    function onCancel (e: MouseEvent): void {
        prompt.hide(false)
        dispatch('canceled')
    }

    function onApprove (e: MouseEvent): void {
        prompt.hide(true)
        dispatch('approved')
    }

    $: buttonClassnames = 'inline-flex w-full justify-center rounded-md border border-transparent px-4 py-2 text-base font-medium text-white shadow-sm focus:outline-none focus:ring-2 focus:ring-offset-2 sm:col-start-2 sm:text-sm ' + (
        $prompt.kind === 'danger' ? 'bg-red-600 hover:bg-red-700 focus:ring-red-500' : 'bg-blue-600 hover:bg-blue-700 focus:ring-blue-500'
    )
</script>

{#if $prompt.active}

    <div class="relative z-20" aria-labelledby="modal-title" role="dialog" aria-modal="true">

        <div transition:fade={{ duration: 300, easing: sineOut }} class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"></div>

        <div class="fixed inset-0 z-10 overflow-y-auto">
            <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">

                <div use:outsideClick on:clickOutside={onCancel} transition:fade={{ duration: 300, easing: sineIn }} class="relative transform overflow-hidden rounded-lg bg-white px-4 pt-5 pb-4 text-left shadow-xl transition-all sm:my-8 sm:w-full sm:max-w-lg sm:p-6">
                    <div>
                        {#if $prompt.kind === 'success'}
                            <div class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-green-100">
                                <svg class="h-6 w-6 text-green-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M4.5 12.75l6 6 9-13.5" />
                                </svg>
                            </div>
                        {:else if $prompt.kind === 'danger'}
                            <div class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-red-100">
                                <svg class="w-6 h-6 text-red-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v3.75m-9.303 3.376c-.866 1.5.217 3.374 1.948 3.374h14.71c1.73 0 2.813-1.874 1.948-3.374L13.949 3.378c-.866-1.5-3.032-1.5-3.898 0L2.697 16.126zM12 15.75h.007v.008H12v-.008z" />
                                </svg>
                            </div>
                        {:else if $prompt.kind === 'info'}
                            <div class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-blue-100">
                                <svg class="w-6 h-6 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
                                </svg>
                            </div>
                        {:else}
                            <div class="mx-auto flex h-12 w-12 items-center justify-center rounded-full bg-blue-100">
                                <svg class="w-6 h-6 text-blue-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
                                </svg>
                            </div>
                        {/if}

                        <div class="mt-3 text-center sm:mt-5">
                            <h3 class="text-lg font-medium leading-6 text-gray-900" id="modal-title">
                                {$prompt.messages.title}
                            </h3>
                            <div class="mt-2">
                                <p class="text-sm text-gray-700">
                                    {$prompt.messages.description}
                                </p>
                            </div>
                        </div>
                    </div>
                    <div class="mt-5 sm:mt-6 sm:grid sm:grid-flow-row-dense sm:grid-cols-2 sm:gap-3">
                        {#if $prompt.kind === 'danger'}
                            <button on:click|preventDefault={onApprove} type="button" class={buttonClassnames}>{$prompt.messages.approve}</button>
                            <button on:click|preventDefault={onCancel} type="button" class="mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 sm:col-start-1 sm:mt-0 sm:text-sm">{$prompt.messages.cancel}</button>
                        {:else}
                            <button on:click|preventDefault={onCancel} type="button" class="col-span-2 mt-3 inline-flex w-full justify-center rounded-md border border-gray-300 bg-white px-4 py-2 text-base font-medium text-gray-700 shadow-sm hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 sm:col-start-1 sm:mt-0 sm:text-sm">{$prompt.messages.close}</button>
                        {/if}
                    </div>
                </div>

            </div>
        </div>
    </div>

{/if}
