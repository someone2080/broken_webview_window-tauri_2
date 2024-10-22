<script lang="ts">
	import '../app.css';
	import {getCurrentWebviewWindow} from "@tauri-apps/api/webviewWindow";
	let { children } = $props();

	const app_window = getCurrentWebviewWindow()
</script>


<div class="bg-black/90 text-white h-[100vh] w-full flex flex-col gap-4 p-8">
	{#if app_window.label === "main"}
		{@render children()}
	{/if}
</div>
