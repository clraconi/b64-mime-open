<script>
	import { openB64, testDataURL } from "./utils";
	import HistoryManager from "./HistoryManager";

	const defaultMimes = ["application/pdf", "image/png"];
	const maxDisplayLength = 30;

	let value = "";
	let selectedMime = defaultMimes[0];
	let customMime = "";
	let isCustomMime = false;

	$: if (value.startsWith("data:")) {
	  const groups = testDataURL(value);
	  if (groups) {
	    update(groups);
	  }
	  mime = isCustomMime ? customMime : selectedMime;
	}
	$: mime = isCustomMime ? customMime : selectedMime;
	// data:[<mediatype>][;base64],<data>
	$: prefix = `data:${mime};base64,`;
	$: b64url = prefix + value;
	$: urlDisplay =
	  b64url.length > maxDisplayLength
	    ? b64url.substring(0, maxDisplayLength) + "..."
	    : b64url;

	let histories = [];

	function handleHistoryDelete(item) {
	  histories = histories.filter(d => d != item);
	  URL.revokeObjectURL(item);
	}

	function open() {
	  const url = openB64(value, mime);
	  histories = [...histories, url];
	}

	function update(groups) {
	  value = groups.data;
	  isCustomMime = !defaultMimes.includes(groups.mime);
	  if (isCustomMime) {
	    customMime = groups.mime;
	  } else {
	    selectedMime = groups.mime;
	  }
	}
</script>

<style>
</style>

<div>
	<div>
		<textarea bind:value={value}/>
	</div>
	<div>
		<ul>
			<li>
					<label>
						<input type="radio" bind:group={isCustomMime} value={true}>
						Custom Mime: 
						<input type="text" bind:value={customMime}>
					</label>
				</li>
				<li>
				<label>
					<input type="radio" bind:group={isCustomMime} value={false}>
					<select bind:value={selectedMime}>
						{#each defaultMimes as mime, i}
							<option value={mime}>{mime}</option>
						{/each}
					</select>
				</label>
			</li>
		</ul>
		<p>{urlDisplay}</p>
	</div>
	<div>
		<button on:click={open}>Open in new tab</button>
	</div>

	<div>
		<HistoryManager histories={histories} onDelete={handleHistoryDelete}/>
	</div>
</div>