<script>
	import { onMount } from 'svelte';
	import { onDestroy } from 'svelte';
	import { writable } from 'svelte/store';
	import emailjs from '@emailjs/browser';

	import Navbar from '../../lib/components/custom/navbar/Navbar.svelte';
	import Footer from '../../lib/components/custom/footer/Footer.svelte';

	let sending = false;
	let success = false;
	let error = false;
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

	async function sendEmail(e) {
		e.preventDefault();
		const form = e.target;
		sending = true;

		try {
			const result = await emailjs.sendForm(
				'service_536o1vo',
				'template_i89gzfg',
				form,
				'r_fDYFQtdYgX_4GyY'
			);
			form.elements.user_name.value = '';
			form.elements.user_message.value = '';
			success = true;
			error = false;
		} catch (e) {
			console.log('FAILED...', e);
			success = false;
			error = true;
		}

		sending = false;
	}

	let dots = writable('');
	let dotCount = 0;

	const interval = setInterval(() => {
		dots.update(() => {
			dotCount = (dotCount + 1) % 6;
			return Array(dotCount).fill('. ').join('');
		});
	}, 400);

	onMount(() => {
		// This is a callback that runs when the component is mounted.
		// The animation interval is already set above.
	});

	onDestroy(() => {
		// This is a callback that runs when the component is unmounted.
		clearInterval(interval); // Cleanup: Clear the animation interval.
	});
</script>

<div
	class={`select-none ${theme === 'dark' ? 'bg-[#18191A] text-white' : 'bg-[#F0F2F5] text-black'}`}
>
	<Navbar {theme} on:theme-changed={toggleTheme} />
	<div
		class="px-5 py-28 mx-[-0.30rem] sm:mx-[5rem] xl:mx-[26rem] 2xl:mx-[34rem] h-screen md:h-[calc(100vh-5rem)]"
	>
		<h1 class="text-2xl font-bold mb-4">Message me!</h1>
		<form class="flex flex-col" on:submit|preventDefault={sendEmail}>
			<input
				type="text"
				name="user_name"
				class={`w-full px-2 py-2 mt-1 text-sm resize-none outline-none rounded-xl mb-3 ${
					theme === 'dark'
						? 'bg-[#3A3B3C] text-white hover:bg-gray-500'
						: 'bg-gray-200 text-black hover:bg-gray-200'
				}`}
				placeholder="Name (Optional)"
			/>
			<textarea
				name="user_message"
				class={`w-full px-2 py-2 mt-1 text-sm resize-none outline-none rounded-xl mb-3 ${
					theme === 'dark'
						? 'bg-[#3A3B3C] text-white hover:bg-gray-500'
						: 'bg-gray-200 text-black hover:bg-gray-200'
				}`}
				rows="12"
				placeholder="Your message here..."
				required
			/>
			{#if sending}
				<div class="text-blue-700 text-lg font-extrabold">{$dots}</div>
				<!-- <div class="text-blue-700 text-sm">Sending...</div> -->
			{:else}
				<input
					type="submit"
					value="Send"
					class="bg-blue-700 text-white w-fit self-end px-4 py-1 rounded-3xl cursor-pointer"
				/>
			{/if}
		</form>
	</div>

	{#if success}
		<div
			class="fixed top-0 left-0 flex justify-center items-center w-full h-full bg-black bg-opacity-40"
		>
			<div
				class={`flex flex-col  p-4 rounded-2xl shadow text-center ${
					theme === 'dark' ? 'bg-[#18191A]' : 'bg-[#F0F2F5]'
				}`}
			>
				Message sent successfully!
				<button
					class="ml-2 mt-3 text-sm text-blue-700 hover:text-red-500"
					on:click={() => (success = false)}
				>
					Close
				</button>
			</div>
		</div>
	{/if}

	{#if error}
		<div
			class="fixed top-0 left-0 flex justify-center items-center w-full h-full bg-black bg-opacity-40"
		>
			<div
				class={`flex flex-col  p-4 rounded-2xl shadow text-center ${
					theme === 'dark' ? 'bg-[#18191A]' : 'bg-[#F0F2F5]'
				}`}
			>
				Failed to send message.
				<button
					class="ml-2 mt-3 text-sm text-blue-700 hover:text-red-500"
					on:click={() => (error = false)}
				>
					Close
				</button>
			</div>
		</div>
	{/if}
	<Footer {theme} />
</div>
