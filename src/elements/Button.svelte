<script lang="ts">
    export let state: 'active' | 'busy' = 'active'
    export let classes =
        'bg-black hover:bg-black text-white focus-visible:outline-brand-black'
    export let classesBusy =
        'bg-gray-100 text-gray-800 focus-visible:outline-gray-800'
    export let text = ''
    export let textBusy = ''
    export let onClick: Promise<any> | any | void = () => {}
</script>

{#if state === 'busy'}
    <button
        disabled
        type="submit"
        class={`block w-full cursor-progress rounded-md px-3.5 py-2.5 text-center text-base font-semibold shadow-sm focus-visible:outline-2 focus-visible:outline-offset-2 ${classesBusy}`}
    >
        {textBusy}
    </button>
{:else}
    <button
        on:click|preventDefault={onClick}
        type="submit"
        class={`block w-full rounded-md px-3.5 py-2.5 text-center text-base font-semibold shadow-sm focus-visible:outline-2 focus-visible:outline-offset-2 ${classes}`}
    >
        {text}
    </button>
{/if}
