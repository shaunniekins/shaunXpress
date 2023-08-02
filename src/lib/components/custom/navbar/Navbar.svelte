<script>
	import { Icon } from 'svelte-ionicons';
	import { createEventDispatcher } from 'svelte';

	// Get a dispatcher to emit events
	const dispatch = createEventDispatcher();

	export let theme; // Receive the theme variable from +page.svelte

	function toggleTheme() {
		// Emit the 'theme-changed' event with the new theme value
		const newTheme = theme === 'light' ? 'dark' : 'light';
		dispatch('theme-changed', { theme: newTheme });
	}
</script>

<div class={`flex justify-between border-b border-gray-200 py-3 px-5 select-none`}>
	<div />
	<h1 class="text-[#5E17EB] font-bold">ShaunXpress</h1>

	<div class="cursor-pointer" on:click={() => toggleTheme()}>
		{#if theme === 'light'}
			<Icon name="moon-outline" class="text-black" width="20" />
		{:else}
			<Icon name="sunny-outline" class="text-white" width="20" />
		{/if}
	</div>
</div>
