<script>
	import { fade, fly } from 'svelte/transition';
	import { FontAwesomeIcon } from '@fortawesome/svelte-fontawesome';
	import { faGithub, faDiscord } from '@fortawesome/free-brands-svg-icons';
	import { config } from '@fortawesome/fontawesome-svg-core';
	import '@fortawesome/fontawesome-svg-core/styles.css';
	import Loading from './Loading.svelte';
	import logo from '$lib/images/zptx-logo.png';
	import { base } from '$app/paths';

	config.autoAddCss = false;

	const startTime = Date.now();
	$: elapsed = Date.now() - startTime;
	const interval = setInterval(() => {
		elapsed = Date.now() - startTime;
	}, 1000);
	const reset = () => {
		clearInterval(interval);
	};
	const preloadImage = (src) => {
		return new Promise((resolve) => {
			let img = new Image();
			img.onload = resolve;
			img.src = src;
		});
	};
	const imagesToLoad = [logo];
	const createAndResolvePromises = async () => {
		await Promise.all(imagesToLoad.map(preloadImage));
		return reset();
	}
</script>

<svelte:head>
	<title>ZPTX</title>
	<meta name="description" content="Building intuitive, user-friendly software for everyone" />
</svelte:head>

{#await createAndResolvePromises()}
	<Loading {elapsed} fullscreen={true} />
{:then}
	<div class="relative min-h-screen bg-zptx-darker overflow-hidden grid-background">
		<!-- Animated gradient orbs with various movements -->
		<div class="absolute top-0 -left-20 w-96 h-96 bg-zptx-orange/20 rounded-full blur-3xl animate-float-slow" in:fade={{ duration: 1000 }}></div>
		<div class="absolute top-1/4 right-1/4 w-80 h-80 bg-purple-500/15 rounded-full blur-3xl animate-float-reverse" style="animation-delay: -2s;" in:fade={{ duration: 1000, delay: 200 }}></div>
		<div class="absolute bottom-20 right-0 w-96 h-96 bg-blue-500/10 rounded-full blur-3xl animate-float-diagonal" style="animation-delay: -4s;" in:fade={{ duration: 1000, delay: 400 }}></div>
		<div class="absolute bottom-1/3 left-1/3 w-72 h-72 bg-zptx-orange/15 rounded-full blur-3xl animate-pulse-slow" in:fade={{ duration: 1000, delay: 600 }}></div>

		<div class="relative z-10 flex flex-col items-center justify-center min-h-screen px-6">
			<!-- Logo with glow effect -->
			<div class="relative mb-8" in:fade={{ duration: 800 }}>
				<div class="absolute inset-0 blur-2xl bg-zptx-orange/30 scale-110 animate-glow"></div>
				<img src={logo} alt="ZPTX Logo" class="relative w-64 md:w-96 animate-fade-in" />
			</div>

			<!-- Main content -->
			<div class="text-center max-w-4xl" in:fly={{ y: 30, duration: 800, delay: 200 }}>
				<h1 class="text-2xl md:text-3xl font-light mb-6 text-gray-400">
					building <span class="text-gradient font-semibold">intuitive</span>, <span class="text-gradient font-semibold">user-friendly</span> software
				</h1>

				<!-- Navigation links with modern styling -->
				<nav class="flex flex-wrap justify-center items-center gap-3 md:gap-6 mt-12 text-base md:text-lg font-mono">
					<a href="{base}/team" class="px-4 py-2 glass-panel hover:bg-white/10 transition-all duration-300 no-underline" on:click={() => sessionStorage.setItem('fromLanding', 'true')}>
						the team
					</a>
					<span class="text-zptx-orange">●</span>
					<a href="{base}/projects" class="px-4 py-2 glass-panel hover:bg-white/10 transition-all duration-300 no-underline" on:click={() => sessionStorage.setItem('fromLanding', 'true')}>
						our projects
					</a>
					<span class="text-zptx-orange">●</span>
					<a href="{base}/contact" class="px-4 py-2 glass-panel hover:bg-white/10 transition-all duration-300 no-underline" on:click={() => sessionStorage.setItem('fromLanding', 'true')}>
						contact us
					</a>
				</nav>
			</div>

			<!-- Social links with modern styling -->
			<div class="absolute bottom-12 flex gap-8 text-3xl" in:fly={{ y: 20, duration: 600, delay: 400 }}>
				<a
					href="https://github.com/ZPTXDev"
					class="glass-panel w-16 h-16 flex items-center justify-center hover:bg-white/10 hover:scale-110 transition-all duration-300 hover:glow-orange no-underline"
					aria-label="GitHub"
				>
					<FontAwesomeIcon icon={faGithub} />
				</a>
				<a
					href="https://go.zptx.dev/discord"
					class="glass-panel w-16 h-16 flex items-center justify-center hover:bg-white/10 hover:scale-110 transition-all duration-300 hover:glow-orange no-underline"
					aria-label="Discord"
				>
					<FontAwesomeIcon icon={faDiscord} />
				</a>
			</div>
		</div>
	</div>
{/await}
