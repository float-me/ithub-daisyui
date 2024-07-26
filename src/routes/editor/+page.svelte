<script lang="ts">
	import WordEditor from './word-editor.svelte';
	import PropEditor from './prop-editor.svelte';
	import type { WordGraph } from '../../lib/graph';
	import { page } from '$app/stores';

	interface PageState {
		wordGraph: WordGraph;
		defaultStr: string;
	}

	$: state = ($page as unknown as { state: PageState }).state;
	$: wordGraph = state.wordGraph;
	$: defaultStr = state.defaultStr;

	let showPropEditor = false;

	let wordEditor: WordEditor;
	function handleKeyDown(event: KeyboardEvent) {
		if (event.key === 'g' && event.ctrlKey) {
			event.preventDefault();
			console.log('Ctrl + G!');
			showPropEditor = !showPropEditor;
			return;
		}
		wordEditor.handleKeyDown(event);
	}
</script>

<section class="layout">
	<div class="word-editor" style="width: {showPropEditor ? 70 : 100}%;">
		<WordEditor {wordGraph} {defaultStr} on:keydown={handleKeyDown} bind:this={wordEditor} />
	</div>
	{#if showPropEditor}
		<div class="prop-editor"><PropEditor /></div>
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
