<script>
	import { fly } from 'svelte/transition';
	import { base } from '$app/paths';
	import { page } from '$app/stores';
	import { onMount } from 'svelte';

	export let logo;

	let shouldAnimate = false;

	onMount(() => {
		// Check if we came from the landing page via session storage
		const fromLanding = sessionStorage.getItem('fromLanding');
		shouldAnimate = fromLanding === 'true';

		// Clear the flag
		sessionStorage.removeItem('fromLanding');
	});
</script>

<header class="py-6 bg-zptx-darker/80 backdrop-blur-lg border-b border-white/5 sticky top-0 z-50">
	<nav class="max-w-7xl mx-auto px-6 md:px-12">
		<div class="flex items-center justify-between">
			<!-- Logo -->
			{#if shouldAnimate}
				<a href="{base}/" class="w-20 hover:opacity-80 transition-opacity duration-300 no-underline" in:fly={{ y: -20, duration: 600 }}>
					<img src={logo} alt="ZPTX Logo" class="w-full h-auto" />
				</a>
			{:else}
				<a href="{base}/" class="w-20 hover:opacity-80 transition-opacity duration-300 no-underline">
					<img src={logo} alt="ZPTX Logo" class="w-full h-auto" />
				</a>
			{/if}

			<!-- Desktop Navigation -->
			{#if shouldAnimate}
				<div class="hidden md:flex items-center gap-6 text-base font-mono" in:fly={{ y: -20, duration: 600, delay: 100 }}>
					<a
						href="{base}/team"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/team' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						the team
					</a>
					<span class="text-gray-600">●</span>
					<a
						href="{base}/projects"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/projects' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						our projects
					</a>
					<span class="text-gray-600">●</span>
					<a
						href="{base}/contact"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/contact' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						contact us
					</a>
				</div>
			{:else}
				<div class="hidden md:flex items-center gap-6 text-base font-mono">
					<a
						href="{base}/team"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/team' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						the team
					</a>
					<span class="text-gray-600">●</span>
					<a
						href="{base}/projects"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/projects' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						our projects
					</a>
					<span class="text-gray-600">●</span>
					<a
						href="{base}/contact"
						class="px-4 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/contact' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						contact us
					</a>
				</div>
			{/if}

			<!-- Mobile Navigation -->
			{#if shouldAnimate}
				<div class="md:hidden flex items-center gap-4 text-sm font-mono" in:fly={{ y: -20, duration: 600, delay: 100 }}>
					<a
						href="{base}/team"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/team' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						team
					</a>
					<a
						href="{base}/projects"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/projects' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						projects
					</a>
					<a
						href="{base}/contact"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/contact' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						contact
					</a>
				</div>
			{:else}
				<div class="md:hidden flex items-center gap-4 text-sm font-mono">
					<a
						href="{base}/team"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/team' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						team
					</a>
					<a
						href="{base}/projects"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/projects' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						projects
					</a>
					<a
						href="{base}/contact"
						class="px-3 py-2 rounded-lg transition-all duration-300 no-underline {$page.url.pathname === '/contact' ? 'text-zptx-orange bg-white/5' : 'hover:text-zptx-orange-light hover:bg-white/5'}"
						on:click={() => sessionStorage.setItem('fromLanding', 'false')}
					>
						contact
					</a>
				</div>
			{/if}
		</div>
	</nav>
</header>
