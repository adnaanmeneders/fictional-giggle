<script lang="ts">
	import CharacterImg from '$lib/imgs/character.png';
	import LogoImg from '$lib/imgs/logo.png';
</script>

<svelte:head>
	<title>404 - Page Not Found | TARGETPRIV</title>
</svelte:head>

<div class="min-h-screen flex flex-col items-center justify-center relative overflow-hidden" style="background: #000;">
	<!-- Grid background -->
	<div class="absolute inset-0 pointer-events-none" style="background: linear-gradient(to right, rgba(255,255,255,0.03) 1px, transparent 1px), linear-gradient(to bottom, rgba(255,255,255,0.02) 1px, transparent 1px); background-size: 40px 40px; -webkit-mask-image: radial-gradient(ellipse 60% 50% at 50% 50%, white, transparent); mask-image: radial-gradient(ellipse 60% 50% at 50% 50%, white, transparent);"></div>

	<!-- Amber glow -->
	<div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[800px] h-[800px] rounded-full pointer-events-none" style="background: radial-gradient(circle, rgba(255,184,0,0.06) 0%, transparent 60%);"></div>

	<!-- Scanline effect -->
	<div class="absolute inset-0 pointer-events-none opacity-[0.03]" style="background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(255,255,255,0.1) 2px, rgba(255,255,255,0.1) 4px);"></div>

	<!-- Main content -->
	<div class="relative z-10 text-center px-6">
		<!-- Glitch 404 text -->
		<div class="relative mb-8">
			<h1 class="text-[10rem] md:text-[14rem] font-black leading-none tracking-tighter select-none glitch-text" style="color: var(--color-amber); text-shadow: 0 0 80px rgba(255,184,0,0.3), 0 0 200px rgba(255,184,0,0.1);">
				404
			</h1>
			<!-- Glitch layers -->
			<span class="absolute inset-0 text-[10rem] md:text-[14rem] font-black leading-none tracking-tighter select-none glitch-layer-1 pointer-events-none" style="color: #ff5f57; opacity: 0.3;">
				404
			</span>
			<span class="absolute inset-0 text-[10rem] md:text-[14rem] font-black leading-none tracking-tighter select-none glitch-layer-2 pointer-events-none" style="color: #28c840; opacity: 0.3;">
				404
			</span>
		</div>

		<!-- Character (banana) floating -->
		<div class="relative mb-8">
			<img
				src={CharacterImg}
				alt="Lost Banana"
				class="mx-auto h-32 md:h-40 w-auto drop-shadow-2xl animate-float"
				style="filter: drop-shadow(0 0 20px rgba(255,184,0,0.2));"
			/>
		</div>

		<!-- Text -->
		<h2 class="text-2xl md:text-3xl font-bold text-white mb-3 tracking-tight">
			PAGE NOT FOUND
		</h2>
		<p class="text-gray-400 text-sm md:text-base mb-8 max-w-md mx-auto">
			Looks like this page got banned. The banana is confused too.
		</p>

		<!-- Buttons -->
		<div class="flex flex-col sm:flex-row items-center gap-4 justify-center">
			<a
				href="/"
				class="relative group overflow-hidden p-0.5 rounded-lg active:scale-100 hover:scale-105 transition-all duration-300" style="background: rgba(255,184,0,0.3);"
			>
				<span class="relative z-10 flex items-center gap-2 bg-gradient-to-t from-black/50 to-black px-8 py-3 rounded-md text-white text-sm font-semibold">
					<img src={LogoImg} alt="" class="size-5" />
					GO HOME
				</span>
				<div class="absolute -bottom-12 group-hover:-bottom-10 transition-all duration-200 left-1/2 -z-0 -translate-x-1/2 blur-xl size-14 rounded-full" style="background: #FFB800;"></div>
			</a>
			<a
				href="https://discord.gg/target"
				target="_blank"
				rel="noopener noreferrer"
				class="relative group overflow-hidden p-0.5 rounded-lg active:scale-100 hover:scale-105 transition-all duration-300" style="background: rgba(255,184,0,0.3);"
			>
				<span class="relative z-10 flex items-center gap-2 bg-gradient-to-t from-black/50 to-black px-8 py-3 rounded-md text-white text-sm font-semibold">
					<svg class="size-5" viewBox="0 0 24 24" fill="currentColor"><path d="M20.317 4.37a19.791 19.791 0 0 0-4.885-1.515.074.074 0 0 0-.079.037c-.21.375-.444.864-.608 1.25a18.27 18.27 0 0 0-5.487 0 12.64 12.64 0 0 0-.617-1.25.077.077 0 0 0-.079-.037A19.736 19.736 0 0 0 3.677 4.37a.07.07 0 0 0-.032.027C.533 9.046-.32 13.58.099 18.057a.082.082 0 0 0 .031.057 19.9 19.9 0 0 0 5.993 3.03.078.078 0 0 0 .084-.028c.462-.63.874-1.295 1.226-1.994a.076.076 0 0 0-.041-.106 13.107 13.107 0 0 1-1.872-.892.077.077 0 0 1-.008-.128 10.2 10.2 0 0 0 .372-.292.074.074 0 0 1 .077-.01c3.928 1.793 8.18 1.793 12.062 0a.074.074 0 0 1 .078.01c.12.098.246.198.373.292a.077.077 0 0 1-.006.127 12.299 12.299 0 0 1-1.873.892.077.077 0 0 0-.041.107c.36.698.772 1.362 1.225 1.993a.076.076 0 0 0 .084.028 19.839 19.839 0 0 0 6.002-3.03.077.077 0 0 0 .032-.054c.5-5.177-.838-9.674-3.549-13.66a.061.061 0 0 0-.031-.03zM8.02 15.33c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.956-2.419 2.157-2.419 1.21 0 2.176 1.095 2.157 2.42 0 1.333-.956 2.418-2.157 2.418zm7.975 0c-1.183 0-2.157-1.085-2.157-2.419 0-1.333.955-2.419 2.157-2.419 1.21 0 2.176 1.095 2.157 2.42 0 1.333-.946 2.418-2.157 2.418z"/></svg>
					DISCORD
				</span>
				<div class="absolute -bottom-12 group-hover:-bottom-10 transition-all duration-200 left-1/2 -z-0 -translate-x-1/2 blur-xl size-14 rounded-full" style="background: #FFB800;"></div>
			</a>
		</div>
	</div>

	<!-- Pixel border decoration at bottom -->
	<div class="absolute bottom-0 left-0 right-0 h-1" style="background: linear-gradient(to right, transparent, var(--color-amber), transparent);"></div>
