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
	class={`${theme === 'dark' ? 'bg-[#18191A]' : 'bg-[#F0F2F5]'}
${theme === 'dark' ? 'text-white' : 'text-black'}
`}
>
	<Navbar {theme} on:theme-changed={toggleTheme} />
	<div
		class="px-5 py-28 mx-[-0.30rem] sm:mx-[5rem] xl:mx-[26rem] 2xl:mx-[34rem] h-[calc(100vh-5rem)]"
	>
		<!-- <main class="p-6"> -->
		<h1 class="text-2xl font-bold mb-4">About</h1>
		<p class="text-gray-700 mb-6">
			Welcome to my fun site where I share my thoughts, rants, and whatever else comes to mind! This
			is a place for me to express myself and connect with others who might share similar interests.
			Remember, everything here is meant for fun only, so take it all with a grain of salt. Feel
			free to explore and leave your thoughts in the comments!
		</p>
		<!-- </main> -->
	</div>
	<Footer {theme} />
</div>
