<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import { cn } from "$lib/utils";

  export let minSize = 0.4;
  export let maxSize = 1.5;
  export let speed = 1;
  export let particleColor = "#FFFFFF";
  export let particleDensity = 120;
  let className = "";
  export { className as class };

  let canvasEl: HTMLCanvasElement;
  let ctx: CanvasRenderingContext2D | null = null;
  let particles: Array<{
    x: number;
    y: number;
    size: number;
    speedX: number;
    speedY: number;
    opacity: number;
    fadeSpeed: number;
  }> = [];
  
  let animationId: number;
  let width = 0;
  let height = 0;

  function initParticles() {
    particles = [];
    const count = Math.floor((width * height) / (1000000 / particleDensity));
    for (let i = 0; i < count; i++) {
      particles.push({
        x: Math.random() * width,
        y: Math.random() * height,
        size: Math.random() * (maxSize - minSize) + minSize,
        speedX: (Math.random() - 0.5) * speed * 0.4,
        speedY: (Math.random() - 0.5) * speed * 0.4,
        opacity: Math.random(),
        fadeSpeed: (Math.random() * 0.01 + 0.002) * speed
      });
    }
  }

  function draw() {
    if (!ctx) return;
    ctx.clearRect(0, 0, width, height);
    ctx.fillStyle = particleColor;

    for (let p of particles) {
      ctx.globalAlpha = p.opacity;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fill();

      // Update positions
      p.x += p.speedX;
      p.y += p.speedY;

      // Opacity fade in/out loop
      p.opacity += p.fadeSpeed;
      if (p.opacity > 1 || p.opacity < 0) {
        p.fadeSpeed = -p.fadeSpeed;
        p.opacity = Math.max(0, Math.min(1, p.opacity));
      }

      // Border wrap
      if (p.x < 0) p.x = width;
      if (p.x > width) p.x = 0;
      if (p.y < 0) p.y = height;
      if (p.y > height) p.y = 0;
    }

    animationId = requestAnimationFrame(draw);
  }

  function handleResize() {
    if (!canvasEl) return;
    const rect = canvasEl.getBoundingClientRect();
    width = rect.width;
    height = rect.height;
    canvasEl.width = width;
    canvasEl.height = height;
    initParticles();
  }

  onMount(() => {
    ctx = canvasEl.getContext("2d");
    handleResize();
    draw();

    window.addEventListener("resize", handleResize);
  });

  onDestroy(() => {
    if (typeof window !== "undefined") {
      window.removeEventListener("resize", handleResize);
      if (animationId) {
        cancelAnimationFrame(animationId);
      }
    }
  });
</script>

<canvas
  bind:this={canvasEl}
  class={cn("block w-full h-full pointer-events-none", className)}
></canvas>
