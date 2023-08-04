<script>
	import { Icon } from 'svelte-ionicons';
	import { onMount } from 'svelte';
	import postData from '../../../../data/data.json';
	export let theme; // Add theme prop

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
		class="flex flex-col my-5 px-5 h-[calc(100vh-3rem)] justify-center items-center text-2xl select-none"
	>
		<Icon name="sad-outline" width="50" height="50" />
		<p>No posts.</p>
	</div>
{:else}
	<div class="select-none">
		{#each posts as post}
			<div
				class={`flex flex-col my-4 px-3 py-5 rounded-lg 
			${theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'}`}
			>
				<div class="flex flex-1 justify-between items-center">
					<div class="flex space-x-3 items-center mb-2">
						<img
							src={post.author === 1
								? 'https://images.unsplash.com/photo-1470229722913-7c0e2dbbafd3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80'
								: ''}
							alt="Picture of Author"
							class="w-10 h-10 rounded-full"
						/>
						<div class="flex justify-between">
							<div class="flex flex-col items-start">
								<h2 class="font-bold text-sm">
									{post.author === 1 ? 'Shaun' : `Author ${post.author}`}
								</h2>
								<p class="text-xs text-gray-500">{post.date}</p>
							</div>
						</div>
					</div>

					<Icon
						name="copy-outline"
						width="15"
						class="cursor-pointer outline-none"
						on:click={() => copyPostToClipboard(post.post)}
					/>
				</div>
				<div class="space-y-2">
					{#if post.photo}
						<img src={post.photo} alt="Post Photo" class="w-full h-1/2 my-2 rounded-xl" />
					{/if}
					{#if post.post.length > 300}
						{#if !post.showFullText}
							<p class="text-sm whitespace-pre-wrap select-text">{post.post.slice(0, 300)}...</p>
							<button on:click={() => (post.showFullText = true)} class="text-[#5E17EB] text-sm">
								Show full text
							</button>
						{:else}
							<p class="text-sm whitespace-pre-wrap select-text">{post.post}</p>
							<button on:click={() => (post.showFullText = false)} class="text-[#5E17EB] text-sm">
								Show less
							</button>
						{/if}
					{:else}
						<p class="text-sm whitespace-pre-wrap select-text">{post.post}</p>
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
