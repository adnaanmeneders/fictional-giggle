<script lang="ts">
	import { ArrowRightIcon } from 'lucide-svelte';
	import AnimatedShinyText from '$lib/components/magic/AnimatedShinyText/AnimatedShinyText.svelte';
	import BorderBeam from '$lib/components/magic/borderbeam/BorderBeam.svelte';
	import type { ObserverEventDetails } from 'svelte-inview';
	import { inview } from 'svelte-inview';
	import BlurFade from '$lib/components/magic/BlurFade.svelte';
	import Sparkles from '$lib/components/magic/Sparkles.svelte';
	import AnimatedGradientText from '$lib/components/magic/AnimatedGradientText.svelte';

	// Images
	import WhCheatImg from '$lib/imgs/whcheat.png';
	import WnoCheatImg from '$lib/imgs/wnocheat.png';

	let inView = false;
	const handleChange = ({ detail }: CustomEvent<ObserverEventDetails>) => {
		inView = detail.inView;
	};

	// Slider state
	let sliderPos = 50;
	let isDragging = false;
	let containerEl: HTMLDivElement;

	let sliderMouseX = 0;
	let sliderMouseY = 0;

	function handleSliderMouseMove(e: MouseEvent) {
		if (!containerEl) return;
		const rect = containerEl.getBoundingClientRect();
		sliderMouseX = e.clientX - rect.left;
		sliderMouseY = e.clientY - rect.top;
	}

	function updateSlider(clientX: number) {
		if (!containerEl) return;
		const rect = containerEl.getBoundingClientRect();
		let pos = ((clientX - rect.left) / rect.width) * 100;
		pos = Math.max(0, Math.min(100, pos));
		sliderPos = pos;
	}

	function handlePointerDown(e: PointerEvent) {
		isDragging = true;
		updateSlider(e.clientX);
		(e.target as HTMLElement)?.setPointerCapture?.(e.pointerId);
	}

	function handlePointerMove(e: PointerEvent) {
		if (!isDragging) return;
		e.preventDefault();
		updateSlider(e.clientX);
	}

	function handlePointerUp() {
		isDragging = false;
	}
</script>

