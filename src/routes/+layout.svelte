<script lang="ts">
	import '../app.css';
	import {getCurrentWebviewWindow} from "@tauri-apps/api/webviewWindow";
	import {onMount} from "svelte";
	let { children } = $props();

	const app_window = getCurrentWebviewWindow()

	onMount(async () => {
		// We need to maximize window here to make it visible, but it will flicker on the desired Display and jump back to the primary Display
		await app_window.maximize() // w/o this, window will remain hidden, or will blink on Primary Display
	})
</script>

<div class="bg-black/90 text-white h-[100vh] w-full flex flex-col gap-4 p-8">
	{#if app_window.label === "main"}
		{@render children()}
	{/if}
</div>
