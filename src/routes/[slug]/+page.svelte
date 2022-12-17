<script>
	import { page } from '$app/stores';
	import Svg from '../../components/Svg.svelte';
	import { onMount } from 'svelte';
	console.log($page);

	let video;
	onMount(async () => {
		const response = await fetch(
			`https://pipedapi.kavin.rocks/streams/${$page.url.search.slice(3)}`
		);
		video = await response.json();
		console.log(video);
	});

	const format = Intl.NumberFormat('en', { notation: 'compact' });
</script>

{#if video}
	<div class="page-container">
		<div class="watch-container">
			<video width="320" height="240" controls>
				<source src={video.videoStreams[0].url} type="video/mp4" />
			</video>
			<h2>{video.title}</h2>

			<div class="info">
				<div id="channel-info">
					<div id="channel-icon"><img id="channel-icon" src={video.uploaderAvatar} /></div>
					<div id="channel-text">
						<h5>{video.uploader}</h5>
						<h6>
							{format.format(video.uploaderSubscriberCount)}
							subscribers
						</h6>
					</div>
				</div>

				<div id="buttons">
					<div id="like-ratio">
						<div id="like">
							<Svg viewBoxHeight={24} svgHeight={20}>
								<path
									d="M1 21h4V9H1v12zm22-11c0-1.1-.9-2-2-2h-6.31l.95-4.57.03-.32c0-.41-.17-.79-.44-1.06L14.17 1 7.59 7.59C7.22 7.95 7 8.45 7 9v10c0 1.1.9 2 2 2h9c.83 0 1.54-.5 1.84-1.22l3.02-7.05c.09-.23.14-.47.14-.73v-2z"
								/>
							</Svg>
							<h6>{format.format(video.likes)}</h6>
						</div>
						<div id="dislike">
							<Svg viewBoxHeight={24} svgHeight={20}>
								<path
									d="M15 3H6c-.83 0-1.54.5-1.84 1.22l-3.02 7.05c-.09.23-.14.47-.14.73v2c0 1.1.9 2 2 2h6.31l-.95 4.57-.03.32c0 .41.17.79.44 1.06L9.83 23l6.59-6.59c.36-.36.58-.86.58-1.41V5c0-1.1-.9-2-2-2zm4 0v12h4V3h-4z"
								/>
							</Svg>
							<h6>{format.format(video.dislikes)}</h6>
						</div>
					</div>
					<div class="button">
						<Svg viewBoxHeight={24} svgHeight={20}>
							<path d="M5,20h14v-2H5V20z M19,9h-4V3H9v6H5l7,7L19,9z" />
						</Svg>
					</div>
					<div class="button">
						<Svg viewBoxHeight={48} svgHeight={20}>
							<path
								d="M19.65 42q-3.15 0-5.325-2.175Q12.15 37.65 12.15 34.5q0-3.15 2.175-5.325Q16.5 27 19.65 27q1.4 0 2.525.4t1.975 1.1V6h11.7v6.75h-8.7V34.5q0 3.15-2.175 5.325Q22.8 42 19.65 42Z"
							/>
						</Svg>
					</div>
					<div class="button">
						<Svg viewBoxHeight={24} svgHeight={20}>
							<path
								d="M18 16.08c-.76 0-1.44.3-1.96.77L8.91 12.7c.05-.23.09-.46.09-.7s-.04-.47-.09-.7l7.05-4.11c.54.5 1.25.81 2.04.81 1.66 0 3-1.34 3-3s-1.34-3-3-3-3 1.34-3 3c0 .24.04.47.09.7L8.04 9.81C7.5 9.31 6.79 9 6 9c-1.66 0-3 1.34-3 3s1.34 3 3 3c.79 0 1.5-.31 2.04-.81l7.12 4.16c-.05.21-.08.43-.08.65 0 1.61 1.31 2.92 2.92 2.92 1.61 0 2.92-1.31 2.92-2.92s-1.31-2.92-2.92-2.92z"
							/>
						</Svg>
					</div>
				</div>
			</div>
			<div id="description">
				<div id="top-description">
					<h5>
						{video.views.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')} views ·
						{new Date(video.uploadDate).toString().slice(4, -42)}
					</h5>
				</div>
				<div id="description-text">
					{#if video.description}
						<p>{video.description}</p>
					{:else}
						<p>No description provided.</p>
					{/if}
				</div>
			</div>
		</div>
		<aside>
			{#each video.relatedStreams as video}
				<a class="video" href={video.url} target="_self">
					<div class="video-card">
						<div id="play-audio">
							<Svg viewBoxHeight={48} svgHeight={12}>
								<path
									d="M19.65 42q-3.15 0-5.325-2.175Q12.15 37.65 12.15 34.5q0-3.15 2.175-5.325Q16.5 27 19.65 27q1.4 0 2.525.4t1.975 1.1V6h11.7v6.75h-8.7V34.5q0 3.15-2.175 5.325Q22.8 42 19.65 42Z"
								/>
							</Svg>
						</div>
						<div
							id="thumbnail"
							style:background-image={`url(https://img.youtube.com/vi/${video.url.slice(
								9
							)}/maxresdefault.jpg)`}
						/>
					</div>
					<div id="info">
						<img id="channel-icon" src={video.uploaderAvatar} />
						<div class="info-text">
							<div id="title"><h3>{video.title}</h3></div>
							<div id="bottom-info">
								<div id="channel">
									<h6>{video.uploaderName}</h6>
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
		</aside>
	</div>
{/if}

<style>
	.page-container {
		display: flex;
	}

	video:focus {
		outline: none;
	}

	video {
        width: 100%;
		height: min-content;

        max-height: 50rem;
		border-radius: 12px;
		margin-bottom: 1rem;
	}

	.watch-container {
		width:120rem;
		padding: 2rem;
	}

    aside {
        padding: 1rem;
    }

	.info {
		display: flex;
		justify-content: space-between;
		margin-top: 0.75rem;
		margin-bottom: 1.75rem;
	}

	#channel-info {
		display: flex;
		gap: 1rem;
		align-items: center;
	}
	#channel-icon {
		border-radius: 50%;
		height: 44px;
	}

	#like-ratio {
		display: flex;
		gap: 0.25rem;
		width: max-content;
		border-radius: 1000px;
		overflow: hidden;
	}

	#like,
	#dislike {
		border-radius: 4px !important;
	}

	#like,
	#dislike,
	.button {
		background-color: var(--neutral-one);
		fill: var(--white-two);
		border-radius: 1000px;
		padding: 0.9rem 1.1rem;
		display: flex;
		align-items: center;
		gap: 0.5rem;
	}

	#buttons {
		display: flex;
		gap: 0.75rem;
	}

	#description {
		overflow-wrap: break-word;
		border-radius: 16px;
		overflow: hidden;
	}

	#top-description {
		background-color: var(--neutral-four);
		padding: 1rem;
	}

	#description-text {
		padding: 1rem;
		background-color: var(--neutral-one);
	}


    /* to do */

    a {
		text-decoration: none;
	}
	.video {
		margin-bottom: 2rem;
		width: 100%;
		cursor: pointer;
        display: flex;
        flex-direction: row;
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
		width: 100%;
		margin-left: 16rem;
		margin-top: 0.25rem;
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
