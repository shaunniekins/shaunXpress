<script>
	import { onMount } from 'svelte';
	import Navbar from '../../lib/components/custom/navbar/Navbar.svelte';
	import Content from '../../lib/components/custom/content/Content.svelte';
	import Footer from '../../lib/components/custom/footer/Footer.svelte';

	// Define the theme variable and set the default theme to 'light'
	let theme = 'light';

	function toggleTheme() {
		theme = theme === 'light' ? 'dark' : 'light';
		if (typeof localStorage !== 'undefined') {
			localStorage.setItem('theme', theme);
		}
	}

	onMount(() => {
		if (typeof localStorage !== 'undefined') {
			const storedTheme = localStorage.getItem('theme');
			theme = storedTheme || theme;
		}

		document.addEventListener('theme-changed', (event) => {
			theme = event.detail.theme;
			if (typeof localStorage !== 'undefined') {
				localStorage.setItem('theme', theme);
			}
		});
	});
</script>

<div
	class={`select-none ${theme === 'dark' ? 'bg-[#18191A] text-white' : 'bg-[#F0F2F5] text-black'}`}
>
	<Navbar {theme} on:theme-changed={toggleTheme} />
	<div
		class="px-5 py-28 mx-[-0.30rem] sm:mx-[5rem] xl:mx-[26rem] 2xl:mx-[34rem] h-screen md:h-[calc(100vh-5rem)]"
	>
		<h1 class={`text-2xl font-bold mb-4 `}>About</h1>
		<p class={`mb-6 ${theme === 'dark' ? 'text-gray-300' : 'text-gray-700'}`}>
			Welcome to my fun site where I share my thoughts, rants, and whatever else comes to mind! This
			is a place for me to express myself and connect with others who might share similar interests.
			Remember, everything here is meant for fun only, so take it all with a grain of salt. Feel
			free to explore and leave your thoughts by contacting me!
		</p>
	</div>
	<Footer {theme} />
</div>
