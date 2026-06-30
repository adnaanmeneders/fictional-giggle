<script lang="ts">
	import Footer from '$lib/layout/Footer.svelte';
	import Header from '$lib/layout/Header.svelte';
	import { onMount } from 'svelte';

	onMount(() => {
		// Prevent hosting on unauthorized domains (Domain Lock)
		const hostname = window.location.hostname;
		const isLocal = hostname === 'localhost' || hostname === '127.0.0.1' || hostname.startsWith('192.168.') || hostname.startsWith('10.');
		const isAllowedDomain = hostname === 'targetpriv.xyz' || hostname.endsWith('.targetpriv.xyz') || hostname.endsWith('.vercel.app') || hostname.endsWith('.pages.dev');
		if (!isLocal && !isAllowedDomain) {
			document.body.innerHTML = '<div style="background:#000;display:flex;align-items:center;justify-content:center;height:100vh;margin:0;"><img src="/logo.png" style="width:64px;height:64px;animation:pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;" /><style>@keyframes pulse { 0%, 100% { opacity: 0.2; transform: scale(0.92); } 50% { opacity: 0.85; transform: scale(1.08); } }</style></div>';
			return;
		}

		// Prevent context menu (right click)
		const preventDefault = (e: Event) => e.preventDefault();
		document.addEventListener('contextmenu', preventDefault);

		// Prevent devtools shortcuts
		const handleKeyDown = (e: KeyboardEvent) => {
			// F12
			if (e.key === 'F12' || e.keyCode === 123) {
				e.preventDefault();
				return false;
			}
			// Ctrl+Shift+I, Ctrl+Shift+J, Ctrl+Shift+C
			if (e.ctrlKey && e.shiftKey && (e.key === 'I' || e.key === 'i' || e.key === 'J' || e.key === 'j' || e.key === 'C' || e.key === 'c' || e.keyCode === 73 || e.keyCode === 74 || e.keyCode === 67)) {
				e.preventDefault();
				return false;
			}
			// Ctrl+U (View Source)
			if (e.ctrlKey && (e.key === 'U' || e.key === 'u' || e.keyCode === 85)) {
				e.preventDefault();
				return false;
			}
			// Ctrl+S (Save Page)
			if (e.ctrlKey && (e.key === 'S' || e.key === 's' || e.keyCode === 83)) {
				e.preventDefault();
				document.body.innerHTML = '<div style="background:#000;display:flex;align-items:center;justify-content:center;height:100vh;margin:0;"><img src="/logo.png" style="width:64px;height:64px;animation:pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;" /><style>@keyframes pulse { 0%, 100% { opacity: 0.2; transform: scale(0.92); } 50% { opacity: 0.85; transform: scale(1.08); } }</style></div>';
				return false;
			}
		};
		document.addEventListener('keydown', handleKeyDown);

		// Prevent saving via browser menu (wipe body on blur)
		const handleBlur = () => {
			document.body.innerHTML = '<div style="background:#000;display:flex;align-items:center;justify-content:center;height:100vh;margin:0;"><img src="/logo.png" style="width:64px;height:64px;animation:pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;" /><style>@keyframes pulse { 0%, 100% { opacity: 0.2; transform: scale(0.92); } 50% { opacity: 0.85; transform: scale(1.08); } }</style></div>';
		};
		const handleFocus = () => {
			if (document.body.innerHTML.includes('logo.png')) {
				window.location.reload();
			}
		};
		window.addEventListener('blur', handleBlur);
		window.addEventListener('focus', handleFocus);

		return () => {
			document.removeEventListener('contextmenu', preventDefault);
			document.removeEventListener('keydown', handleKeyDown);
			window.removeEventListener('blur', handleBlur);
			window.removeEventListener('focus', handleFocus);
		};
	});
</script>


<div class="min-h-screen bg-background font-sans antialiased relative" style="background: #000;">
	<!-- Global amber glow top -->
	<div class="fixed top-0 left-0 right-0 h-[800px] z-0 pointer-events-none">
		<div class="absolute left-1/2 -translate-x-1/2 -top-[200px] w-[2000px] h-[1000px] rounded-full" style="background: radial-gradient(ellipse at center, rgba(255,184,0,0.07) 0%, rgba(255,184,0,0.02) 40%, transparent 70%);"></div>
	</div>

	<Header />
	<div class="mx-auto flex-1 overflow-hidden relative z-[1]">
		<slot></slot>
	</div>
	<Footer />
</div>

<style>
	:global(body) {
		user-select: none !important;
		-webkit-user-select: none !important;
		-moz-user-select: none !important;
		-ms-user-select: none !important;
	}
</style>
