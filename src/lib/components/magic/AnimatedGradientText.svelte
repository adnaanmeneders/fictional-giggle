<script lang="ts">
  import { cn } from "$lib/utils";

  let className = "";
  export { className as class };
</script>

<div
  class={cn(
    "group relative mx-auto flex max-w-fit flex-row items-center justify-center rounded-full bg-black/40 px-4 py-1.5 text-xs font-semibold shadow-[inset_0_-8px_10px_#ffb8000a] backdrop-blur-sm transition-shadow duration-500 ease-out [--bg-size:300%] hover:shadow-[inset_0_-5px_10px_#ffb8001a] border border-white/5",
    className
  )}
>
  <!-- Animated border overlay -->
  <div
    class="absolute inset-0 block h-full w-full animate-gradient bg-gradient-to-r from-[#FFB800]/30 via-[#FFA100]/60 to-[#FFB800]/30 bg-[length:var(--bg-size)_100%] p-[1px] ![mask-composite:subtract] [border-radius:inherit] [mask:linear-gradient(#fff_0_0)_content-box,linear-gradient(#fff_0_0)]"
  />

  <slot>Gradient</slot>
</div>

<style>
  :global(.animate-gradient) {
    animation: gradient 6s linear infinite;
  }

  @keyframes gradient {
    0% {
      background-position: 0% center;
    }
    100% {
      background-position: 200% center;
    }
  }
</style>
