<script>
	import { Icon } from 'svelte-ionicons';
	import { createEventDispatcher } from 'svelte';
	import { onMount, onDestroy } from 'svelte';

	export let theme;
	const dispatch = createEventDispatcher();

	let chosenPictures = [];

	function closeModal() {
		dispatch('closeModal');
	}

	function handleKeyPress(event) {
		if (event.key === 'Escape') {
			closeModal();
		}
	}

	onMount(() => {
		// Attach the event listener when the component is mounted
		document.addEventListener('keydown', handleKeyPress);
	});

	onDestroy(() => {
		// Remove the event listener when the component is unmounted
		document.removeEventListener('keydown', handleKeyPress);
	});

	async function handleImageIconClick() {
		try {
			const files = await getChosenFilesFromUser();
			chosenPictures = [...chosenPictures, ...files];
		} catch (error) {
			console.error('Error selecting images:', error);
		}
	}

	async function getChosenFilesFromUser() {
		return new Promise((resolve, reject) => {
			const input = document.createElement('input');
			input.type = 'file';
			input.accept = 'image/*';
			input.multiple = true;

			input.onchange = (event) => {
				const { files } = event.target;
				resolve(files);
			};

			input.onerror = (event) => {
				reject(event.error);
			};

			input.click();
		});
	}

	function removeImage(index) {
		chosenPictures = chosenPictures.filter((_, i) => i !== index);
	}
</script>

<div class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
	<div
		class={`w-[400px] rounded-xl ${theme === 'dark' ? 'bg-[#242526]' : 'bg-[#FFFFFF] text-black'}`}
	>
		<div class="px-4 py-2">
			<Icon
				name="close-outline"
				width="23"
				class="cursor-pointer outline-none text-blue-700"
				on:click={closeModal}
			/>
		</div>
		<div class={`border-t-2  ${theme === 'dark' ? 'border-gray-700' : 'border-gray-300'}`} />
		<div class="px-4 py-2">
			<div class="flex space-x-1">
				<img
					src="https://images.unsplash.com/photo-1470229722913-7c0e2dbbafd3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80"
					alt="Author Picture {1}"
					class="w-10 h-10 rounded-full"
				/>
				<div class="flex flex-1 flex-col">
					<form class="flex-grow space-y-3">
						<textarea
							class={`w-full px-2 py-2 mt-1 text-sm resize-none cursor-pointer outline-none rounded-2xl ${
								theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'
							}`}
							rows="3"
							placeholder="Thinkin' of?"
						/>
						<div class="flex flex-wrap overflow-y-auto max-h-56 space-y-6">
							{#each chosenPictures as picture, index}
								<div class="relative">
									<img
										src={URL.createObjectURL(picture)}
										alt="Chosen Picture {index}"
										class="w-25 h-25 mr-2"
									/>
									<Icon
										name="close-circle"
										width="20"
										class="absolute top-2 right-2 cursor-pointer outline-none text-red-500"
										on:click={() => removeImage(index)}
									/>
								</div>
							{/each}
						</div>
					</form>
					<div class="flex justify-between items-center">
						<div class="flex space-x-3">
							<Icon
								name="image-outline"
								width="20"
								class="cursor-pointer outline-none text-blue-700"
								on:click={handleImageIconClick}
							/>
							<Icon
								name="globe-outline"
								width="20"
								class="cursor-pointer outline-none text-blue-700"
							/>
						</div>
						<button class="bg-blue-700 text-white px-4 py-1 rounded-3xl" on:click={closeModal}
							>Post</button
						>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>
