<script>
	import { seo } from '$lib';
	import CtaSection from '$lib/components/landing/CtaSection.svelte';
	import HeroSection from '$lib/components/landing/HeroSection.svelte';
	import PricingSection from '$lib/components/landing/PricingSection.svelte';
	import FeaturesSection from '$lib/components/landing/FeaturesSection.svelte';
	import BentoFeatures from '$lib/components/landing/BentoFeatures.svelte';
	import SphereMask from '$lib/components/magic/SphereMask/SphereMask.svelte';
	import BlurFade from '$lib/components/magic/BlurFade.svelte';
</script>

<svelte:head>
	<title>{seo.title}</title>
	<meta name="description" content={seo.description} />
	<meta name="keywords" content={seo.keywords} />

	<meta property="og:title" content={seo.title} />
	<meta property="og:description" content={seo.description} />
	<meta property="og:image" content={seo.image} />
	<meta property="og:site_name" content={seo.title} />
	<meta property="og:url" content={seo.url} />

	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:title" content={seo.title} />
	<meta name="twitter:description" content={seo.description} />
	<meta name="twitter:image" content={seo.image} />
	<meta name="twitter:site" content="@targetpriv" />
</svelte:head>

<BlurFade delay={0.1} once={true}>
	<HeroSection />
</BlurFade>

<BlurFade delay={0.2} once={true}>
	<SphereMask />
</BlurFade>

<BlurFade delay={0.3} once={true}>
	<BentoFeatures />
</BlurFade>

<BlurFade delay={0.4} once={true}>
	<PricingSection />
</BlurFade>

<BlurFade delay={0.5} once={true}>
	<CtaSection />
</BlurFade>

<BlurFade delay={0.6} once={true}>
	<FeaturesSection />
</BlurFade>

