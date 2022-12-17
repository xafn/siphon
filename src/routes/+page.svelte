<script lang="ts">
	import { onMount } from 'svelte';
	import { fly } from 'svelte/transition';
	import { cubicOut, quintOut } from 'svelte/easing';
	import type { Video } from '../util/types';
	import Svg from '../components/Svg.svelte';

	let trending: Video[];
	let country = 'US';
	onMount(async () => {
		const response = await fetch(`https://pipedapi.kavin.rocks/trending?region=${country}`);
		trending = await response.json();
		console.log(trending);
	});
</script>

<svelte:head>
	<title>Siphon</title>
	<meta name="description" content="Siphone Home" />
</svelte:head>
<main>
	<aside>
		<div class="title">
			<h2>Siphon</h2>
			<h6>{country}</h6>
		</div>
		<nav>
			<div class="nav selected-nav">
				<h5>Trending</h5>
			</div>
			<div class="nav">
				<h5>Search</h5>
			</div>
			<div class="nav">
				<h5>Feed</h5>
			</div>
			<div class="nav">
				<h5>History</h5>
			</div>
			<div class="nav">
				<h5>Settings</h5>
			</div>
		</nav>
	</aside>
	{#if trending}
		<section class="video-grid">
			{#each trending as video}
				<a class="video" href={video.url}>
					<div class="video-card">
						<div id="play-audio">
							<Svg viewBoxHeight={48} svgHeight={12}>
								<path
									d="M19.65 42q-3.15 0-5.325-2.175Q12.15 37.65 12.15 34.5q0-3.15 2.175-5.325Q16.5 27 19.65 27q1.4 0 2.525.4t1.975 1.1V6h11.7v6.75h-8.7V34.5q0 3.15-2.175 5.325Q22.8 42 19.65 42Z"
								/>
							</Svg>
						</div>
						<div id="timestamp">
							<h6>
								{new Date(video.duration * 60000).toISOString().substring(11, 16)}
							</h6>
						</div>
						<!-- gets the non shitty quality thumbnail straight from youtube, though i dont know how privacy invasive this is... -->
						<div
							id="thumbnail"
							style:background-image={`url(https://img.youtube.com/vi/${video.url.slice(
								9
							)}/maxresdefault.jpg)`}
						/>
					</div>
					<div id="info">
						<a href={video.uploaderUrl}>
							<img id="channel-icon" src={video.uploaderAvatar.replace(/s48/g, 'sm')} />
						</a>
						<div class="info-text">
							<div id="title"><h3>{video.title}</h3></div>
							<div id="bottom-info">
								<div id="channel">
									<a href={video.uploaderUrl}>
										<h6>{video.uploaderName}</h6>
									</a>
									{#if video.uploaderVerified}
										<Svg viewBoxHeight={48} svgHeight={14}>
											<path
												d="M21.05 33.1 35.2 18.95l-2.3-2.25-11.85 11.85-6-6-2.25 2.25ZM24 44q-4.1 0-7.75-1.575-3.65-1.575-6.375-4.3-2.725-2.725-4.3-6.375Q4 28.1 4 24q0-4.15 1.575-7.8 1.575-3.65 4.3-6.35 2.725-2.7 6.375-4.275Q19.9 4 24 4q4.15 0 7.8 1.575 3.65 1.575 6.35 4.275 2.7 2.7 4.275 6.35Q44 19.85 44 24q0 4.1-1.575 7.75-1.575 3.65-4.275 6.375t-6.35 4.3Q28.15 44 24 44Z"
											/>
										</Svg>
									{/if}
								</div>
								<h6>
									{Intl.NumberFormat('en', { notation: 'compact' }).format(video.views)} · {video.uploadedDate}
								</h6>
							</div>
						</div>
					</div>
				</a>
			{/each}
		</section>
	{/if}
</main>

<style>
	main {
		display: flex;
	}

	aside {
		background-color: var(--neutral-two);
		padding: 2rem;
		position: fixed;
		height: 100vh;
	}

	nav {
		margin-top: 1rem;
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.title {
		display: flex;
		gap: 0.25rem;
		margin-bottom: 2rem;
	}

	.nav {
		padding: 0.75rem 1rem;
		width: 12rem;
		border-radius: 100px;
		margin-left: -0.5rem;
	}

	.selected-nav {
		background-color: var(--yellow-opacity);
	}

	.selected-nav h5 {
		color: var(--white);
	}

	/* ---------- */

	a {
		text-decoration: none;
	}
	.video {
		margin-bottom: 2rem;
		width: 100%;
		cursor: pointer;
	}

	.video:hover #play-audio {
		opacity: 1;
	}

	.video-card {
		position: relative;
	}

	#thumbnail {
		background-size: cover;
		background-position: center;
		aspect-ratio: 16/9;
		width: 100%;
		border-radius: 8px;
		background-color: var(--neutral-one);
		object-fit: cover;
	}

	#timestamp {
		background-color: var(--neutral-five);
		width: max-content;
		padding: 0.25rem 0.5rem;
		border-radius: 4px;
		position: absolute;
		z-index: 1;
		bottom: 0.5rem;
		right: 0.5rem;
	}

	#timestamp h6 {
		color: var(--white);
	}

	#play-audio {
		fill: var(--white);
		background-color: var(--neutral-five);
		width: max-content;
		width: 24px;
		height: 24px;
		display: flex;
		align-items: center;
		justify-content: center;
		border-radius: 4px;
		position: absolute;
		top: 0.5rem;
		right: 0.5rem;
		transition: all 0.1s var(--bezier-one);
		opacity: 0;
	}

	.video-grid {
		padding: 2rem;
		display: grid;
		justify-items: center;
		gap: 1rem;
		width: min(100%, 125rem);
		margin-top: 0.25rem;
		margin-left: 16rem;
		grid-template-columns: repeat(auto-fit, minmax(min(100%/2, max(250px, 100%/7)), 1fr));
	}

	#info {
		margin-top: 1rem;
		display: flex;
		gap: 0.75rem;
	}

	#info {
		margin-top: 1rem;
		display: flex;
		gap: 0.75rem;
	}

	#title {
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-line-clamp: 2;
		line-clamp: 2;
		-webkit-box-orient: vertical;
	}

	#bottom-info {
		margin-top: 0.25rem;
	}

	#channel-icon {
		border-radius: 50%;
		height: 36px;
	}

	#channel path {
		fill: var(--white-two);
	}

	#channel {
		display: flex;
		gap: 0.25rem;
		overflow: hidden;
		text-overflow: ellipsis;
		-webkit-box-orient: vertical;
	}
</style>
