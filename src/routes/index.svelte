<script>
	// Import the $app store from '@sveltejs/kit/app'
	import { goto } from '@sveltejs/kit/app';

	// Get the current route from the context object
	export async function load({ context }) {
		// Check if the current route is 'website_name/'
		if (context.url.pathname === '/') {
			// Redirect to 'website_name/home'
			goto('/home');
		}
	}
</script>
