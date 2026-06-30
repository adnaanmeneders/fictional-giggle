<script lang="ts">
	const testimonials = [
		{ name: 'Anonym1', handle: '@user_a1', text: "Flawless echo.ac bypass. Injection is extremely fast and target ESP works perfectly." },
		{ name: 'Anonym2', handle: '@user_b2', text: "Nexorascanner.ac scan went 100% clean. Best investment for Fivem." },
		{ name: 'Anonym3', handle: '@user_c3', text: "Easily bypassed napse.ac. Silent aimbot is extremely smooth." },
		{ name: 'Anonym4', handle: '@user_d4', text: "Anticheat.ac has no chance against Target. Undetected for weeks now." },
		{ name: 'Anonym5', handle: '@user_e5', text: "Passed detect.ac screenshare check last night. This engine is literally magic." },
		{ name: 'Anonym6', handle: '@user_f6', text: "Tested on echo.ac and nexorascanner, both bypassed easily. Best Fivem private cheat." },
		{ name: 'Anonym7', handle: '@user_g7', text: "Napse.ac bypassed without any FPS drop. Performance is stellar." },
		{ name: 'Anonym8', handle: '@user_h8', text: "Detect.ac safe, echo.ac safe. Easiest bypass configuration." }
	];

	// Duplicate lists 3 times to make perfect infinite vertical loops
	const col1 = [...testimonials, ...testimonials, ...testimonials];
	const col2 = [...testimonials, ...testimonials, ...testimonials].reverse();
	const col3 = [...testimonials, ...testimonials, ...testimonials].slice(3).concat(testimonials.slice(0, 3));
	const col4 = [...testimonials, ...testimonials, ...testimonials].slice(5).concat(testimonials.slice(0, 5)).reverse();
</script>

