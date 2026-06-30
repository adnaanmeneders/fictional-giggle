<script>
	import '../app.css';
    import { ModeWatcher } from 'mode-watcher';
</script>

<svelte:head>
	<link rel="icon" href="/logo.png" type="image/png" />
</svelte:head>

<ModeWatcher defaultMode="dark" />

<slot></slot>
