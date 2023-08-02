<script>
	import { onMount } from 'svelte';
	import Navbar from './../lib/components/custom/navbar/Navbar.svelte';
	import Content from '../lib/components/custom/content/Content.svelte';
	import Footer from '../lib/components/custom/footer/Footer.svelte';

	// Define the theme variable and set the default theme to 'light'
	let theme = 'light';

	// Function to toggle the theme
	function toggleTheme() {
		theme = theme === 'light' ? 'dark' : 'light';
	}

	// Listen for the 'theme-changed' event emitted by Navbar
	onMount(() => {
		document.addEventListener('theme-changed', (event) => {
			theme = event.detail.theme;
		});
	});
</script>

<div
	class={` ${theme === 'dark' ? 'bg-[#1A202C]' : 'bg-white-100'}
${theme === 'dark' ? 'text-white' : 'text-black'}
`}
>
	<Navbar {theme} on:theme-changed={toggleTheme} />
	<Content {theme} />
	<Footer {theme} />
</div>
