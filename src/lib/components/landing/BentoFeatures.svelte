<script lang="ts">
	import { Shield, Cpu, Eye, Zap, Code } from 'lucide-svelte';
	import { cn } from '$lib/utils';

	// Chart hover state
	let chartHover = { active: false, x: 0, svgX: 0, svgY: 0, ms: '0.0' };

	// Curve data points for interpolation (matching the SVG path)
	const curvePoints = [
		[35, 85], [55, 80], [70, 72], [90, 75], [110, 78], [120, 60],
		[140, 55], [160, 50], [175, 68], [195, 65], [215, 62], [225, 45],
		[245, 42], [265, 39], [275, 55], [295, 50], [315, 45], [325, 35],
		[345, 38], [365, 41], [375, 30], [380, 28]
	];

	function interpolateY(svgX: number): number {
		for (let i = 0; i < curvePoints.length - 1; i++) {
			const [x1, y1] = curvePoints[i];
			const [x2, y2] = curvePoints[i + 1];
			if (svgX >= x1 && svgX <= x2) {
				const t = (svgX - x1) / (x2 - x1);
				return y1 + t * (y2 - y1);
			}
		}
		return curvePoints[curvePoints.length - 1][1];
	}

	function handleChartHover(e: MouseEvent) {
		const target = e.currentTarget as HTMLElement;
		const rect = target.getBoundingClientRect();
		const x = e.clientX - rect.left;
		const relX = x / rect.width;
		const svgX = Math.max(35, Math.min(380, relX * 386));
		const svgY = interpolateY(svgX);
		// Map svgY (28-85) to ms (0.2-3.8) - lower Y = lower ms
		const ms = (0.2 + ((svgY - 28) / (85 - 28)) * 3.6).toFixed(1);
		chartHover = { active: true, x, svgX, svgY, ms };

		// Spotlight tracking for performance card
		cardMice['performance'] = {
			x: e.clientX - rect.left,
			y: e.clientY - rect.top
		};
	}

	function handleChartLeave() {
		chartHover = { ...chartHover, active: false };
	}

	// Spotlight positions state
	let cardMice: Record<string, { x: number; y: number }> = {
		undetected: { x: 0, y: 0 },
		secure: { x: 0, y: 0 },
		performance: { x: 0, y: 0 },
		'anti-detection': { x: 0, y: 0 },
		'low-cpu': { x: 0, y: 0 }
	};

	function handleCardMouseMove(e: MouseEvent, cardId: string) {
		const target = e.currentTarget as HTMLElement;
		const rect = target.getBoundingClientRect();
		cardMice[cardId] = {
			x: e.clientX - rect.left,
			y: e.clientY - rect.top
		};
	}

	// Target Ready sound + visual
	let fpActivated = false;

	function playTargetReady() {
		if (fpActivated) return;
		fpActivated = true;

		try {
			const ctx = new (window.AudioContext || (window as any).webkitAudioContext)();

			// Beep 1 - rising tone
			const osc1 = ctx.createOscillator();
			const gain1 = ctx.createGain();
			osc1.type = 'sine';
			osc1.frequency.setValueAtTime(600, ctx.currentTime);
			osc1.frequency.linearRampToValueAtTime(900, ctx.currentTime + 0.15);
			gain1.gain.setValueAtTime(0.3, ctx.currentTime);
			gain1.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.2);
			osc1.connect(gain1).connect(ctx.destination);
			osc1.start(ctx.currentTime);
			osc1.stop(ctx.currentTime + 0.2);

			// Beep 2 - confirmation tone
			const osc2 = ctx.createOscillator();
			const gain2 = ctx.createGain();
			osc2.type = 'sine';
			osc2.frequency.setValueAtTime(1200, ctx.currentTime + 0.25);
			gain2.gain.setValueAtTime(0, ctx.currentTime);
			gain2.gain.setValueAtTime(0.35, ctx.currentTime + 0.25);
			gain2.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.55);
			osc2.connect(gain2).connect(ctx.destination);
			osc2.start(ctx.currentTime + 0.25);
			osc2.stop(ctx.currentTime + 0.55);

			// Beep 3 - high confirmation
			const osc3 = ctx.createOscillator();
			const gain3 = ctx.createGain();
			osc3.type = 'sine';
			osc3.frequency.setValueAtTime(1500, ctx.currentTime + 0.55);
			gain3.gain.setValueAtTime(0, ctx.currentTime);
			gain3.gain.setValueAtTime(0.3, ctx.currentTime + 0.55);
			gain3.gain.exponentialRampToValueAtTime(0.01, ctx.currentTime + 1.0);
			osc3.connect(gain3).connect(ctx.destination);
			osc3.start(ctx.currentTime + 0.55);
			osc3.stop(ctx.currentTime + 1.0);
		} catch(e) {
			// Audio not supported
		}

		setTimeout(() => { fpActivated = false; }, 2500);
	}
</script>

