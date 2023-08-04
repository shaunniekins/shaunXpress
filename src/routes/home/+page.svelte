<script>
	import { onMount } from 'svelte';
	import Navbar from '../../lib/components/custom/navbar/Navbar.svelte';
	import Content from '../../lib/components/custom/content/Content.svelte';
	import Footer from '../../lib/components/custom/footer/Footer.svelte';

	// Define the theme variable and set the default theme to 'light'
	let theme = 'light';

	// Function to toggle the theme
	function toggleTheme() {
		theme = theme === 'light' ? 'dark' : 'light';
		if (typeof localStorage !== 'undefined') {
			localStorage.setItem('theme', theme); // Save the theme in localStorage if available
		}
	}

	// Listen for the 'theme-changed' event emitted by Navbar
	onMount(() => {
		if (typeof localStorage !== 'undefined') {
			const storedTheme = localStorage.getItem('theme');
			theme = storedTheme || theme;
		}

		document.addEventListener('theme-changed', (event) => {
			theme = event.detail.theme;
			if (typeof localStorage !== 'undefined') {
				localStorage.setItem('theme', theme); // Save the theme in localStorage when it changes from the Navbar
			}
		});
	});
</script>

<div
	class={` ${theme === 'dark' ? 'bg-[#18191A]' : 'bg-[#F0F2F5]'}
${theme === 'dark' ? 'text-white' : 'text-black'}
`}
>
	<Navbar {theme} on:theme-changed={toggleTheme} />
	<div class="px-5 sm:mx-[26rem] xl:mx-[26rem] 2xl:mx-[34rem] h-fit">
		<Content {theme} />
	</div>
	<Footer {theme} />
</div>