<section id="hero" class="relative mx-auto mt-24 max-w-7xl px-6 text-center md:mt-32 md:px-8">
	<!-- Recommended badge -->
	<BlurFade delay={0.15} once={true}>
		<AnimatedGradientText class="hover:cursor-pointer">
			<span class="inline-block size-1.5 rounded-full mr-2" style="background: var(--color-amber);"></span>
			<span class="inline animate-gradient bg-gradient-to-r from-[#FFB800] via-[#FFFFFF] to-[#FFB800] bg-[length:var(--bg-size)_100%] bg-clip-text text-transparent text-xs tracking-wider font-semibold uppercase">
				Recommended by players
			</span>
			<ArrowRightIcon
				class="ml-1 size-3 transition-transform duration-300 ease-in-out group-hover:translate-x-0.5 text-gray-400"
			/>
		</AnimatedGradientText>
	</BlurFade>

	<!-- Main heading -->
	<BlurFade delay={0.25} once={true}>
		<h1
			class="py-6 text-6xl font-black leading-none tracking-tighter text-white sm:text-7xl md:text-8xl lg:text-9xl"
			style="text-shadow: 0 0 80px rgba(255,255,255,0.1);"
		>
			TARGET.
		</h1>
	</BlurFade>

	<!-- Description -->
	<BlurFade delay={0.35} once={true}>
		<p
			class="mb-12 text-balance text-base tracking-tight text-gray-400 md:text-lg"
		>
			Experience the industry's most sophisticated private software. Built for security,
			<br class="hidden md:block" />
			designed for performance, and maintained by experts.
		</p>
	</BlurFade>

	<!-- Hero comparison slider -->
	<BlurFade delay={0.45} once={true}>
		<div
			use:inview={{
				unobserveOnEnter: true,
				rootMargin: '-100px'
			}}
			on:inview_change={handleChange}
			class="relative mt-20 [perspective:2000px] after:absolute after:inset-0 after:z-50 after:[background:linear-gradient(to_top,hsl(var(--background))_10%,transparent)] after:pointer-events-none"
		>
			<div
				class="rounded-xl border border-white/10 bg-white bg-opacity-[0.01] overflow-hidden before:absolute before:bottom-1/2 before:left-0 before:top-0 before:size-full before:opacity-0 before:[background-image:linear-gradient(to_bottom,var(--color-one),var(--color-one),transparent_40%)] before:[filter:blur(180px)] {inView
					? 'before:animate-image-glow'
					: ''}"
			>
				<BorderBeam
					size={200}
				duration={12}
				delay={0}
				colorFrom="var(--color-one)"
				colorTo="var(--color-two)"
			/>

			<!-- Before/After Comparison Slider -->
			<div
				bind:this={containerEl}
				class="relative select-none touch-none cursor-col-resize group overflow-hidden"
				on:pointerdown={handlePointerDown}
				on:pointermove={handlePointerMove}
				on:pointerup={handlePointerUp}
				on:pointerleave={handlePointerUp}
				on:mousemove={handleSliderMouseMove}
				style="--mouse-x: {sliderMouseX}px; --mouse-y: {sliderMouseY}px;"
				role="slider"
				aria-valuenow={Math.round(sliderPos)}
				aria-valuemin={0}
				aria-valuemax={100}
				aria-label="Image comparison slider"
				tabindex="0"
			>
				<!-- Bottom layer: Without Cheat (full width) -->
				<img
					src={WnoCheatImg}
					alt="Without Cheat"
					class="relative block w-full object-cover"
					draggable="false"
				/>

				<!-- Top layer: With Cheat (clipped by slider position) -->
				<div
					class="absolute inset-0 overflow-hidden"
					style="width: {sliderPos}%;"
				>
					<img
						src={WhCheatImg}
						alt="With Cheat"
						class="block h-full object-cover"
						style="width: {containerEl ? containerEl.offsetWidth + 'px' : '100vw'}; max-width: none;"
						draggable="false"
					/>
				</div>

				<!-- Local squares grid lines overlay -->
				<div 
					class="absolute inset-0 z-10 pointer-events-none mix-blend-screen opacity-20 transition-opacity duration-300 group-hover:opacity-40"
					style="
						background-image: 
							linear-gradient(to right, rgba(255,255,255,0.06) 1px, transparent 1px), 
							linear-gradient(to bottom, rgba(255,255,255,0.04) 1px, transparent 1px);
						background-size: 40px 40px;
					"
				/>

				<!-- Floating white sparkles overlay inside the image frame -->
				<div class="absolute inset-0 z-20 pointer-events-none opacity-60 overflow-hidden">
					<Sparkles
						minSize={0.4}
						maxSize={1.4}
						particleDensity={700}
						particleColor="#FFFFFF"
						speed={0.7}
					/>
				</div>

				<!-- Snowflake Sparkle Dots Scattered inside the comparison slider -->
				<div class="absolute inset-0 z-20 pointer-events-none overflow-hidden">
					<div class="sparkle-dot" style="left:10%;top:15%;animation-delay:0s;"></div>
					<div class="sparkle-dot" style="left:25%;top:8%;animation-delay:1.2s;"></div>
					<div class="sparkle-dot" style="left:40%;top:22%;animation-delay:0.5s;"></div>
					<div class="sparkle-dot" style="left:55%;top:12%;animation-delay:2.1s;"></div>
					<div class="sparkle-dot" style="left:70%;top:18%;animation-delay:0.8s;"></div>
					<div class="sparkle-dot" style="left:85%;top:10%;animation-delay:1.7s;"></div>
					<div class="sparkle-dot" style="left:15%;top:35%;animation-delay:3.1s;"></div>
					<div class="sparkle-dot" style="left:50%;top:40%;animation-delay:2.5s;"></div>
					<div class="sparkle-dot" style="left:80%;top:32%;animation-delay:0.3s;"></div>
					<div class="sparkle-dot" style="left:35%;top:5%;animation-delay:1.9s;"></div>
					<div class="sparkle-dot" style="left:65%;top:28%;animation-delay:4.0s;"></div>
					<div class="sparkle-dot" style="left:90%;top:25%;animation-delay:2.8s;"></div>
				</div>

				<!-- Labels -->
				<div class="absolute left-4 bottom-4 z-20 pointer-events-none select-none">
					<span class="rounded bg-black/60 px-2.5 py-1 text-xs font-mono font-bold tracking-wider text-gray-300 border border-white/5 uppercase">
						Without Cheat
					</span>
				</div>
				<div class="absolute right-4 bottom-4 z-20 pointer-events-none select-none">
					<span class="rounded bg-amber-500/15 px-2.5 py-1 text-xs font-mono font-bold tracking-wider text-amber-400 border border-amber-500/20 uppercase">
						With Cheat
					</span>
				</div>

				<!-- Divider line -->
				<div
					class="absolute top-0 bottom-0 z-30 w-px pointer-events-none"
					style="left: {sliderPos}%; background: rgba(255, 184, 0, 0.45);"
				>
					<!-- Handle circle -->
					<div
						class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2 pointer-events-auto flex size-7 items-center justify-center rounded-full border backdrop-blur-sm cursor-col-resize"
						style="border-color: rgba(255, 184, 0, 0.5); background: rgba(0,0,0,0.75);"
					>
						<svg class="size-3.5" style="color: var(--color-amber);" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
							<path d="m9 18-6-6 6-6" />
							<path d="m15 6 6 6-6 6" />
						</svg>
					</div>
				</div>
			</div>
		</div>
	</div>
	</BlurFade>
</section>

<style>
	.sparkle-dot {
		position: absolute;
		width: 2.5px;
		height: 2.5px;
		border-radius: 50%;
		background: white;
		animation: sparkle-blink 3s ease-in-out infinite;
	}

	@keyframes sparkle-blink {
		0%, 100% { opacity: 0; transform: scale(0.5); }
		50% { opacity: 0.8; transform: scale(1.3); }
	}
</style>
