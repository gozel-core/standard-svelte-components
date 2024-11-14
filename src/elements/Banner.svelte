<script lang="ts">
    import { _ } from 'svelte-i18n'

    export let isActive = false
    export let containerClasses = 'bg-black text-white'

    function onCloseClick() {
        disable()
    }

    function disable() {
        isActive = false
    }
</script>

{#if isActive}
    <div
        class={`relative isolate flex items-center justify-center gap-x-6 overflow-hidden px-6 py-2.5 ${containerClasses}`}
    >
        <p
            class="flex flex-1 items-center justify-center text-center text-lg leading-6"
        >
            <slot></slot>
        </p>
        <div class="flex justify-end">
            <button
                on:click={onCloseClick}
                type="button"
                class="-m-3 p-3 focus-visible:outline-offset-[-4px]"
            >
                <span class="sr-only">{$_('close')}</span>
                <svg
                    class="h-5 w-5"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                    aria-hidden="true"
                >
                    <path
                        d="M6.28 5.22a.75.75 0 00-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 101.06 1.06L10 11.06l3.72 3.72a.75.75 0 101.06-1.06L11.06 10l3.72-3.72a.75.75 0 00-1.06-1.06L10 8.94 6.28 5.22z"
                    />
                </svg>
            </button>
        </div>
    </div>
{/if}