<section id="bento-features" class="py-16 md:py-32">
	<div class="mx-auto max-w-3xl lg:max-w-5xl px-6">
		<div class="relative">
			<div class="relative z-10 grid grid-cols-6 gap-3">
				
				<!-- Card 1: 100% Undetected -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div
					id="undetected"
					class="spotlight-card group relative col-span-full flex flex-col justify-between overflow-hidden lg:col-span-2 rounded-xl border border-white/10 text-card-foreground shadow-sm transition-all duration-300"
					style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice.undetected.x}px; --mouse-y: {cardMice.undetected.y}px;"
					on:mousemove={(e) => handleCardMouseMove(e, 'undetected')}
				>
					<div class="relative m-auto size-fit p-6">
						<div class="relative flex h-24 w-56 items-center">
							<svg class="text-white/5 absolute inset-0 size-full" viewBox="0 0 254 104" fill="none" xmlns="http://www.w3.org/2000/svg">
								<path
									d="M112.891 97.7022C140.366 97.0802 171.004 94.6715 201.087 87.5116C210.43 85.2881 219.615 82.6412 228.284 78.2473C232.198 76.3179 235.905 73.9942 239.348 71.3124C241.85 69.2557 243.954 66.7571 245.555 63.9408C249.34 57.3235 248.281 50.5341 242.498 45.6109C239.033 42.7237 235.228 40.2703 231.169 38.3054C219.443 32.7209 207.141 28.4382 194.482 25.534C184.013 23.1927 173.358 21.7755 162.64 21.2989C161.376 21.3512 160.113 21.181 158.908 20.796C158.034 20.399 156.857 19.1682 156.962 18.4535C157.115 17.8927 157.381 17.3689 157.743 16.9139C158.104 16.4588 158.555 16.0821 159.067 15.8066C160.14 15.4683 161.274 15.3733 162.389 15.5286C179.805 15.3566 196.626 18.8373 212.998 24.462C220.978 27.2494 228.798 30.4747 236.423 34.1232C240.476 36.1159 244.202 38.7131 247.474 41.8258C254.342 48.2578 255.745 56.9397 251.841 65.4892C249.793 69.8582 246.736 73.6777 242.921 76.6327C236.224 82.0192 228.522 85.4602 220.502 88.2924C205.017 93.7847 188.964 96.9081 172.738 99.2109C153.442 101.949 133.993 103.478 114.506 103.79C91.1468 104.161 67.9334 102.97 45.1169 97.5831C36.0094 95.5616 27.2626 92.1655 19.1771 87.5116C13.839 84.5746 9.1557 80.5802 5.41318 75.7725C-0.54238 67.7259 -1.13794 59.1763 3.25594 50.2827C5.82447 45.3918 9.29572 41.0315 13.4863 37.4319C24.2989 27.5721 37.0438 20.9681 50.5431 15.7272C68.1451 8.8849 86.4883 5.1395 105.175 2.83669C129.045 0.0992292 153.151 0.134761 177.013 2.94256C197.672 5.23215 218.04 9.01724 237.588 16.3889C240.089 17.3418 242.498 18.5197 244.933 19.6446C246.627 20.4387 247.725 21.6695 246.997 23.615C246.455 25.1105 244.814 25.5605 242.63 24.5811C230.322 18.9961 217.233 16.1904 204.117 13.4376C188.761 10.3438 173.2 8.36665 157.558 7.52174C129.914 5.70776 102.154 8.06792 75.2124 14.5228C60.6177 17.8788 46.5758 23.2977 33.5102 30.6161C26.6595 34.3329 20.4123 39.0673 14.9818 44.658C12.9433 46.8071 11.1336 49.1622 9.58207 51.6855C4.87056 59.5336 5.61172 67.2494 11.9246 73.7608C15.2064 77.0494 18.8775 79.925 22.8564 82.3236C31.6176 87.7101 41.3848 90.5291 51.3902 92.5804C70.6068 96.5773 90.0219 97.7419 112.891 97.7022Z"
									fill="currentColor"
								/>
							</svg>
							<span class="mx-auto block w-fit text-5xl font-semibold text-white">100%</span>
						</div>
						<h2 class="mt-6 text-center text-3xl font-semibold text-white">Undetected</h2>
					</div>
					<!-- Spotlight borders -->
					<div class="spotlight-border" />
				</div>

				<!-- Card 2: Secure by default (Full Fingerprint) -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div
					id="secure"
					class="spotlight-card group relative col-span-full flex flex-col justify-between overflow-hidden sm:col-span-3 lg:col-span-2 rounded-xl border border-white/10 text-card-foreground shadow-sm transition-all duration-300"
					style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice.secure.x}px; --mouse-y: {cardMice.secure.y}px;"
					on:mousemove={(e) => handleCardMouseMove(e, 'secure')}
				>
					<div class="p-6">
						<!-- svelte-ignore a11y-click-events-have-key-events -->
						<!-- svelte-ignore a11y-no-static-element-interactions -->
						<div
							class="relative mx-auto flex aspect-square size-32 rounded-full border cursor-pointer transition-all duration-500 before:absolute before:-inset-2 before:rounded-full before:border before:transition-all before:duration-500"
							on:click={playTargetReady}
							style={fpActivated
								? 'border-color: var(--color-amber); box-shadow: 0 0 40px rgba(255,184,0,0.3);'
								: 'border-color: rgba(255,255,255,0.1);'
							}
						>
							<svg class="m-auto h-fit w-24" viewBox="0 0 212 143" fill="none" xmlns="http://www.w3.org/2000/svg">
								<!-- Background Fingerprint (Dark Gray) -->
								<path
									d="M44.0209 55.3542C43.1945 54.7639 42.6916 54.0272 42.5121 53.1442C42.3327 52.2611 42.5995 51.345 43.3125 50.3958C50.632 40.3611 59.812 32.5694 70.8525 27.0208C81.8931 21.4722 93.668 18.6979 106.177 18.6979C118.691 18.6979 130.497 21.3849 141.594 26.7587C152.691 32.1326 161.958 39.8936 169.396 50.0417C170.222 51.1042 170.489 52.0486 170.196 52.875C169.904 53.7014 169.401 54.4097 168.688 55C167.979 55.5903 167.153 55.8571 166.208 55.8004C165.264 55.7437 164.438 55.2408 163.729 54.2917C157.236 45.0833 148.885 38.0307 138.675 33.1337C128.466 28.2368 117.633 25.786 106.177 25.7812C94.7257 25.7812 83.9827 28.2321 73.948 33.1337C63.9132 38.0354 55.5903 45.0881 48.9792 54.2917C48.2709 55.3542 47.4445 55.9444 46.5 56.0625C45.5556 56.1806 44.7292 55.9444 44.0209 55.3542ZM126.188 142.656C113.91 139.587 103.875 133.476 96.0834 124.325C88.2917 115.173 84.3959 103.988 84.3959 90.7708C84.3959 84.8681 86.5209 79.9097 90.7709 75.8958C95.0209 71.8819 100.156 69.875 106.177 69.875C112.198 69.875 117.333 71.8819 121.583 75.8958C125.833 79.9097 127.958 84.8681 127.958 90.7708C127.958 94.6667 129.434 97.9439 132.385 100.602C135.337 103.261 138.819 104.588 142.833 104.583C146.847 104.583 150.271 103.256 153.104 100.602C155.938 97.9486 157.354 94.6714 157.354 90.7708C157.354 77.0764 152.337 65.566 142.302 56.2396C132.267 46.9132 120.285 42.25 106.354 42.25C92.4237 42.25 80.441 46.9132 70.4063 56.2396C60.3716 65.566 55.3542 77.0174 55.3542 90.5937C55.3542 93.4271 55.621 96.9687 56.1546 101.219C56.6882 105.469 57.9562 110.427 59.9584 116.094C60.3125 117.156 60.2842 118.101 59.8734 118.927C59.4625 119.753 58.7825 120.344 57.8334 120.698C56.8889 121.052 55.9752 121.024 55.0921 120.613C54.2091 120.202 53.5881 119.522 53.2292 118.573C51.4584 113.969 50.1905 109.395 49.4255 104.853C48.6605 100.31 48.2756 95.6158 48.2709 90.7708C48.2709 75.0694 53.9682 61.9062 65.363 51.2812C76.7577 40.6562 90.3624 35.3437 106.177 35.3437C122.115 35.3437 135.809 40.6562 147.26 51.2812C158.712 61.9062 164.438 75.0694 164.438 90.7708C164.438 96.6736 162.343 101.601 158.155 105.554C153.966 109.506 148.859 111.485 142.833 111.49C136.813 111.49 131.649 109.513 127.342 105.561C123.035 101.608 120.88 96.6783 120.875 90.7708C120.875 86.875 119.43 83.5978 116.54 80.9392C113.65 78.2805 110.196 76.9536 106.177 76.9583C102.163 76.9583 98.7089 78.2876 95.8142 80.9462C92.9195 83.6049 91.4745 86.8797 91.4792 90.7708C91.4792 102.222 94.8745 111.785 101.665 119.458C108.456 127.132 117.22 132.503 127.958 135.573C129.021 135.927 129.729 136.517 130.083 137.344C130.438 138.17 130.497 139.056 130.26 140C130.024 140.826 129.552 141.535 128.844 142.125C128.135 142.715 127.25 142.892 126.188 142.656ZM67.0417 18.3437C66.0973 18.934 65.1528 19.0828 64.2084 18.79C63.2639 18.4972 62.5556 17.8762 62.0834 16.9271C61.6112 15.9826 61.4931 15.1279 61.7292 14.3629C61.9653 13.5979 62.5556 12.9179 63.5 12.3229C70.1112 8.78125 77.0174 6.06597 84.2188 4.17708C91.4202 2.28819 98.7396 1.34375 106.177 1.34375C113.733 1.34375 121.111 2.25986 128.313 4.09208C135.514 5.92431 142.479 8.54986 149.208 11.9687C150.271 12.559 150.892 13.2674 151.071 14.0937C151.251 14.9201 151.161 15.7465 150.802 16.5729C150.448 17.3993 149.858 18.0486 149.031 18.5208C148.205 18.9931 147.201 18.934 146.021 18.3437C139.764 15.1563 133.299 12.7078 126.627 10.9983C119.954 9.28889 113.138 8.43181 106.177 8.42708C99.3299 8.42708 92.6007 9.22514 85.9896 10.8212C79.3785 12.4174 73.0625 14.9249 67.0417 18.3437ZM87.9375 140.177C80.9723 132.858 75.6314 125.392 71.915 117.78C68.1987 110.167 66.3381 101.164 66.3334 90.7708C66.3334 80.0278 70.2292 70.9658 78.0209 63.585C85.8125 56.2042 95.198 52.5161 106.177 52.5208C117.156 52.5208 126.601 56.2112 134.51 63.5921C142.42 70.9729 146.375 80.0325 146.375 90.7708C146.375 91.8333 146.052 92.6904 145.405 93.3421C144.758 93.9937 143.901 94.3172 142.833 94.3125C141.889 94.3125 141.063 93.989 140.354 93.3421C139.646 92.6951 139.292 91.8381 139.292 90.7708C139.292 81.9167 136.014 74.5099 129.46 68.5504C122.906 62.591 115.145 59.6089 106.177 59.6042C97.2049 59.6042 89.503 62.5862 83.0713 68.5504C76.6396 74.5146 73.4214 81.9214 73.4167 90.7708C73.4167 100.333 75.0695 108.451 78.375 115.123C81.6806 121.796 86.5209 128.494 92.8959 135.219C93.6042 135.927 93.9584 136.753 93.9584 137.698C93.9584 138.642 93.6042 139.469 92.8959 140.177C92.1875 140.885 91.3612 141.24 90.4167 141.24C89.4723 141.24 88.6459 140.885 87.9375 140.177ZM141.417 128.135C130.91 128.135 121.789 124.594 114.054 117.51C106.319 110.427 102.454 101.514 102.458 90.7708C102.458 89.8264 102.784 89 103.436 88.2917C104.088 87.5833 104.942 87.2292 106 87.2292C107.063 87.2292 107.92 87.5833 108.571 88.2917C109.223 89 109.546 89.8264 109.542 90.7708C109.542 99.625 112.729 106.885 119.104 112.552C125.479 118.219 132.917 121.052 141.417 121.052C142.125 121.052 143.129 120.993 144.427 120.875C145.726 120.757 147.083 120.58 148.5 120.344C149.563 120.108 150.479 120.256 151.248 120.79C152.018 121.324 152.519 122.119 152.75 123.177C152.986 124.122 152.809 124.948 152.219 125.656C151.629 126.365 150.861 126.837 149.917 127.073C147.792 127.663 145.934 127.989 144.342 128.05C142.751 128.112 141.776 128.14 141.417 128.135Z"
									fill="rgba(255,255,255,0.06)"
								/>
								<!-- Clipped Amber overlay -->
								<g clip-path="url(#clip_fp)">
									<path
										d="M44.0209 55.3542C43.1945 54.7639 42.6916 54.0272 42.5121 53.1442C42.3327 52.2611 42.5995 51.345 43.3125 50.3958C50.632 40.3611 59.812 32.5694 70.8525 27.0208C81.8931 21.4722 93.668 18.6979 106.177 18.6979C118.691 18.6979 130.497 21.3849 141.594 26.7587C152.691 32.1326 161.958 39.8936 169.396 50.0417C170.222 51.1042 170.489 52.0486 170.196 52.875C169.904 53.7014 169.401 54.4097 168.688 55C167.979 55.5903 167.153 55.8571 166.208 55.8004C165.264 55.7437 164.438 55.2408 163.729 54.2917C157.236 45.0833 148.885 38.0307 138.675 33.1337C128.466 28.2368 117.633 25.786 106.177 25.7812C94.7257 25.7812 83.9827 28.2321 73.948 33.1337C63.9132 38.0354 55.5903 45.0881 48.9792 54.2917C48.2709 55.3542 47.4445 55.9444 46.5 56.0625C45.5556 56.1806 44.7292 55.9444 44.0209 55.3542ZM126.188 142.656C113.91 139.587 103.875 133.476 96.0834 124.325C88.2917 115.173 84.3959 103.988 84.3959 90.7708C84.3959 84.8681 86.5209 79.9097 90.7709 75.8958C95.0209 71.8819 100.156 69.875 106.177 69.875C112.198 69.875 117.333 71.8819 121.583 75.8958C125.833 79.9097 127.958 84.8681 127.958 90.7708C127.958 94.6667 129.434 97.9439 132.385 100.602C135.337 103.261 138.819 104.588 142.833 104.583C146.847 104.583 150.271 103.256 153.104 100.602C155.938 97.9486 157.354 94.6714 157.354 90.7708C157.354 77.0764 152.337 65.566 142.302 56.2396C132.267 46.9132 120.285 42.25 106.354 42.25C92.4237 42.25 80.441 46.9132 70.4063 56.2396C60.3716 65.566 55.3542 77.0174 55.3542 90.5937C55.3542 93.4271 55.621 96.9687 56.1546 101.219C56.6882 105.469 57.9562 110.427 59.9584 116.094C60.3125 117.156 60.2842 118.101 59.8734 118.927C59.4625 119.753 58.7825 120.344 57.8334 120.698C56.8889 121.052 55.9752 121.024 55.0921 120.613C54.2091 120.202 53.5881 119.522 53.2292 118.573C51.4584 113.969 50.1905 109.395 49.4255 104.853C48.6605 100.31 48.2756 95.6158 48.2709 90.7708C48.2709 75.0694 53.9682 61.9062 65.363 51.2812C76.7577 40.6562 90.3624 35.3437 106.177 35.3437C122.115 35.3437 135.809 40.6562 147.26 51.2812C158.712 61.9062 164.438 75.0694 164.438 90.7708C164.438 96.6736 162.343 101.601 158.155 105.554C153.966 109.506 148.859 111.485 142.833 111.49C136.813 111.49 131.649 109.513 127.342 105.561C123.035 101.608 120.88 96.6783 120.875 90.7708C120.875 86.875 119.43 83.5978 116.54 80.9392C113.65 78.2805 110.196 76.9536 106.177 76.9583C102.163 76.9583 98.7089 78.2876 95.8142 80.9462C92.9195 83.6049 91.4745 86.8797 91.4792 90.7708C91.4792 102.222 94.8745 111.785 101.665 119.458C108.456 127.132 117.22 132.503 127.958 135.573C129.021 135.927 129.729 136.517 130.083 137.344C130.438 138.17 130.497 139.056 130.26 140C130.024 140.826 129.552 141.535 128.844 142.125C128.135 142.715 127.25 142.892 126.188 142.656ZM67.0417 18.3437C66.0973 18.934 65.1528 19.0828 64.2084 18.79C63.2639 18.4972 62.5556 17.8762 62.0834 16.9271C61.6112 15.9826 61.4931 15.1279 61.7292 14.3629C61.9653 13.5979 62.5556 12.9179 63.5 12.3229C70.1112 8.78125 77.0174 6.06597 84.2188 4.17708C91.4202 2.28819 98.7396 1.34375 106.177 1.34375C113.733 1.34375 121.111 2.25986 128.313 4.09208C135.514 5.92431 142.479 8.54986 149.208 11.9687C150.271 12.559 150.892 13.2674 151.071 14.0937C151.251 14.9201 151.161 15.7465 150.802 16.5729C150.448 17.3993 149.858 18.0486 149.031 18.5208C148.205 18.9931 147.201 18.934 146.021 18.3437C139.764 15.1563 133.299 12.7078 126.627 10.9983C119.954 9.28889 113.138 8.43181 106.177 8.42708C99.3299 8.42708 92.6007 9.22514 85.9896 10.8212C79.3785 12.4174 73.0625 14.9249 67.0417 18.3437ZM87.9375 140.177C80.9723 132.858 75.6314 125.392 71.915 117.78C68.1987 110.167 66.3381 101.164 66.3334 90.7708C66.3334 80.0278 70.2292 70.9658 78.0209 63.585C85.8125 56.2042 95.198 52.5161 106.177 52.5208C117.156 52.5208 126.601 56.2112 134.51 63.5921C142.42 70.9729 146.375 80.0325 146.375 90.7708C146.375 91.8333 146.052 92.6904 145.405 93.3421C144.758 93.9937 143.901 94.3172 142.833 94.3125C141.889 94.3125 141.063 93.989 140.354 93.3421C139.646 92.6951 139.292 91.8381 139.292 90.7708C139.292 81.9167 136.014 74.5099 129.46 68.5504C122.906 62.591 115.145 59.6089 106.177 59.6042C97.2049 59.6042 89.503 62.5862 83.0713 68.5504C76.6396 74.5146 73.4214 81.9214 73.4167 90.7708C73.4167 100.333 75.0695 108.451 78.375 115.123C81.6806 121.796 86.5209 128.494 92.8959 135.219C93.6042 135.927 93.9584 136.753 93.9584 137.698C93.9584 138.642 93.6042 139.469 92.8959 140.177C92.1875 140.885 91.3612 141.24 90.4167 141.24C89.4723 141.24 88.6459 140.885 87.9375 140.177ZM141.417 128.135C130.91 128.135 121.789 124.594 114.054 117.51C106.319 110.427 102.454 101.514 102.458 90.7708C102.458 89.8264 102.784 89 103.436 88.2917C104.088 87.5833 104.942 87.2292 106 87.2292C107.063 87.2292 107.92 87.5833 108.571 88.2917C109.223 89 109.546 89.8264 109.542 90.7708C109.542 99.625 112.729 106.885 119.104 112.552C125.479 118.219 132.917 121.052 141.417 121.052C142.125 121.052 143.129 120.993 144.427 120.875C145.726 120.757 147.083 120.58 148.5 120.344C149.563 120.108 150.479 120.256 151.248 120.79C152.018 121.324 152.519 122.119 152.75 123.177C152.986 124.122 152.809 124.948 152.219 125.656C151.629 126.365 150.861 126.837 149.917 127.073C147.792 127.663 145.934 127.989 144.342 128.05C142.751 128.112 141.776 128.14 141.417 128.135Z"
										fill="url(#paint_fp_linear)"
									/>
								</g>
								<!-- Horizontal Scanner Line -->
								<line
									x1="41"
									y1="72"
									x2="170"
									y2="72"
									stroke="#FFB800"
									stroke-width="3"
								/>
								<defs>
									<linearGradient id="paint_fp_linear" x1="106" y1="1" x2="106" y2="72" gradientUnits="userSpaceOnUse">
										<stop stop-color="#FFB800" stop-opacity="0" />
										<stop offset="1" stop-color="#FFB800" stop-opacity="0.85" />
									</linearGradient>
									<clipPath id="clip_fp">
										<rect width="129" height="72" fill="white" transform="translate(41)" />
									</clipPath>
								</defs>
							</svg>
						</div>
						<div class="relative z-10 mt-6 space-y-2 text-center">
							{#if fpActivated}
								<h2 class="text-lg font-bold tracking-widest animate-pulse" style="color: var(--color-amber);">TARGET READY</h2>
								<p class="text-sm" style="color: var(--color-amber); opacity: 0.6;">Identity verified. Access granted.</p>
							{:else}
								<h2 class="text-lg font-medium text-white">Secure by default</h2>
								<p class="text-sm text-gray-400">Memory-safe injection with zero kernel footprint. Your system stays clean.</p>
							{/if}
						</div>
					</div>
					<!-- Spotlight borders -->
					<div class="spotlight-border" />
				</div>

				<!-- Card 3: Faster than light -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div
					id="performance"
					class="spotlight-card group relative col-span-full flex flex-col justify-between overflow-hidden sm:col-span-3 lg:col-span-2 rounded-xl border border-white/10 text-card-foreground shadow-sm transition-all duration-300"
					style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice.performance.x}px; --mouse-y: {cardMice.performance.y}px;"
					on:mousemove={handleChartHover}
					on:mouseleave={handleChartLeave}
				>
					<div class="p-6">
						<div class="pt-6 lg:px-6 relative" role="img" aria-label="Performance chart">
							<!-- Tooltip -->
							{#if chartHover.active}
								<div
									class="absolute z-20 pointer-events-none transition-opacity duration-150"
									style="left: {chartHover.x}px; top: 4px; transform: translateX(-50%);"
								>
									<div class="rounded-md px-2.5 py-1 text-[10px] font-mono font-bold whitespace-nowrap border border-white/10" style="background: rgba(0,0,0,0.85); color: var(--color-amber);">
										{chartHover.ms}ms
									</div>
								</div>
							{/if}

							<svg class="text-gray-500 w-full" viewBox="0 0 386 123" fill="none" xmlns="http://www.w3.org/2000/svg">
								<rect width="386" height="123" rx="10" />
								
								<!-- Y-axis labels -->
								<text x="8" y="30" fill="rgba(255,255,255,0.2)" font-size="8" font-family="monospace">0ms</text>
								<text x="8" y="65" fill="rgba(255,255,255,0.2)" font-size="8" font-family="monospace">2ms</text>
								<text x="8" y="100" fill="rgba(255,255,255,0.2)" font-size="8" font-family="monospace">4ms</text>

								<!-- Horizontal grid lines -->
								<line x1="30" y1="30" x2="380" y2="30" stroke="rgba(255,255,255,0.04)" stroke-width="0.5" />
								<line x1="30" y1="55" x2="380" y2="55" stroke="rgba(255,255,255,0.04)" stroke-width="0.5" />
								<line x1="30" y1="80" x2="380" y2="80" stroke="rgba(255,255,255,0.04)" stroke-width="0.5" />
								<line x1="30" y1="105" x2="380" y2="105" stroke="rgba(255,255,255,0.04)" stroke-width="0.5" />

								<!-- Gradient fill under curve -->
								<path
									d="M35 85 C55 80, 70 72, 90 75 C110 78, 120 60, 140 55 C160 50, 175 68, 195 65 C215 62, 225 45, 245 42 C265 39, 275 55, 295 50 C315 45, 325 35, 345 38 C365 41, 375 30, 380 28 L380 115 L35 115 Z"
									fill="url(#paint_perf_gradient)"
								/>
								
								<!-- Main line -->
								<path
									class="text-amber-500"
									d="M35 85 C55 80, 70 72, 90 75 C110 78, 120 60, 140 55 C160 50, 175 68, 195 65 C215 62, 225 45, 245 42 C265 39, 275 55, 295 50 C315 45, 325 35, 345 38 C365 41, 375 30, 380 28"
									stroke="currentColor"
									stroke-width="2.5"
									fill="none"
									stroke-linecap="round"
								/>

								<!-- Hover crosshair -->
								{#if chartHover.active}
									<line
										x1={chartHover.svgX}
										y1="20"
										x2={chartHover.svgX}
										y2="115"
										stroke="rgba(255,184,0,0.4)"
										stroke-width="1"
										stroke-dasharray="3 3"
									/>
									<circle
										cx={chartHover.svgX}
										cy={chartHover.svgY}
										r="4"
										fill="#FFB800"
										stroke="black"
										stroke-width="2"
									/>
								{/if}

								<!-- Glow dot at end -->
								<circle cx="380" cy="28" r="3" fill="#FFB800" />
								<circle cx="380" cy="28" r="6" fill="rgba(255,184,0,0.2)" />

								<defs>
									<linearGradient id="paint_perf_gradient" x1="200" y1="28" x2="200" y2="115" gradientUnits="userSpaceOnUse">
										<stop stop-color="rgba(255,184,0,0.12)" />
										<stop offset="1" stop-color="rgba(255,184,0,0)" stop-opacity="0" />
									</linearGradient>
								</defs>
							</svg>

							<!-- Bottom label -->
							<div class="flex items-center justify-between mt-2">
								<span class="text-[10px] text-gray-600 font-mono">LATENCY</span>
								<span class="text-[10px] font-mono font-bold" style="color: var(--color-amber);">0.8ms AVG</span>
							</div>
						</div>
						<div class="relative z-10 mt-6 space-y-2 text-center">
							<h2 class="text-lg font-medium text-white">Faster than light</h2>
							<p class="text-sm text-gray-400">Updates deployed in under 2 minutes. Performance cost lower than vanilla overlay.</p>
						</div>
					</div>
					<!-- Spotlight borders -->
					<div class="spotlight-border" />
				</div>

				<!-- Card 4: Shield + Code window -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div
					id="anti-detection"
					class="spotlight-card group relative col-span-full flex flex-col justify-between overflow-hidden lg:col-span-3 rounded-xl border border-white/10 text-card-foreground shadow-sm transition-all duration-300"
					style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice['anti-detection'].x}px; --mouse-y: {cardMice['anti-detection'].y}px;"
					on:mousemove={(e) => handleCardMouseMove(e, 'anti-detection')}
				>
					<div class="grid p-6 sm:grid-cols-2">
						<div class="relative z-10 flex flex-col justify-between space-y-12 lg:space-y-6">
							<div class="relative flex aspect-square size-12 rounded-full border border-white/10 before:absolute before:-inset-2 before:rounded-full before:border before:border-white/5">
								<Shield class="m-auto size-5" style="color: var(--color-amber);" stroke-width={1} />
							</div>
							<div class="space-y-2">
								<h2 class="text-lg font-medium text-white">Anti-Detection Engine</h2>
								<p class="text-sm text-gray-400">Custom polymorphic engine bypasses all known anti-cheat systems automatically.</p>
							</div>
						</div>
						<div class="relative -mb-6 -mr-6 mt-6 h-fit rounded-tl-xl border-l border-t border-white/10 p-6 py-6 sm:ml-6">
							<div class="absolute left-3 top-2 flex gap-1.5">
								<span class="block size-2.5 rounded-full" style="background: #ff5f57;"></span>
								<span class="block size-2.5 rounded-full" style="background: #febc2e;"></span>
								<span class="block size-2.5 rounded-full" style="background: #28c840;"></span>
							</div>
							<!-- Blurred code visualization - no real text -->
							<div class="mt-4 space-y-2.5 select-none" style="user-select: none; -webkit-user-select: none;">
								<div class="flex gap-1.5 items-center">
									<div class="h-2.5 w-8 rounded-sm bg-purple-400/60" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-12 rounded-sm bg-amber-400/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-3 rounded-sm bg-white/20" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-6 rounded-sm bg-green-400/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-16 rounded-sm bg-blue-400/50" style="filter: blur(1px);"></div>
								</div>
								<div class="flex gap-1.5 items-center">
									<div class="h-2.5 w-10 rounded-sm bg-purple-400/60" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-14 rounded-sm bg-amber-400/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-5 rounded-sm bg-white/20" style="filter: blur(1px);"></div>
								</div>
								<div class="flex gap-1.5 items-center pl-4">
									<div class="h-2.5 w-10 rounded-sm bg-gray-500/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-14 rounded-sm bg-green-400/50" style="filter: blur(1px);"></div>
								</div>
								<div class="flex gap-1.5 items-center pl-4">
									<div class="h-2.5 w-12 rounded-sm bg-gray-500/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-7 rounded-sm bg-green-400/50" style="filter: blur(1px);"></div>
								</div>
								<div class="flex gap-1.5 items-center pl-4">
									<div class="h-2.5 w-9 rounded-sm bg-gray-500/50" style="filter: blur(1px);"></div>
									<div class="h-2.5 w-8 rounded-sm bg-red-400/50" style="filter: blur(1px);"></div>
								</div>
							</div>
						</div>
					</div>
					<!-- Spotlight borders -->
					<div class="spotlight-border" />
				</div>

				<!-- Card 5: Low CPU Usage with CPU Architecture SVG -->
				<!-- svelte-ignore a11y-no-static-element-interactions -->
				<div
					id="low-cpu"
					class="spotlight-card group relative col-span-full flex flex-col justify-between overflow-hidden lg:col-span-3 rounded-xl border border-white/10 text-card-foreground shadow-sm transition-all duration-300"
					style="background: linear-gradient(135deg, rgba(12,12,12,0.98) 0%, rgba(4,4,4,0.99) 100%); --mouse-x: {cardMice['low-cpu'].x}px; --mouse-y: {cardMice['low-cpu'].y}px;"
					on:mousemove={(e) => handleCardMouseMove(e, 'low-cpu')}
				>
					<div class="grid h-full p-6 sm:grid-cols-2">
						<div class="relative z-10 flex flex-col justify-between space-y-12 lg:space-y-6">
							<div class="relative flex aspect-square size-12 rounded-full border border-white/10 before:absolute before:-inset-2 before:rounded-full before:border before:border-white/5">
								<Cpu class="m-auto size-6" style="color: var(--color-amber);" stroke-width={1} />
							</div>
							<div class="space-y-2">
								<h2 class="text-lg font-medium text-white">Low CPU Usage</h2>
								<p class="text-sm text-gray-400">Optimized to run with minimal system resources. No FPS drops, no stuttering.</p>
							</div>
						</div>
						<div class="relative mt-6 sm:-my-6 sm:-mr-6 flex items-center justify-center">
							<!-- CPU Architecture SVG -->
							<svg class="text-gray-700 w-full h-full" width="100%" height="100%" viewBox="0 0 200 100">
								<!-- Paths -->
								<g stroke="currentColor" fill="none" stroke-width="0.3" stroke-dasharray="100 100" pathLength="100" marker-start="url(#cpu-circle-marker)">
									<path stroke-dasharray="100 100" pathLength="100" d="M 10 20 h 79.5 q 5 0 5 5 v 30" />
									<path stroke-dasharray="100 100" pathLength="100" d="M 180 10 h -69.7 q -5 0 -5 5 v 30" />
									<path d="M 130 20 v 21.8 q 0 5 -5 5 h -10" />
									<path d="M 170 80 v -21.8 q 0 -5 -5 -5 h -50" />
									<path stroke-dasharray="100 100" pathLength="100" d="M 135 65 h 15 q 5 0 5 5 v 10 q 0 5 -5 5 h -39.8 q -5 0 -5 -5 v -20" />
									<path d="M 94.8 95 v -36" />
									<path d="M 88 88 v -15 q 0 -5 -5 -5 h -10 q -5 0 -5 -5 v -5 q 0 -5 5 -5 h 14" />
									<path d="M 30 30 h 25 q 5 0 5 5 v 6.5 q 0 5 5 5 h 20" />
									<animate attributeName="stroke-dashoffset" from="100" to="0" dur="1s" fill="freeze" calcMode="spline" keySplines="0.25,0.1,0.5,1" keyTimes="0; 1" />
								</g>
								<!-- Light animations -->
								<g mask="url(#cpu-mask-1)"><circle class="cpu-line-1" cx="0" cy="0" r="8" fill="url(#cpu-blue-grad)" /></g>
								<g mask="url(#cpu-mask-2)"><circle class="cpu-line-2" cx="0" cy="0" r="8" fill="url(#cpu-yellow-grad)" /></g>
								<g mask="url(#cpu-mask-3)"><circle class="cpu-line-3" cx="0" cy="0" r="8" fill="url(#cpu-pinkish-grad)" /></g>
								<g mask="url(#cpu-mask-4)"><circle class="cpu-line-4" cx="0" cy="0" r="8" fill="url(#cpu-white-grad)" /></g>
								<g mask="url(#cpu-mask-5)"><circle class="cpu-line-5" cx="0" cy="0" r="8" fill="url(#cpu-green-grad)" /></g>
								<g mask="url(#cpu-mask-6)"><circle class="cpu-line-6" cx="0" cy="0" r="8" fill="url(#cpu-orange-grad)" /></g>
								<g mask="url(#cpu-mask-7)"><circle class="cpu-line-7" cx="0" cy="0" r="8" fill="url(#cpu-cyan-grad)" /></g>
								<g mask="url(#cpu-mask-8)"><circle class="cpu-line-8" cx="0" cy="0" r="8" fill="url(#cpu-rose-grad)" /></g>
								<!-- CPU Box -->
								<g>
									<g fill="url(#cpu-connection-gradient)">
										<rect x="93" y="37" width="2.5" height="5" rx="0.7" />
										<rect x="104" y="37" width="2.5" height="5" rx="0.7" />
										<rect x="116.3" y="44" width="2.5" height="5" rx="0.7" transform="rotate(90 116.25 45.5)" />
										<rect x="122.8" y="44" width="2.5" height="5" rx="0.7" transform="rotate(90 116.25 45.5)" />
										<rect x="104" y="16" width="2.5" height="5" rx="0.7" transform="rotate(180 105.25 39.5)" />
										<rect x="114.5" y="16" width="2.5" height="5" rx="0.7" transform="rotate(180 105.25 39.5)" />
										<rect x="80" y="-13.6" width="2.5" height="5" rx="0.7" transform="rotate(270 115.25 19.5)" />
										<rect x="87" y="-13.6" width="2.5" height="5" rx="0.7" transform="rotate(270 115.25 19.5)" />
									</g>
									<rect x="85" y="40" width="30" height="20" rx="2" fill="#181818" filter="url(#cpu-light-shadow)" />
									<text x="92" y="52.5" font-size="7" fill="url(#cpu-text-gradient)" font-weight="600" letter-spacing="0.05em">CPU</text>
								</g>
								<!-- Defs -->
								<defs>
									<mask id="cpu-mask-1"><path d="M 10 20 h 79.5 q 5 0 5 5 v 24" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-2"><path d="M 180 10 h -69.7 q -5 0 -5 5 v 24" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-3"><path d="M 130 20 v 21.8 q 0 5 -5 5 h -10" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-4"><path d="M 170 80 v -21.8 q 0 -5 -5 -5 h -50" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-5"><path d="M 135 65 h 15 q 5 0 5 5 v 10 q 0 5 -5 5 h -39.8 q -5 0 -5 -5 v -20" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-6"><path d="M 94.8 95 v -36" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-7"><path d="M 88 88 v -15 q 0 -5 -5 -5 h -10 q -5 0 -5 -5 v -5 q 0 -5 5 -5 h 14" stroke-width="0.5" stroke="white" /></mask>
									<mask id="cpu-mask-8"><path d="M 30 30 h 25 q 5 0 5 5 v 6.5 q 0 5 5 5 h 20" stroke-width="0.5" stroke="white" /></mask>
									<radialGradient id="cpu-blue-grad" fx="1"><stop offset="0%" stop-color="#00E8ED" /><stop offset="50%" stop-color="#08F" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-yellow-grad" fx="1"><stop offset="0%" stop-color="#FFD800" /><stop offset="50%" stop-color="#FFD800" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-pinkish-grad" fx="1"><stop offset="0%" stop-color="#830CD1" /><stop offset="50%" stop-color="#FF008B" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-white-grad" fx="1"><stop offset="0%" stop-color="white" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-green-grad" fx="1"><stop offset="0%" stop-color="#22c55e" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-orange-grad" fx="1"><stop offset="0%" stop-color="#f97316" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-cyan-grad" fx="1"><stop offset="0%" stop-color="#06b6d4" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<radialGradient id="cpu-rose-grad" fx="1"><stop offset="0%" stop-color="#f43f5e" /><stop offset="100%" stop-color="transparent" /></radialGradient>
									<filter id="cpu-light-shadow" x="-50%" y="-50%" width="200%" height="200%"><feDropShadow dx="1.5" dy="1.5" stdDeviation="1" flood-color="black" flood-opacity="0.1" /></filter>
									<marker id="cpu-circle-marker" viewBox="0 0 10 10" refX="5" refY="5" markerWidth="18" markerHeight="18">
										<circle cx="5" cy="5" r="2" fill="black" stroke="#232323" stroke-width="0.5">
											<animate attributeName="r" values="0; 3; 2" dur="0.5s" />
										</circle>
									</marker>
									<linearGradient id="cpu-connection-gradient" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#4F4F4F" /><stop offset="60%" stop-color="#121214" /></linearGradient>
									<linearGradient id="cpu-text-gradient" x1="0" y1="0" x2="1" y2="0">
										<stop offset="0%" stop-color="#666666"><animate attributeName="offset" values="-2; -1; 0" dur="5s" repeatCount="indefinite" calcMode="spline" keyTimes="0; 0.5; 1" keySplines="0.4 0 0.2 1; 0.4 0 0.2 1" /></stop>
										<stop offset="25%" stop-color="white"><animate attributeName="offset" values="-1; 0; 1" dur="5s" repeatCount="indefinite" calcMode="spline" keyTimes="0; 0.5; 1" keySplines="0.4 0 0.2 1; 0.4 0 0.2 1" /></stop>
										<stop offset="50%" stop-color="#666666"><animate attributeName="offset" values="0; 1; 2" dur="5s" repeatCount="indefinite" calcMode="spline" keyTimes="0; 0.5; 1" keySplines="0.4 0 0.2 1; 0.4 0 0.2 1" /></stop>
									</linearGradient>
								</defs>
							</svg>
						</div>
					</div>
					<!-- Spotlight borders -->
					<div class="spotlight-border" />
				</div>

			</div>
		</div>
	</div>
</section>

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
