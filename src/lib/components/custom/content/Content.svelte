<script>
	import { Icon } from 'svelte-ionicons';
	import { onMount } from 'svelte';
	import postData from '../../../../data/data.json';

	let posts = [];
	let visiblePosts = 10; // Number of posts to show initially
	let increment = 10; // Increment value for each "Show more" click

	onMount(() => {
		posts = postData.posts.slice(0, visiblePosts); // Show only the first 10 posts initially
	});

	function showMore() {
		visiblePosts += increment;
		posts = postData.posts.slice(0, visiblePosts);
	}

	function copyPostToClipboard(postContent) {
		navigator.clipboard.writeText(postContent).then(
			() => {
				console.log('Post copied to clipboard.');
			},
			(err) => {
				console.error('Failed to copy post to clipboard: ', err);
			}
		);
	}
</script>

{#if posts.length === 0}
	<div
		class="flex flex-col my-5 px-5 mx-[34rem] h-[calc(100vh-3rem)] justify-center items-center text-2xl select-none"
	>
		<Icon name="sad-outline" width="50" height="50" />
		<p>No posts.</p>
	</div>
{:else}
	<div class="my-5 py-3 px-5 sm:mx-[26rem] xl:mx-[26rem] 2xl:mx-[34rem] h-fit select-none">
		{#each posts as post}
			<div class="flex items-start space-x-3 mb-6">
				<img src={post.author_picture} alt="Author Picture" class="w-12 h-12 rounded-full" />
				<div class="flex-1">
					<div class="flex justify-between">
						<div class="flex items-center gap-x-2">
							<h2 class="font-bold">{post.author}</h2>
							<p class="text-sm text-gray-500">|</p>
							<p class="text-sm text-gray-500">{post.date}</p>
						</div>
						<Icon
							name="copy-outline"
							width="15"
							class="cursor-pointer"
							on:click={() => copyPostToClipboard(post.post)}
						/>
					</div>
					{#if post.photo}
						<img src={post.photo} alt="Post Photo" class="w-full h-1/2 my-2 rounded-xl" />
					{/if}
					{#if post.post.length > 300}
						{#if !post.showFullText}
							<p class="text-md whitespace-pre-wrap select-text">{post.post.slice(0, 300)}...</p>
							<button on:click={() => (post.showFullText = true)} class="text-[#5E17EB]">
								Show full text
							</button>
						{:else}
							<p class="text-md whitespace-pre-wrap select-text">{post.post}</p>
							<button on:click={() => (post.showFullText = false)} class="text-[#5E17EB]">
								Show less
							</button>
						{/if}
					{:else}
						<p class="text-md whitespace-pre-wrap select-text">{post.post}</p>
					{/if}
				</div>
			</div>
		{/each}
	</div>
	{#if posts.length < postData.posts.length}
		<button
			on:click={showMore}
			class="flex justify-center mx-auto mb-10 px-4 py-2 rounded-xl bg-[#5E17EB] text-white select-none"
		>
			Show more
		</button>
	{/if}
{/if}
