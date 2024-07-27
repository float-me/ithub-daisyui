<script lang="ts">
	import WordEditor from './word-editor/editor.svelte';
	import PropEditor from './prop-editor/editor.svelte';
	import { page } from '$app/stores';

	interface PageState {
		defaultStr: string;
	}

	$: state = ($page as unknown as { state: PageState }).state;
	$: defaultStr = state.defaultStr;

	let showPropEditor = false;

	let wordEditor: WordEditor;
	let propEditor: PropEditor;
	function handleKeyDown(event: KeyboardEvent) {
		if (event.key === 'g' && event.ctrlKey) {
			event.preventDefault();
			console.log('Ctrl + G!');
			showPropEditor = !showPropEditor;
			return;
		}
		switch ((event.target as HTMLInputElement).id) {
			case 'word-editor-input':
				wordEditor.handleKeyDown(event);
				break;
			case 'prop-editor-input':
				propEditor.handleKeyDown(event);
				break;
		}
	}
</script>

<section class="layout">
	<div class="word-editor" style="width: {showPropEditor ? 70 : 100}%;">
		<WordEditor {defaultStr} on:keydown={handleKeyDown} bind:this={wordEditor} />
	</div>
	{#if showPropEditor}
		<div class="prop-editor">
			<PropEditor bind:this={propEditor} on:keydown={handleKeyDown} />
		</div>
	{/if}
</section>

<style>
	.layout {
		display: flex;
		gap: 8px;
	}

	.word-editor {
		float: left;
		grid-area: word-editor;
		border: 1px black solid;
		align-content: center;
	}
	.prop-editor {
		float: right;
		width: 30%;
		grid-area: prop-editor;
		border: 1px black solid;
	}
</style>
