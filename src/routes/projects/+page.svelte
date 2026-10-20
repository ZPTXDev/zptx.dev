<script>
	import Header from '../Header.svelte';
	import Loading from '../Loading.svelte';
	import logo from '$lib/images/zptx-logo.png';
	import { fly } from 'svelte/transition';
	import projects from '$lib/data/projects.json';
	import { faDiscord, faGithub } from '@fortawesome/free-brands-svg-icons';
	import { FontAwesomeIcon } from '@fortawesome/svelte-fontawesome';

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
	const imagesToLoad = [logo, ...Object.values(projects).map(project => `/${project.image}`)];
	const createAndResolvePromises = async () => {
		await Promise.all(imagesToLoad.map(preloadImage));
		return reset();
	}
</script>

<svelte:head>
	<title>our projects - ZPTX</title>
	<meta name="description" content="A collection of projects we've worked on, are working on, or plan to work on in the future." />
</svelte:head>

{#await createAndResolvePromises()}
	<Header {logo} />
	<div class="bg-zptx-darker min-h-[calc(100vh-5rem)] flex items-center justify-center">
		<Loading {elapsed} />
	</div>
{:then}
	<div class="min-h-screen bg-zptx-darker grid-background">
		<Header {logo} />

		<!-- Hero section -->
		<div class="relative overflow-hidden py-24 px-6">
			<div class="absolute top-0 right-1/4 w-96 h-96 bg-zptx-orange/10 rounded-full blur-3xl"></div>
			<div class="max-w-4xl mx-auto text-center relative z-10">
				<h1 class="text-5xl md:text-6xl font-bold mb-6">
					<span class="text-gradient">our projects</span>
				</h1>
				<p class="text-xl text-gray-400 max-w-2xl mx-auto">
					a collection of projects we've worked on, are working on, or plan to work on in the future.
				</p>
			</div>
		</div>

		<!-- Projects -->
		<div class="max-w-6xl mx-auto px-6 md:px-12 pb-24">
			{#each Object.keys(projects) as project, i}
				<article
					class="glass-panel mb-8 overflow-hidden hover:border-zptx-orange/30 transition-all duration-500"
					in:fly|global={{ y: 30, duration: 600, delay: 100 * i }}
				>
					<div class="flex flex-col md:flex-row">
						<!-- Image section with better aspect ratio -->
						<div class="w-full md:w-96 shrink-0 bg-gradient-to-br from-zptx-purple to-zptx-dark flex items-center justify-center overflow-hidden">
							<img
								class="w-full h-full object-cover"
								src="/{projects[project].image}"
								alt="{projects[project].name}"
							/>
						</div>

						<!-- Content section -->
						<div class="p-8 flex-1 flex flex-col justify-between">
							<!-- Top section -->
							<div>
								<h2 class="text-3xl md:text-4xl font-bold mb-4">
									{projects[project].name}
								</h2>

								<p class="text-gray-400 text-lg leading-relaxed mb-6">
									{projects[project].description}
								</p>
							</div>

							<!-- Social links at bottom -->
							<div class="flex gap-4 text-2xl">
								<a
									href="{projects[project].github}"
									class="glass-panel w-14 h-14 flex items-center justify-center hover:bg-zptx-orange/20 hover:text-zptx-orange transition-all duration-300 no-underline"
									aria-label="View on GitHub"
								>
									<FontAwesomeIcon icon={faGithub} />
								</a>
								{#if projects[project].discord}
									<a
										href="{projects[project].discord}"
										class="glass-panel w-14 h-14 flex items-center justify-center hover:bg-zptx-orange/20 hover:text-zptx-orange transition-all duration-300 no-underline"
										aria-label="Join Discord"
									>
										<FontAwesomeIcon icon={faDiscord} />
									</a>
								{/if}
							</div>
						</div>
					</div>
				</article>
			{/each}
		</div>
	</div>
{/await}
