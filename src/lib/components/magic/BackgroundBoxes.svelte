<script lang="ts">
  import { cn } from "$lib/utils";
  import { Motion } from "svelte-motion";

  let className: any = "";
  export { className as class };
  
  // Grid size
  export let rows = 25;
  export let cols = 20;
  
  let rowArray = new Array(rows).fill(1);
  let colArray = new Array(cols).fill(1);
  
  // Palette of yellow shades for randomized neon highlight effect
  const colors = [
    "rgba(255, 184, 0, 0.15)",
    "rgba(255, 184, 0, 0.20)",
    "rgba(255, 184, 0, 0.10)",
    "rgba(255, 184, 0, 0.25)"
  ];
  
  function getRandomColor() {
    return colors[Math.floor(Math.random() * colors.length)];
  }
</script>

<div class={cn("absolute inset-0 w-full h-full overflow-hidden flex flex-wrap z-0 pointer-events-none", className)}>
  <div 
    class="flex flex-wrap w-[150vw] h-[150vh] absolute -left-[25vw] -top-[25vh] pointer-events-auto"
    style="transform: skewY(-6deg) scale(1.1); transform-origin: top left;"
  >
    {#each rowArray as _, i}
      <div class="flex flex-row">
        {#each colArray as _, j}
          <Motion
            whileHover={{
              backgroundColor: getRandomColor(),
              transition: { duration: 0 }
            }}
            animate={{
              backgroundColor: "rgba(0,0,0,0)",
              transition: { duration: 1.5 }
            }}
            let:motion
          >
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <div
              use:motion
              class="w-16 h-16 border-r border-t border-white/[0.02] flex-shrink-0 transition-colors duration-1000 hover:duration-0 relative"
            >
              <!-- Subtle inner dot for premium look -->
              <span class="absolute top-1 left-1 size-0.5 rounded-full bg-white/[0.03]"></span>
            </div>
          </Motion>
        {/each}
      </div>
    {/each}
  </div>
</div>
