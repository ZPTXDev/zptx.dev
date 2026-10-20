<script>
	import Header from '../Header.svelte';
	import Loading from '../Loading.svelte';
	import logo from '$lib/images/zptx-logo.png';
	import { fly } from 'svelte/transition';
	import contact from '$lib/data/contact.json';
	import { faDiscord } from '@fortawesome/free-brands-svg-icons';
	import { faEnvelope, faHeartPulse } from '@fortawesome/free-solid-svg-icons';
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
	const imagesToLoad = [logo];
	const createAndResolvePromises = async () => {
		await Promise.all(imagesToLoad.map(preloadImage));
		return reset();
	}

	const contactMethods = [
		{
			icon: faDiscord,
			label: 'Discord Server',
			description: 'Join our community',
			url: contact.discord,
			color: 'hover:bg-indigo-500/20 hover:border-indigo-500/50'
		},
		{
			icon: faEnvelope,
			label: 'Email Us',
			description: 'Get in touch directly',
			url: contact.email,
			color: 'hover:bg-zptx-orange/20 hover:border-zptx-orange/50'
		},
		{
			icon: faHeartPulse,
			label: 'Status Page',
			description: 'Check our services',
			url: contact.status,
			color: 'hover:bg-green-500/20 hover:border-green-500/50'
		}
	];
</script>

<svelte:head>
	<title>contact us - ZPTX</title>
	<meta name="description" content="Need to get in touch with us? Here's where you can find us." />
</svelte:head>

{#await createAndResolvePromises()}
	<Header {logo} />
	<div class="bg-zptx-darker min-h-[calc(100vh-5rem)] flex items-center justify-center">
		<Loading {elapsed} />
	</div>
{:then}
	<div class="min-h-screen bg-zptx-darker grid-background">
		<Header {logo} />

		<div class="relative overflow-hidden">
			<!-- Gradient orbs -->
			<div class="absolute top-1/4 right-1/3 w-96 h-96 bg-zptx-orange/10 rounded-full blur-3xl"></div>
			<div class="absolute bottom-1/4 left-1/3 w-96 h-96 bg-purple-500/10 rounded-full blur-3xl"></div>

			<div class="relative z-10 flex flex-col items-center justify-center min-h-[calc(100vh-6rem)] px-6 py-24">
				<!-- Header -->
				<div class="text-center max-w-3xl mb-16">
					<h1 class="text-5xl md:text-6xl font-bold mb-6">
						<span class="text-gradient">contact us</span>
					</h1>
					<p class="text-xl text-gray-400">
						need to get in touch with us? here's where you can find us.
					</p>
				</div>

				<!-- Contact cards -->
				<div class="grid md:grid-cols-3 gap-6 max-w-5xl w-full">
					{#each contactMethods as method, i}
						<a
							href={method.url}
							class="glass-panel p-8 text-center transition-all duration-500 border-2 {method.color} group"
							in:fly={{ y: 30, duration: 600, delay: 100 * i }}
						>
							<div class="text-5xl mb-4 transition-transform duration-300 group-hover:scale-110">
								<FontAwesomeIcon icon={method.icon} />
							</div>
							<h2 class="text-xl font-bold mb-2">{method.label}</h2>
							<p class="text-gray-400 text-sm">{method.description}</p>
						</a>
					{/each}
				</div>

				<!-- Additional info -->
				<div class="mt-16 text-center max-w-2xl">
					<p class="text-gray-500 text-sm">
						We typically respond within 24-48 hours. For urgent matters, Discord is your fastest option.
					</p>
				</div>
			</div>
		</div>
	</div>
{/await}