<script>
	import Header from '../Header.svelte';
	import Loading from '../Loading.svelte';
	import logo from '$lib/images/zptx-logo.png';
	import { fly, fade } from 'svelte/transition';
	import team from '$lib/data/team.json';
	import { faDiscord, faGithub } from '@fortawesome/free-brands-svg-icons';
	import { faClipboardCheck } from '@fortawesome/free-solid-svg-icons';
	import { FontAwesomeIcon } from '@fortawesome/svelte-fontawesome';
	import { copy } from 'svelte-copy';

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
	const imagesToLoad = [logo, ...Object.values(team).map(member => member.image)];
	const createAndResolvePromises = async () => {
		await Promise.all(imagesToLoad.map(preloadImage));
		return reset();
	}

	$: copied = [];
	const copySucceeded = (target) => {
		if (copied.includes(target)) return;
		copied = [...copied, target];
		setTimeout(() => {
			copied = copied.filter(member => member !== target);
		}, 2000);
	};

	const roleConfig = {
		MANAGEMENT_TEAM: { label: 'management', color: 'bg-red-500/20 text-red-400 border-red-500/30' },
		DEVELOPMENT_TEAM: { label: 'developer', color: 'bg-blue-500/20 text-blue-400 border-blue-500/30' },
		LOCALIZATION_TEAM: { label: 'localization', color: 'bg-purple-500/20 text-purple-400 border-purple-500/30' }
	};
</script>

<svelte:head>
	<title>the team - ZPTX</title>
	<meta name="description" content="We're a small team of passionate developers dedicated to making intuitive, user-friendly, and accessible software for everyone." />
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
			<div class="absolute top-0 left-1/4 w-96 h-96 bg-purple-500/10 rounded-full blur-3xl"></div>
			<div class="max-w-4xl mx-auto text-center relative z-10">
				<h1 class="text-5xl md:text-6xl font-bold mb-6">
					<span class="text-gradient">the team</span>
				</h1>
				<p class="text-xl text-gray-400 max-w-2xl mx-auto">
					we're a small team of passionate developers dedicated to making intuitive, user-friendly, and accessible software for everyone.
				</p>
			</div>
		</div>

		<!-- Team members -->
		<div class="max-w-6xl mx-auto px-6 md:px-12 pb-24">
			{#each Object.keys(team) as member, i}
				<article
					class="glass-panel mb-8 overflow-hidden hover:border-zptx-orange/30 transition-all duration-500"
					in:fly|global={{ y: 30, duration: 600, delay: 100 * i }}
				>
					<div class="flex flex-col md:flex-row">
						<!-- Image section -->
						<div class="w-full md:w-80 shrink-0">
							<img
								class="w-full h-full object-cover object-center"
								src="{team[member].image}"
								alt="{team[member].name}'s Profile Picture"
							/>
						</div>

						<!-- Content section -->
						<div class="p-8 flex-1 flex flex-col justify-between">
							<!-- Top section -->
							<div>
								<div class="mb-4">
									<h2 class="text-3xl md:text-4xl font-bold mb-2">{team[member].name}</h2>
									<p class="text-zptx-orange-light font-mono text-lg">@{member}</p>
								</div>

								<p class="text-gray-400 text-lg leading-relaxed mb-6">
									{team[member].statement}
								</p>

								<!-- Roles -->
								<div class="flex flex-wrap gap-2 mb-6">
									{#each team[member].roles as role}
										<span class="px-4 py-1.5 rounded-full text-sm font-medium border {roleConfig[role].color}">
											{roleConfig[role].label}
										</span>
									{/each}
								</div>
							</div>

							<!-- Social links at bottom -->
							<div class="flex gap-4 text-2xl">
								<a
									href="{team[member].github}"
									class="glass-panel w-14 h-14 flex items-center justify-center hover:bg-white/10 hover:text-zptx-orange transition-all duration-300 no-underline"
									aria-label="GitHub profile"
								>
									<FontAwesomeIcon icon={faGithub} />
								</a>
								<button
									use:copy={team[member].discord}
									on:svelte-copy={() => copySucceeded(member)}
									class="glass-panel w-14 h-14 flex items-center justify-center hover:bg-white/10 hover:text-zptx-orange transition-all duration-300"
									aria-label="Copy Discord username"
								>
									{#if copied.includes(member)}
										<span in:fade={{ duration: 200 }}>
											<FontAwesomeIcon icon={faClipboardCheck} />
										</span>
									{:else}
										<span in:fade={{ duration: 200 }}>
											<FontAwesomeIcon icon={faDiscord} />
										</span>
									{/if}
								</button>
							</div>
						</div>
					</div>
				</article>
			{/each}
		</div>
	</div>
{/await}
