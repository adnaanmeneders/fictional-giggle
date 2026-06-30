<script lang="ts">
  import { cn } from "$lib/utils";

  export let reverse = false;
  export let pauseOnHover = false;
  export let vertical = false;
  export let repeat = 4;
  let className = "";
  export { className as class };
</script>

<div
  class={cn(
    'group flex overflow-hidden p-2 [--duration:40s] [--gap:1.5rem] [gap:var(--gap)]',
    {
      'flex-row': !vertical,
      'flex-col': vertical,
    },
    className
  )}
>
  {#each Array(repeat) as _, i}
    <div
      class={cn(
        !vertical ? 'flex-row [gap:var(--gap)]' : 'flex-col [gap:var(--gap)]',
        'flex shrink-0 justify-around',
        !vertical && 'animate-marquee flex-row',
        vertical && 'animate-marquee-vertical flex-col',
        pauseOnHover && 'group-hover:[animation-play-state:paused]',
        reverse && '[animation-direction:reverse]',
      )}
    >
      <slot />
    </div>
  {/each}
</div>

<style>
  :global(.animate-marquee) {
    animation: marquee var(--duration, 40s) infinite linear;
  }
  :global(.animate-marquee-vertical) {
    animation: marquee-vertical var(--duration, 40s) linear infinite;
  }

  @keyframes marquee {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(calc(-100% - var(--gap, 1.5rem)));
    }
  }

  @keyframes marquee-vertical {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(calc(-100% - var(--gap, 1.5rem)));
    }
  }
</style>
