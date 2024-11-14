<script type="ts">
    import { loadingBar } from '@gozel-core/standard-svelte'

    export let bgClasses = 'bg-emerald-500'
</script>

{#if $loadingBar}
    <div class="fixed z-40 h-1 w-full">
        <div class={`animate-loading-bar h-1 ${bgClasses}`}></div>
    </div>
{/if}
