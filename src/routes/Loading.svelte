<script>
	import { fade, fly } from 'svelte/transition';

	export let elapsed = 0;
	export let fullscreen = false;
</script>

<div class="transition-enforcement {fullscreen ? 'bg-zptx-darker min-w-screen min-h-screen' : ''} flex justify-center items-center font-mono text-2xl *:justify-self-center *:animate-pulse">
	{#if elapsed > 12000}
		<p in:fade={{ delay: 250 }} out:fade>
			something's not right...
		</p>
		{#if elapsed > 15000}
			<div class="absolute bottom-4 left-1/2 transform -translate-x-1/2 -translate-y-1/2 flex justify-center gap-2 !animate-none text-xl">
				<a href="https://status.zptx.dev" class="transition hover:text-white no-underline" in:fly={{ y: '20%' }} out:fade>system status</a>
				<p class="text-slate-600" in:fly={{ y: '20%' }} out:fade>/</p>
				<a href="https://go.zptx.dev/discord" class="transition hover:text-white no-underline" in:fly={{ y: '20%' }} out:fade>let us know</a>
			</div>
		{/if}
	{:else if elapsed > 7000}
		<p in:fade={{ delay: 250 }} out:fade>
			still loading...
		</p>
	{:else if elapsed > 3000}
		<p in:fade={{ delay: 250 }} out:fade>
			this is taking longer than expected
		</p>
	{:else}
		<p in:fade={{ delay: 100 }} out:fade>
			one moment please
		</p>
	{/if}
</div>

<style>
    .transition-enforcement {
        display: grid;
    }
    .transition-enforcement > * {
        grid-column: 1/2;
        grid-row: 1/2;
    }
</style>
