<script lang="ts">
	import { Eye } from 'lucide-svelte';
	import { cn } from '$lib/utils';

	let toggles = [
		{ label: 'Player Box', enabled: false },
		{ label: 'Health Bars', enabled: true },
		{ label: 'Distance ESP', enabled: true },
		{ label: 'Weapon ESP', enabled: true },
		{ label: 'Name ESP', enabled: true }
	];

	let boxColor = '#FFB800';
	let textColor = '#AAAAAA';

	function toggleFeature(index: number) {
		toggles[index].enabled = !toggles[index].enabled;
		toggles = toggles;
	}
</script>

<section id="visuals" class="relative mx-auto max-w-7xl px-6 py-20 md:px-8">
	<h2
		class="mb-16 text-center text-4xl font-black tracking-tighter text-white md:text-5xl lg:text-6xl"
		style="text-shadow: 0 0 60px rgba(255,255,255,0.05);"
	>
		VISUALS CONFIGURATION.
	</h2>

	<div class="mx-auto grid max-w-5xl grid-cols-1 items-center gap-8 lg:grid-cols-3">
		<!-- Left: Visuals toggles -->
		<div
			class="rounded-xl border border-white/10 p-6"
			style="background: rgba(255,255,255,0.02);"
		>
			<div class="mb-4 flex items-center gap-2">
				<Eye class="size-4" style="color: var(--color-amber);" />
				<span class="text-sm font-bold uppercase tracking-wider text-white">VISUALS</span>
			</div>

			<div class="flex flex-col gap-3">
				{#each toggles as toggle, i}
					<div class="flex items-center justify-between">
						<span class="text-sm text-gray-400">{toggle.label}</span>
						<button
							on:click={() => toggleFeature(i)}
							class={cn(
								'relative h-6 w-11 rounded-full transition-all duration-300 cursor-pointer',
								toggle.enabled ? 'bg-amber-500' : 'bg-gray-700'
							)}
							style={toggle.enabled ? 'background: var(--color-amber);' : ''}
						>
							<span
								class={cn(
									'absolute left-0.5 top-0.5 size-5 rounded-full bg-white transition-transform duration-300 shadow-md',
									toggle.enabled ? 'translate-x-5' : 'translate-x-0'
								)}
							></span>
						</button>
					</div>
				{/each}
			</div>
		</div>

		<!-- Center: Character preview placeholder -->
		<div class="flex flex-col items-center justify-center">
			<div class="relative flex h-80 w-64 flex-col items-center justify-center">
				<!-- Vertical ESP line -->
				<div class="absolute left-1/2 top-0 h-full w-0.5 -translate-x-1/2" style="background: linear-gradient(to bottom, transparent, #00ff00, transparent);"></div>

				<!-- Character silhouette -->
				<div class="relative z-10 flex flex-col items-center">
					<div class="text-xs text-gray-400 mb-2 tracking-wider">Banana Guy</div>
					<!-- Stylized character representation -->
					<div class="relative">
						<svg class="size-40 text-amber-400" viewBox="0 0 100 150" fill="none">
							<!-- Head -->
							<ellipse cx="50" cy="25" rx="15" ry="18" fill="currentColor" opacity="0.8"/>
							<!-- Body -->
							<ellipse cx="50" cy="65" rx="20" ry="25" fill="currentColor" opacity="0.7"/>
							<!-- Arms -->
							<ellipse cx="25" cy="60" rx="6" ry="15" fill="currentColor" opacity="0.6" transform="rotate(-15 25 60)"/>
							<ellipse cx="75" cy="60" rx="6" ry="15" fill="currentColor" opacity="0.6" transform="rotate(15 75 60)"/>
							<!-- Legs -->
							<ellipse cx="40" cy="105" rx="7" ry="20" fill="currentColor" opacity="0.6"/>
							<ellipse cx="60" cy="105" rx="7" ry="20" fill="currentColor" opacity="0.6"/>
							<!-- Eyes -->
							<circle cx="44" cy="22" r="3" fill="#000"/>
							<circle cx="56" cy="22" r="3" fill="#000"/>
						</svg>
						<!-- ESP box -->
						<div class="absolute -inset-3 border-2 rounded" style="border-color: var(--color-amber);"></div>
						<!-- Health bar -->
						<div class="absolute -left-6 top-0 h-full w-1.5 rounded-full bg-gray-700 overflow-hidden">
							<div class="absolute bottom-0 w-full rounded-full" style="height: 85%; background: #00ff00;"></div>
						</div>
					</div>
					<div class="mt-3 text-xs text-gray-500">[14m]</div>
					<div class="text-xs font-semibold tracking-wider text-white mt-1">BANANA GUN</div>
				</div>
			</div>
		</div>

		<!-- Right: ESP Colors -->
		<div
			class="rounded-xl border border-white/10 p-6"
			style="background: rgba(255,255,255,0.02);"
		>
			<div class="mb-4 flex items-center gap-2">
				<svg class="size-4" style="color: var(--color-amber);" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
					<circle cx="12" cy="12" r="10"/>
					<line x1="2" y1="12" x2="22" y2="12"/>
					<path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>
				</svg>
				<span class="text-sm font-bold uppercase tracking-wider text-white">ESP COLORS</span>
			</div>

			<div class="flex flex-col gap-4">
				<div class="flex items-center justify-between">
					<span class="text-sm text-gray-400">Box Color</span>
					<div class="relative">
						<input
							type="color"
							bind:value={boxColor}
							class="h-8 w-8 cursor-pointer appearance-none rounded border-0 bg-transparent"
							style="padding: 0;"
						/>
						<div
							class="pointer-events-none absolute inset-0 rounded"
							style="background: {boxColor}; border: 1px solid rgba(255,255,255,0.1);"
						></div>
					</div>
				</div>
				<div class="flex items-center justify-between">
					<span class="text-sm text-gray-400">Text Color</span>
					<div class="relative">
						<input
							type="color"
							bind:value={textColor}
							class="h-8 w-8 cursor-pointer appearance-none rounded border-0 bg-transparent"
							style="padding: 0;"
						/>
						<div
							class="pointer-events-none absolute inset-0 rounded"
							style="background: {textColor}; border: 1px solid rgba(255,255,255,0.1);"
						></div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>
