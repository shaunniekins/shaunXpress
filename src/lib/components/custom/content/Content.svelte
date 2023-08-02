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
</script>

{#if posts.length === 0}
	<div
		class="flex flex-col my-5 px-5 mx-[34rem] h-[calc(100vh-3rem)] justify-center items-center text-2xl"
	>
		<Icon name="sad-outline" width="50" height="50" />
		<p>No posts.</p>
	</div>
{:else}
	<div class="my-5 py-3 px-5 sm:mx-[26rem] xl:mx-[26rem] 2xl:mx-[34rem] h-fit">
		{#each posts as post}
			<div class="flex items-start space-x-3 mb-4">
				<img src={post.author_picture} alt="Author Picture" class="w-12 h-12 rounded-full" />
				<div class="flex-1">
					<h2 class="font-bold">{post.author}</h2>
					<p class="text-sm text-gray-500">{post.date}</p>
					{#if post.photo}
						<img src={post.photo} alt="Post Photo" class="w-12 h-12" />
					{/if}
					<p class="text-md">{post.post}</p>
				</div>
			</div>
		{/each}
	</div>
	{#if posts.length < postData.posts.length}
		<button
			on:click={showMore}
			class="flex justify-center mx-auto my-2 px-4 py-2 rounded-xl bg-blue-500 text-white"
		>
			Show more
		</button>
	{/if}
{/if}
