<script>
	import { openB64 } from "./utils";
	const defaultMimes = ["application/pdf", "image/png"];
	const maxDisplayLength = 30;

	let value = "";
	let selectedMime = defaultMimes[0];
	let customMime = "";
	let isCustomMime = false;

	// data:[<mediatype>][;base64],<data>
	$: mime = isCustomMime ? customMime : selectedMime;
	$: prefix = `data:${mime};base64,`;
	$: b64url = prefix + value;
	$: urlDisplay =
	  b64url.length > maxDisplayLength
	    ? b64url.substring(0, maxDisplayLength) + "..."
	    : b64url;

	function open() {
	  prevURL = openB64(value, mime);
	}
</script>

<style>
</style>

<div>
	<div>
		<textarea bind:value={value} />
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
</div>