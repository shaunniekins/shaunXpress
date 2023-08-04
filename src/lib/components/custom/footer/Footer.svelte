<script>
	export let theme;
</script>

<div
	class={`flex justify-center py-2 px-5 text-[#737373] font-medium text-xs select-none ${
		theme === 'dark' ? 'bg-[#242526] text-white' : 'bg-[#FFFFFF] text-black'
	}`}
>
	<ul class="flex gap-x-5">
		<li>
			<a href="/about">About</a>
		</li>
		<li>
			<a href="https://shaunniel.vercel.app/" target="_blank">Portfolio</a>
		</li>
		<li>Contact</li>
	</ul>
</div>
