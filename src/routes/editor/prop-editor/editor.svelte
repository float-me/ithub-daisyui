<script lang="ts">
	import PropTag from './prop-tag.svelte';
	export let tagNames: string[];

	let value: string;
	let inputBind: HTMLInputElement;

	export function handleKeyDown(event: KeyboardEvent) {
		if (event.key === ' ') {
			event.preventDefault();
			if (!value) return;
			tagNames = [...tagNames, value];
			value = '';
		}
	}

	function handleOnSelect(event: CustomEvent<{ index: number }>) {
		tagNames.splice(event.detail.index, 1);
		tagNames = tagNames;
		inputBind.focus();
	}

	function init(el: HTMLInputElement) {
		el.focus();
	}
</script>

<div class="container">
	{#each tagNames as name, index}
		<PropTag {name} {index} on:select={handleOnSelect} />
	{/each}
</div>
<div class="container">
	<input
		class="input w-max"
		id="prop-editor-input"
		bind:value
		bind:this={inputBind}
		on:keydown
		use:init
	/>
</div>

<style>
	.container {
		display: flex;
		justify-content: center;
		flex-wrap: wrap; /* Allows tags to wrap to the next line if needed */
		gap: 10px; /* Adjust the spacing as needed */
	}
</style>