</div>

<style>
	@keyframes glitch-1 {
		0%, 100% { clip-path: inset(0 0 95% 0); transform: translate(0); }
		10% { clip-path: inset(20% 0 60% 0); transform: translate(-3px, 2px); }
		20% { clip-path: inset(50% 0 30% 0); transform: translate(3px, -1px); }
		30% { clip-path: inset(10% 0 80% 0); transform: translate(-2px, 1px); }
		40% { clip-path: inset(70% 0 10% 0); transform: translate(2px, -2px); }
		50% { clip-path: inset(40% 0 40% 0); transform: translate(-1px, 2px); }
		60% { clip-path: inset(80% 0 5% 0); transform: translate(3px, 0); }
		70% { clip-path: inset(5% 0 70% 0); transform: translate(-3px, -1px); }
		80% { clip-path: inset(60% 0 20% 0); transform: translate(2px, 1px); }
		90% { clip-path: inset(30% 0 50% 0); transform: translate(-1px, -2px); }
	}

	@keyframes glitch-2 {
		0%, 100% { clip-path: inset(95% 0 0 0); transform: translate(0); }
		10% { clip-path: inset(60% 0 20% 0); transform: translate(3px, -2px); }
		20% { clip-path: inset(30% 0 50% 0); transform: translate(-3px, 1px); }
		30% { clip-path: inset(80% 0 10% 0); transform: translate(2px, -1px); }
		40% { clip-path: inset(10% 0 70% 0); transform: translate(-2px, 2px); }
		50% { clip-path: inset(40% 0 40% 0); transform: translate(1px, -2px); }
		60% { clip-path: inset(5% 0 80% 0); transform: translate(-3px, 0); }
		70% { clip-path: inset(70% 0 5% 0); transform: translate(3px, 1px); }
		80% { clip-path: inset(20% 0 60% 0); transform: translate(-2px, -1px); }
		90% { clip-path: inset(50% 0 30% 0); transform: translate(1px, 2px); }
	}

	.glitch-layer-1 {
		animation: glitch-1 3s infinite linear;
	}

	.glitch-layer-2 {
		animation: glitch-2 3s infinite linear;
	}

	.glitch-text {
		animation: glitch-main 4s infinite;
	}

	@keyframes glitch-main {
		0%, 95%, 100% { transform: translate(0); }
		96% { transform: translate(-3px, 1px); }
		97% { transform: translate(3px, -1px); }
		98% { transform: translate(-2px, -1px); }
		99% { transform: translate(2px, 1px); }
	}
</style>
