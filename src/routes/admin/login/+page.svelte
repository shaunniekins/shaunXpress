<script>
	import { onMount } from 'svelte';
	import Navbar from '../../../lib/components/custom/navbar/Navbar.svelte';

	// Define the theme variable and set the default theme to 'light'
	let theme = 'light';

	function toggleTheme() {
		theme = theme === 'light' ? 'dark' : 'light';
		if (typeof localStorage !== 'undefined') {
			localStorage.setItem('theme', theme); // Save the theme in localStorage if available
		}
	}

	// Check if localStorage is available and get the stored theme
	onMount(() => {
		if (typeof localStorage !== 'undefined') {
			const storedTheme = localStorage.getItem('theme');
			theme = storedTheme || theme;
		}
	});
</script>

<Navbar {theme} on:theme-changed={toggleTheme} />

<div
	class={`flex justify-center items-center h-[calc(100vh-3rem)] select-none ${
		theme === 'dark' ? 'bg-[#18191A] text-white' : 'bg-[#F0F2F5] text-black'
	}`}
>
	<div class="relative">
		<div
			class="absolute inset-0 bg-gradient-to-r from-purple-100 to-purple-800 -rotate-12 ml-2 rounded-2xl z-0"
		/>

		<div
			class={`flex flex-col max-w-sm px-6 py-10 shadow z-12 h-fit rounded-2xl sm:w-[400px] drop-shadow-lg z-10 ${
				theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'
			}`}
		>
			<h5
				class={`text-2xl font-bold tracking-tight text-center ${
					theme === 'dark' ? 'text-white ' : 'text-black'
				}`}
			>
				LOGIN
			</h5>

			<form class="space-y-6" action="#" method="POST">
				<input type="hidden" name="remember" value="true" />
				<div class="flex flex-col mt-3">
					<input
						id="email-address"
						name="email"
						type="email"
						autocomplete="email"
						required
						placeholder="Email address"
						class={`mb-3 rounded-lg p-1.5 text-sm ${
							theme === 'dark'
								? 'bg-[#3A3B3C] text-white hover:bg-gray-500'
								: 'bg-[#F0F2F5] text-black hover:bg-gray-200'
						}`}
					/>
					<input
						id="password"
						name="password"
						type="password"
						autocomplete="current-password"
						required
						placeholder="Password"
						class={`mb-5 sm:mb-3 rounded-lg p-1.5 text-sm  ${
							theme === 'dark'
								? 'bg-[#3A3B3C] text-white hover:bg-gray-500'
								: 'bg-[#F0F2F5] text-black hover:bg-gray-200'
						}`}
					/>
					<button
						type="submit"
						class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-purple-800 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
					>
						<span class="absolute left-0 inset-y-0 flex items-center pl-3 sm:text-sm" />
						Log in
					</button>
				</div>
			</form>
		</div>
	</div>
</div>
