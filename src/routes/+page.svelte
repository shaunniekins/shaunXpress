<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	onMount(() => {
		// Redirect to /home if the base URL is accessed
		if (window.location.pathname === '/') {
			goto('/home');
		}
	});
</script>
