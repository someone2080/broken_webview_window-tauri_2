<script lang="ts">
	import {availableMonitors, type Monitor, PhysicalPosition, type WindowOptions} from "@tauri-apps/api/window";
	import {onMount} from "svelte";
	import {WebviewWindow} from "@tauri-apps/api/webviewWindow";

	let available_monitors = $state<Monitor[]>([])
	let window_count = $state(0)

	onMount(async () => {
		available_monitors = await availableMonitors()
	});

	async function createNewWindow(position: PhysicalPosition) {
		const label = `sw-${Math.round(Math.random()*1000000)}-${++window_count}`

		const window_options: WindowOptions = {
			title: label,
			x: position.x, // broken in Tauri 2
			y: position.y, // broken in Tauri 2
		}

		let webview = new WebviewWindow(label, window_options)
		await webview.once('tauri://created', async function() {
			console.log('tauri://created');

			// position was reset to 0,0
			console.log(await webview.position())

			// 	we need to set position here again
			// await webview.setPosition(position)
		});
		await webview.once('tauri://error', function(e) {
			console.log('tauri://error', e);
		});
	}
</script>

{#each available_monitors as monitor}
	<div class="flex flex-col border w-fit p-4">
		<button class="text-blue-500" onclick={() => createNewWindow(monitor.position)}>Open window on this Display</button>
		<p>{monitor.name}</p>
		<p>Size: {monitor.size.height}x{monitor.size.width}</p>
		<p>Position: {monitor.position.y}x{monitor.position.x}</p>
	</div>
{/each}
