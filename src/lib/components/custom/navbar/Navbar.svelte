<script>
	import { Icon } from 'svelte-ionicons';
	import { createEventDispatcher } from 'svelte';
	import { onMount } from 'svelte';

	const dispatch = createEventDispatcher();

	export let theme;

	function toggleTheme() {
		const newTheme = theme === 'light' ? 'dark' : 'light';
		localStorage.setItem('theme', newTheme); // Save the theme in localStorage
		dispatch('theme-changed', { theme: newTheme });
	}

	let showLogin = false;
	onMount(() => {
		// Redirect to /home if the base URL is accessed
		if (window.location.pathname === '/admin/login') {
			showLogin = true;
		}
	});
</script>

{#if showLogin}
	<div
		class={`flex justify-end py-3 px-5 select-none shadow-sm ${
			theme === 'dark' ? 'bg-[#18191A] text-white' : 'bg-[#F0F2F5] text-black'
		}`}
	>
		<div class="cursor-pointer" on:click={() => toggleTheme()}>
			{#if theme === 'light'}
				<Icon name="moon-outline" class="text-black" width="20" />
			{:else}
				<Icon name="sunny-outline" class="text-white" width="20" />
			{/if}
		</div>
	</div>
{:else}
	<div
		class={`flex justify-between py-3 px-5 select-none shadow-sm ${
			theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'
		}`}
	>
		<div />
		<a href="/home">
			<h1 class="text-[#5E17EB] font-bold">ShaunXpress</h1>
		</a>

		<div class="cursor-pointer" on:click={() => toggleTheme()}>
			{#if theme === 'light'}
				<Icon name="moon-outline" class="text-black" width="20" />
			{:else}
				<Icon name="sunny-outline" class="text-white" width="20" />
			{/if}
		</div>
	</div>
{/if}
