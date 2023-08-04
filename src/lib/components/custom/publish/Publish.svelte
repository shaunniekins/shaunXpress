<script>
	import { Icon } from 'svelte-ionicons';
	export let theme;
</script>

<div
	class={`flex items-center space-x-3 my-4 rounded-lg p-3 ${
		theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'
	}`}
>
	<img
		src="https://images.unsplash.com/photo-1470229722913-7c0e2dbbafd3?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80"
		alt="Author Picture"
		class="w-10 h-10 rounded-full"
	/>
	<form class="flex-grow">
		<textarea
			class={`w-full px-2 py-2 text-sm resize-none cursor-pointer  rounded-2xl ${
				theme === 'dark'
					? 'bg-[#3A3B3C] text-white hover:bg-gray-500'
					: 'bg-[#F0F2F5] text-black hover:bg-gray-200'
			}`}
			rows="1"
			placeholder="Thinkin' of?"
			disabled
		/>

		<!-- <div class="flex justify-between items-center">
			<Icon name="image-outline" width="20" class="cursor-pointer text-blue-700" />
			<button type="submit" class="px-4 py-1 bg-[#5E17EB] text-white rounded-2xl">Post</button>
		</div> -->
	</form>
	<Icon name="image-outline" width="20" class="cursor-pointer text-blue-700 outline-none" />
</div>
