<script lang="ts">
  import { onMount } from "svelte";

  export let guildId = "1327662659205857360";
  let serverInfo: any = null;
  let isLoading = true;
  let useFallback = false;

  onMount(async () => {
    try {
      // Fetch details of invite code "target" (matching footer invite link)
      const res = await fetch("https://discord.com/api/v9/invites/target?with_counts=true");
      if (!res.ok) throw new Error("Invite API failed");
      serverInfo = await res.json();
      if (!serverInfo || !serverInfo.guild) throw new Error("No guild data");
      isLoading = false;
    } catch (err) {
      useFallback = true;
      isLoading = false;
    }
  });
</script>

<div class="w-full max-w-[360px] mx-auto rounded-xl border border-white/10 bg-[#070708] overflow-hidden shadow-2xl">
  {#if isLoading}
    <div class="flex h-[320px] items-center justify-center">
      <div class="text-xs text-amber-500/50 uppercase tracking-widest animate-pulse font-sans">Loading Server Profile...</div>
    </div>
  {:else if useFallback}
    <!-- Cleaned up styled fallback widget iframe -->
    <div class="p-3 bg-black/40 h-[360px]">
      <iframe
        src="https://discord.com/widget?id={guildId}&theme=dark"
        width="100%"
        height="100%"
        allowtransparency="true"
        frameborder="0"
        sandbox="allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts"
        class="rounded-lg opacity-85 hover:opacity-100 transition-opacity duration-300"
        title="Discord Server Widget"
      ></iframe>
    </div>
  {:else}
    <!-- Premium Dotted Server Profile Card layout -->
    <div class="relative h-24 w-full bg-neutral-900 overflow-hidden">
      {#if serverInfo.guild.banner}
        <img
          src="https://cdn.discordapp.com/banners/{serverInfo.guild.id}/{serverInfo.guild.banner}.webp?size=512"
          alt="Server Banner"
          class="w-full h-full object-cover opacity-75"
        />
      {:else}
        <div class="w-full h-full bg-gradient-to-br from-[#1a1c23] to-[#0d0e12]"></div>
      {/if}
      <div class="absolute inset-0 bg-gradient-to-t from-[#070708] to-transparent"></div>
    </div>

    <!-- Server Icon overlapping banner -->
    <div class="px-5 pb-5 relative">
      <div class="absolute -top-8 left-5">
        <div class="size-16 rounded-2xl bg-[#070708] p-0.5 border border-white/10 shadow-2xl overflow-hidden">
          {#if serverInfo.guild.icon}
            <img
              src="https://cdn.discordapp.com/icons/{serverInfo.guild.id}/{serverInfo.guild.icon}.webp?size=128"
              alt="Server Icon"
              class="w-full h-full rounded-xl object-cover"
            />
          {:else}
            <div class="w-full h-full rounded-xl bg-amber-500/10 flex items-center justify-center font-bold text-amber-500 text-lg uppercase">
              {serverInfo.guild.name.slice(0, 2)}
            </div>
          {/if}
        </div>
      </div>

      <!-- Server Details -->
      <div class="pt-10">
        <div class="flex items-center gap-1.5 mb-2">
          <h3 class="text-base font-bold text-white leading-tight tracking-tight">{serverInfo.guild.name}</h3>
          {#if serverInfo.guild.features && (serverInfo.guild.features.includes("VERIFIED") || serverInfo.guild.features.includes("PARTNERED"))}
            <svg class="size-4 text-blue-400 fill-current" viewBox="0 0 24 24">
              <path d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10 10-4.5 10-10S17.5 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
            </svg>
          {/if}
        </div>

        {#if serverInfo.guild.description}
          <p class="text-[11px] text-gray-400 leading-relaxed mb-4">{serverInfo.guild.description}</p>
        {:else}
          <p class="text-[11px] text-gray-400 leading-relaxed mb-4">Official community server. Chat with developers, track status logs, updates, patch notes, and support.</p>
        {/if}

        <!-- Member Stats Row -->
        <div class="flex items-center gap-5 py-3 border-t border-b border-white/5 mb-4">
          <div class="flex flex-col">
            <span class="text-[9px] text-gray-500 uppercase tracking-wider font-semibold">Online</span>
            <div class="flex items-center gap-1.5 mt-0.5">
              <span class="size-1.5 rounded-full bg-green-500 animate-pulse"></span>
              <span class="text-xs font-bold text-white tracking-wide">{serverInfo.approximate_presence_count?.toLocaleString() || 0}</span>
            </div>
          </div>
          <div class="flex flex-col">
            <span class="text-[9px] text-gray-500 uppercase tracking-wider font-semibold">Members</span>
            <div class="flex items-center gap-1.5 mt-0.5">
              <span class="size-1.5 rounded-full bg-gray-600"></span>
              <span class="text-xs font-bold text-white tracking-wide">{serverInfo.approximate_member_count?.toLocaleString() || 0}</span>
            </div>
          </div>
        </div>

        <!-- Join Button -->
        <a
          href="https://discord.gg/target"
          target="_blank"
          rel="noopener noreferrer"
          class="flex w-full items-center justify-center gap-2 rounded-lg py-2.5 text-xs font-bold uppercase tracking-wider text-black transition-all duration-300"
          style="background: var(--color-amber); box-shadow: 0 4px 15px rgba(255, 184, 0, 0.25);"
        >
          <svg class="size-4" viewBox="0 0 127.14 96.36" fill="currentColor">
            <path d="M107.7,8.07A105.15,105.15,0,0,0,77.26,0a77.19,77.19,0,0,0-3.3,6.83A96.67,96.67,0,0,0,53.22,6.83,77.19,77.19,0,0,0,49.88,0,105.15,105.15,0,0,0,19.44,8.07C3.66,31.58-1.86,54.65,1,77.53A105.73,105.73,0,0,0,32,96.36a77.7,77.7,0,0,0,6.63-10.85,68.43,68.43,0,0,1-10.4-5c.87-.64,1.71-1.32,2.51-2a75.76,75.76,0,0,0,72.68,0c.8,0.7,1.64,1.38,2.51,2a68.43,68.43,0,0,1-10.4,5,77.7,77.7,0,0,0,6.63,10.85,105.73,105.73,0,0,0,31-18.83C129,54.65,122.94,31.58,107.7,8.07ZM42.45,65.69C36.18,65.69,31,60,31,53S36.18,40.36,42.45,40.36,53.9,46,53.9,53,48.72,65.69,42.45,65.69Zm42.24,0C78.41,65.69,73.24,60,73.24,53S78.41,40.36,84.69,40.36,96.14,46,96.14,53,91,65.69,84.69,65.69Z"/>
          </svg>
          Join Discord
        </a>
      </div>
    </div>
  {/if}
</div>
