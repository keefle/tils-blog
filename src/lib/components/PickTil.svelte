<script>
	import Fuse from 'fuse.js';
	export let tils;
	export let toggle;
	export let chosen;

	function formatDate(datestr) {
		let date = new Date(datestr);
		return (
			('0' + date.getDate()).slice(-2) +
			'-' +
			('0' + date.getMonth()).slice(-2) +
			'-' +
			date.getFullYear()
		);
	}

	function truncate(hash, len) {
		return hash.substring(0, len);
	}

	const fuse = new Fuse(tils, {
		keys: ['title', 'category']
	});

	let input = '';
	let results = fuse.search(input == '' ? ' ' : input);

	$: results = fuse.search(input == '' ? ' ' : input);
</script>

<div class="w-full h-full grid grid-cols-1 grid-rows-5 p-5 gap-5 text-gray-300">
	<div class="col-span-1 row-span-1 flex flex-col place-self-center gap-5">
		<div class="font-black text-6xl ">TILs</div>
	</div>

	<div class="flex flex-col gap-2 col-span-1 justify-start items-start row-span-1 place-self-center">
		<label for="searchtil" class="font-bold">Term:</label>
		<input
			id="searchtil"
			placeholder="is there anything on ansible?"
			class="text-gray-800 ring ring-gray-100 focus:ring-blue-400 rounded p-2 outline-none focus:outline-none w-64 flex-grow"
			type="text"
			bind:value={input}
		/>
	</div>

	<div
		class="flex justify-center items-start w-full col-span-1 row-span-3 overflow-y-scroll no-scroll"
	>
		<table class="table-fixed w-11/12">
			<thead class="border-b-2 border-gray-400">
				<tr class="text-left">
					<th class="font-bold p-2"> Title </th>
					<th class="font-bold p-2"> Topic </th>
					<th class="font-bold p-2 hidden md:table-cell"> Date </th>
					<th class="font-bold p-2 hidden md:table-cell"> Commit </th>
				</tr>
			</thead>
			<tbody>
				{#each results as json}
					<tr
						on:click={() => {
							chosen = json.item.html;
							toggle = !toggle;
						}}
						class="hover:bg-indigo-900 transition-all"
					>
						<td class="border-gray-400 border-b-2 p-2 "> {json.item.title} </td>
						<td class="truncate border-gray-400 border-b-2 p-2">
							<div class="grid place-items-start">
								<div
									class="p-2 truncate
                       bg-purple-800 rounded-full"
								>
									{json.item.category}
								</div>
							</div></td
						>
						<td class="border-gray-400 border-b-2 p-2 truncate  md:table-cell hidden font-mono">
							{formatDate(json.item.date)}
						</td>
						<td class="border-gray-400 border-b-2 p-2 truncate  md:table-cell hidden font-mono">
							{truncate(json.item.commit, 8)}
						</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</div>
</div>
