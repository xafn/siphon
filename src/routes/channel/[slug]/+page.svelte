<script>
	import { page } from '$app/stores';
	import { onMount } from 'svelte';
	console.log($page);

	let channel;
	onMount(async () => {
		const response = await fetch(`https://pipedapi.kavin.rocks/channel/${$page.params.slug}`);
		channel = await response.json();
		console.log(channel);
	});

    const format = Intl.NumberFormat('en', { notation: 'compact' });
</script>

{#if channel}
	<div id="channel">
		<!-- no goddamn clue why piped returns a shitty avatar quality, but you can just replace the '48' with 'm' for 'm'ax quality -->
		<img id="avatar" src={channel.avatarUrl.replace(/s48/g, 'sm')} />
		<div>
			<h2>{channel.name}</h2>
			<h6>{format.format(channel.subscriberCount)} subscribers</h6>
		</div>
	</div>
{/if}

<style>
    #avatar {
        height: 64px;
        border-radius: 50%;
    }

    #channel {
        display: flex;
        align-items: center;
        gap: 1rem;
    }
</style>