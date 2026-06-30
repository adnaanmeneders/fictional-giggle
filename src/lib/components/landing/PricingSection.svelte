<script lang="ts">
	import { cn } from '$lib/utils';

	const plans = [
		{ name: 'SOLO BUILD', firstMonth: '$70', nextMonth: '$60', popular: false },
		{ name: 'SOLO SPECIAL', firstMonth: '$120', nextMonth: '$70', popular: false },
		{ name: 'DUO BUILD', firstMonth: '$140', nextMonth: '$120', popular: false },
		{ name: 'TRIO BUILD', firstMonth: '$190', nextMonth: '$140', popular: true },
		{ name: 'TEAM BUILD (7)', firstMonth: '$360', nextMonth: '$280', popular: false },
		{ name: 'TEAM BUILD (10)', firstMonth: '$480', nextMonth: '$360', popular: false }
	];

	// Mouse positions state for pricing cards
	let cardMice: Record<number, { x: number; y: number }> = {};

	function handleMouseMove(e: MouseEvent, index: number) {
		const target = e.currentTarget as HTMLElement;
		const rect = target.getBoundingClientRect();
		cardMice[index] = {
			x: e.clientX - rect.left,
			y: e.clientY - rect.top
		};
	}
</script>

<div id="pricing" class="mx-auto relative overflow-x-hidden py-20">
	<!-- Header -->
	<article class="text-center mb-10 max-w-3xl mx-auto space-y-2 relative z-50">
		<h2 class="text-4xl font-black tracking-tighter text-white md:text-5xl lg:text-6xl uppercase">
			SELECT YOUR ACCESS
		</h2>
		<div class="mx-auto mt-4 h-0.5 w-16 bg-amber-500" style="background: var(--color-amber);"></div>
		<p class="text-gray-400 mt-4 text-sm">
			Trusted by thousands. Explore which option is right for you.
		</p>
	</article>

	<!-- Pricing Cards Grid -->
	<div class="grid sm:grid-cols-2 lg:grid-cols-3 max-w-6xl gap-4 py-6 mx-auto px-4 relative z-10">
		{#each plans as plan, index}
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class={cn(
					'spotlight-card relative rounded-xl border text-white transition-all duration-300',
					plan.popular ? 'border-white/10 z-20 shadow-[0_-20px_250px_-20px_rgba(255,184,0,0.15)]' : 'border-white/10 z-10'
				)}
				style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice[index]?.x ?? 0}px; --mouse-y: {cardMice[index]?.y ?? 0}px;"
				on:mousemove={(e) => handleMouseMove(e, index)}
			>
				<div class="p-6 text-left relative z-10">
					<h3 class="text-2xl mb-4">{plan.name}</h3>
					<div class="space-y-2">
						<div class="flex items-baseline gap-2">
							<span class="text-sm text-gray-400">First Month:</span>
							<span class="text-3xl font-semibold">{plan.firstMonth}</span>
						</div>
						<div class="flex items-baseline gap-2">
							<span class="text-sm text-gray-400">Next Month:</span>
							<span class="text-3xl font-semibold">{plan.nextMonth}</span>
						</div>
					</div>
				</div>
				<!-- Spotlight border overlay -->
				<div class="spotlight-border" />
			</div>
		{/each}
	</div>

	<!-- Single Discord button -->
	<div class="flex justify-center mt-10 relative z-10">
		<a
			href="https://discord.gg/target"
			target="_blank"
			rel="noopener noreferrer"
			class="relative group overflow-hidden p-0.5 rounded-lg active:scale-100 hover:scale-105 transition-all duration-300" style="background: rgba(255,184,0,0.3);"
		>
			<span class="relative z-10 flex items-center gap-3 bg-gradient-to-t from-black/50 to-black px-10 py-4 rounded-md text-white text-lg font-semibold tracking-wider">
				JOIN NOW
			</span>
			<div class="absolute -bottom-12 group-hover:-bottom-10 transition-all duration-200 left-1/2 -z-0 -translate-x-1/2 blur-xl size-14 rounded-full" style="background: #FFB800;"></div>
		</a>
	</div>
</div>

<style>
	/* Spotlight Card Background Glow */
	.spotlight-card {
		position: relative;
	}

	.spotlight-card::before {
		content: "";
		position: absolute;
		inset: 0;
		border-radius: inherit;
		pointer-events: none;
		opacity: 0;
		transition: opacity 0.5s ease;
		z-index: 5;
		background: radial-gradient(
			300px circle at var(--mouse-x, 0px) var(--mouse-y, 0px),
			rgba(255, 184, 0, 0.08),
			transparent 80%
		);
	}

	.spotlight-card:hover::before {
		opacity: 1;
	}

	/* Spotlight Border Beam Glow */
	.spotlight-border {
		position: absolute;
		inset: 0;
		border-radius: inherit;
		pointer-events: none;
		opacity: 0;
		transition: opacity 0.5s ease;
		z-index: 10;
		padding: 1px;
		background: radial-gradient(
			180px circle at var(--mouse-x, 0px) var(--mouse-y, 0px),
			rgba(255, 184, 0, 0.45),
			transparent 70%
		);
		-webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
		-webkit-mask-composite: xor;
		mask-composite: exclude;
	}

	.spotlight-card:hover .spotlight-border {
		opacity: 1;
	}
</style>
