<script context="module">
	import oldjson from '$lib/tils.json';
	/* import marked from 'marked/lib/marked.esm.js'; */
	import marked from '../../node_modules/marked/lib/marked.esm.js';
	import hljs from 'highlight.js';
	import 'highlight.js/styles/tomorrow-night-blue.css';

	marked.setOptions({
		highlight: function (code, lang) {
			const language = hljs.getLanguage(lang) ? lang : 'plaintext';
			return hljs.highlight(code, { language }).value;
		}
	});

	const tils = oldjson.map((x) => {
		let j = { ...x };
		j.html = marked(atob(x.content));
		j.content = atob(x.content);
		return j;
	});

	export function load({ error, status }) {
		return {
			props: {
				tils: tils
			}
		};
	}
</script>

<script>
	export let tils;
	import PickTil from '$lib/components/PickTil.svelte';
	let toggle = false;
	let chosen = '<h1> Hello World </h1>';
</script>

<div class="absolute w-full h-full overflow-hidden">
	<div class="absolute w-full h-full bg-gray-100 z-10">&ThinSpace;</div>

	<div
		class="absolute transition-all duration-300 {toggle
			? 'z-30 opacity-100 delay-500'
			: 'z-20 opacity-0 delay-200'} grid grid-cols-1 grid-rows-1 place-items-center w-full h-full "
	>
		<PickTil bind:chosen bind:toggle {tils} />
	</div>

	<div class="absolute z-20 grid grid-cols-1 grid-rows-1 place-items-center w-full h-full ">
		<div
			class="transition-all duration-200 prose prose-indigo mb-5 mt-3 prose-sm md:prose-lg col-span-1
      row-span-1 md:prose-md bg-gray-200 p-8 rounded-xl max-h-[80%] max-w-xs md:max-w-lg  lg:max-w-lg overflow-y-scroll no-scroll xl:prose-lg"
		>
			{@html chosen}
		</div>
	</div>

	<button
		class="absolute z-20 w-10 h-10 top-10 left-10 rounded-full bg-indigo-800 transition-all duration-1000 {toggle
			? 'scale-[1000]'
			: ''}"
	/>

	<button
		class="absolute text-indigo-800 opacity-0 z-50 w-10 h-10 top-10 left-10 rounded-full bg-gray-200 transition-all duration-1000 {toggle
			? '!opacity-100'
			: 'opacity-0'}"
		on:click={() => {
			toggle = !toggle;
		}}
	>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			class="h-10 w-10"
			fill="none"
			viewBox="0 0 24 24"
			stroke="currentColor"
		>
			<path
				stroke-linecap="round"
				stroke-linejoin="round"
				stroke-width="2"
				d="M6 18L18 6M6 6l12 12"
			/>
		</svg>
	</button>
</div>