<section id="wall-of-fame" class="relative py-24 overflow-hidden bg-transparent">
	<!-- Gradient glows behind the 3D grid -->
	<div class="absolute top-1/2 left-0 w-72 h-72 rounded-full pointer-events-none -translate-y-1/2 bg-amber-500/5 blur-[120px]"></div>
	<div class="absolute top-1/2 right-0 w-72 h-72 rounded-full pointer-events-none -translate-y-1/2 bg-amber-500/5 blur-[120px]"></div>

	<div class="w-full px-4 md:px-8">
		<div class="max-w-3xl mx-auto text-center mb-16">
			<h2 class="text-4xl font-black tracking-tighter text-white md:text-5xl lg:text-6xl uppercase">
				REVIEWS & FEEDBACK
			</h2>
			<div class="mx-auto mt-4 h-0.5 w-16 bg-amber-500" style="background: var(--color-amber);"></div>
		</div>

		<!-- 3D Perspective testmonials container -->
		<div class="relative flex h-[480px] w-full flex-row items-center justify-center gap-4 [perspective:1000px] overflow-hidden">
			<div
				class="flex flex-row items-start gap-4"
				style="transform: translateX(0px) translateY(0px) translateZ(-50px) rotateX(15deg) rotateY(-8deg) rotateZ(10deg);"
			>
				<!-- Col 1: Downward Marquee -->
				<div class="flex flex-col gap-4 w-48 shrink-0 marquee-col-down">
					{#each col1 as item}
						<div class="review-card select-none">
							<div class="flex items-center gap-2 mb-2">
								<div class="avatar flex items-center justify-center">
									<svg class="size-3.5 text-amber-500 fill-none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
										<path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" />
										<circle cx="12" cy="7" r="4" />
									</svg>
								</div>
								<div>
									<h4 class="text-[11px] font-semibold text-white leading-none blur-[4px]">{item.name}</h4>
									<p class="text-[9px] text-gray-500 mt-0.5 blur-[4px] font-mono">{item.handle}</p>
								</div>
								<div class="ml-auto flex gap-0.5">
									{#each Array(5) as _}
										<span class="text-amber-500 text-[8px]">★</span>
									{/each}
								</div>
							</div>
							<p class="text-[10px] leading-relaxed text-gray-400">{item.text}</p>
						</div>
					{/each}
				</div>

				<!-- Col 2: Upward Marquee -->
				<div class="flex flex-col gap-4 w-48 shrink-0 marquee-col-up">
					{#each col2 as item}
						<div class="review-card select-none">
							<div class="flex items-center gap-2 mb-2">
								<div class="avatar flex items-center justify-center">
									<svg class="size-3.5 text-amber-500 fill-none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
										<path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" />
										<circle cx="12" cy="7" r="4" />
									</svg>
								</div>
								<div>
									<h4 class="text-[11px] font-semibold text-white leading-none blur-[4px]">{item.name}</h4>
									<p class="text-[9px] text-gray-500 mt-0.5 blur-[4px] font-mono">{item.handle}</p>
								</div>
								<div class="ml-auto flex gap-0.5">
									{#each Array(5) as _}
										<span class="text-amber-500 text-[8px]">★</span>
									{/each}
								</div>
							</div>
							<p class="text-[10px] leading-relaxed text-gray-400">{item.text}</p>
						</div>
					{/each}
				</div>

				<!-- Col 3: Downward Marquee -->
				<div class="flex flex-col gap-4 w-48 shrink-0 marquee-col-down">
					{#each col3 as item}
						<div class="review-card select-none">
							<div class="flex items-center gap-2 mb-2">
								<div class="avatar flex items-center justify-center">
									<svg class="size-3.5 text-amber-500 fill-none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
										<path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" />
										<circle cx="12" cy="7" r="4" />
									</svg>
								</div>
								<div>
									<h4 class="text-[11px] font-semibold text-white leading-none blur-[4px]">{item.name}</h4>
									<p class="text-[9px] text-gray-500 mt-0.5 blur-[4px] font-mono">{item.handle}</p>
								</div>
								<div class="ml-auto flex gap-0.5">
									{#each Array(5) as _}
										<span class="text-amber-500 text-[8px]">★</span>
									{/each}
								</div>
							</div>
							<p class="text-[10px] leading-relaxed text-gray-400">{item.text}</p>
						</div>
					{/each}
				</div>

				<!-- Col 4: Upward Marquee -->
				<div class="flex flex-col gap-4 w-48 shrink-0 marquee-col-up">
					{#each col4 as item}
						<div class="review-card select-none">
							<div class="flex items-center gap-2 mb-2">
								<div class="avatar flex items-center justify-center">
									<svg class="size-3.5 text-amber-500 fill-none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
										<path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" />
										<circle cx="12" cy="7" r="4" />
									</svg>
								</div>
								<div>
									<h4 class="text-[11px] font-semibold text-white leading-none blur-[4px]">{item.name}</h4>
									<p class="text-[9px] text-gray-500 mt-0.5 blur-[4px] font-mono">{item.handle}</p>
								</div>
								<div class="ml-auto flex gap-0.5">
									{#each Array(5) as _}
										<span class="text-amber-500 text-[8px]">★</span>
									{/each}
								</div>
							</div>
							<p class="text-[10px] leading-relaxed text-gray-400">{item.text}</p>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	.review-card {
		width: 100%;
		border-radius: 0.6rem;
		border: 1px solid rgba(255, 255, 255, 0.05);
		background: linear-gradient(135deg, rgba(255, 255, 255, 0.015) 0%, rgba(255, 255, 255, 0.005) 100%);
		backdrop-filter: blur(8px);
		padding: 0.85rem;
		transition: all 0.3s ease;
	}

	.review-card:hover {
		border-color: rgba(255, 184, 0, 0.2);
		background: linear-gradient(135deg, rgba(255, 184, 0, 0.03) 0%, rgba(255, 255, 255, 0.008) 100%);
		box-shadow: 0 10px 30px -10px rgba(255, 184, 0, 0.05);
	}

	.avatar {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 1.75rem;
		height: 1.75rem;
		border-radius: 9999px;
		background: rgba(255, 184, 0, 0.1);
		color: #FFB800;
	}

	.marquee-col-down {
		animation: marquee-down 24s linear infinite;
	}

	.marquee-col-up {
		animation: marquee-up 26s linear infinite;
	}

	.marquee-col-down:hover,
	.marquee-col-up:hover {
		animation-play-state: paused;
	}

	@keyframes marquee-down {
		from {
			transform: translateY(0);
		}
		to {
			transform: translateY(-33.333%);
		}
	}

	@keyframes marquee-up {
		from {
			transform: translateY(-33.333%);
		}
		to {
			transform: translateY(0);
		}
	}
</style>
