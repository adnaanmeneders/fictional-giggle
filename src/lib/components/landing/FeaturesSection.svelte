<script lang="ts">
	import { cn } from '$lib/utils';
	import RotatingGlobe from '$lib/components/magic/RotatingGlobe.svelte';
	import DiscordWidget from './DiscordWidget.svelte';
</script>

<section id="contact" class="relative py-24">
	<div class="mx-auto max-w-screen-xl px-4 md:px-8">
		<div class="max-w-3xl mx-auto text-center mb-16">
			<h2 class="text-4xl font-black tracking-tighter text-white md:text-5xl lg:text-6xl uppercase">
				CONTACT
			</h2>
			<div class="mx-auto mt-4 h-0.5 w-16 bg-amber-500" style="background: var(--color-amber);"></div>
			<p class="mt-4 text-sm text-gray-400">
				Connect with our support team and check server status worldwide.
			</p>
		</div>

		<div class="grid grid-cols-1 gap-12 lg:grid-cols-2 items-center">
			<!-- Left: Orbital logo animation -->
			<div class="flex items-center justify-center">
				<div class="relative flex size-80 items-center justify-center">
					<!-- Rotating Earth Globe -->
					<RotatingGlobe
						width={320}
						height={320}
						class="absolute inset-0 size-full z-0 opacity-70"
					/>
				</div>
			</div>

			<!-- Right: Custom Discord Widget Card -->
			<div class="flex items-center justify-center w-full">
				<DiscordWidget />
			</div>
		</div>
	</div>
</section>
