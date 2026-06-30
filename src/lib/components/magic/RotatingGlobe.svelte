<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import * as d3 from "d3";
  import { cn } from "$lib/utils";

  export let width = 320;
  export let height = 320;
  let className = "";
  export { className as class };

  let canvasEl: HTMLCanvasElement;
  let isLoading = true;
  let error: string | null = null;
  let landFeatures: any;
  const allDots: Array<{ lng: number; lat: number }> = [];

  let rotationTimer: d3.Timer;

  // Set up rotation and speed
  const rotation = [0, -15];
  let autoRotate = true;
  const rotationSpeed = 0.22;

  // Geographic connection arcs (NY, Paris, Tokyo, London, Shanghai, Sydney, Rio)
  interface ConnectionLink {
    start: [number, number];
    end: [number, number];
    progress: number;
    speed: number;
    delay: number;
  }

  const links: ConnectionLink[] = [
    { start: [-74.006, 40.7128], end: [2.3522, 48.8566], progress: 0, speed: 0.012, delay: 0 },
    { start: [2.3522, 48.8566], end: [139.6917, 35.6762], progress: 0, speed: 0.010, delay: 40 },
    { start: [139.6917, 35.6762], end: [151.2093, -33.8688], progress: 0, speed: 0.014, delay: 80 },
    { start: [151.2093, -33.8688], end: [-74.006, 40.7128], progress: 0, speed: 0.011, delay: 120 },
    { start: [37.6173, 55.7558], end: [121.4737, 31.2304], progress: 0, speed: 0.015, delay: 20 },
    { start: [-0.1278, 51.5074], end: [-43.1729, -22.9068], progress: 0, speed: 0.009, delay: 60 }
  ];

  onMount(() => {
    if (!canvasEl) return;
    const context = canvasEl.getContext("2d");
    if (!context) return;

    // Set up responsive dimensions
    const containerWidth = width;
    const containerHeight = height;
    const radius = Math.min(containerWidth, containerHeight) / 2.2;

    const dpr = window.devicePixelRatio || 1;
    canvasEl.width = containerWidth * dpr;
    canvasEl.height = containerHeight * dpr;
    canvasEl.style.width = `${containerWidth}px`;
    canvasEl.style.height = `${containerHeight}px`;
    context.scale(dpr, dpr);

    // Create orthographic globe projection
    const projection = d3
      .geoOrthographic()
      .scale(radius)
      .translate([containerWidth / 2, containerHeight / 2])
      .clipAngle(90);

    const path = d3.geoPath().projection(projection).context(context);

    function pointInPolygon(point: [number, number], polygon: number[][]): boolean {
      const [x, y] = point;
      let inside = false;
      for (let i = 0, j = polygon.length - 1; i < polygon.length; j = i++) {
        const [xi, yi] = polygon[i];
        const [xj, yj] = polygon[j];
        if (yi > y !== yj > y && x < ((xj - xi) * (y - yi)) / (yj - yi) + xi) {
          inside = !inside;
        }
      }
      return inside;
    }

    function pointInFeature(point: [number, number], feature: any): boolean {
      const geometry = feature.geometry;
      if (geometry.type === "Polygon") {
        const coordinates = geometry.coordinates;
        if (!pointInPolygon(point, coordinates[0])) return false;
        for (let i = 1; i < coordinates.length; i++) {
          if (pointInPolygon(point, coordinates[i])) return false;
        }
        return true;
      } else if (geometry.type === "MultiPolygon") {
        for (const polygon of geometry.coordinates) {
          if (pointInPolygon(point, polygon[0])) {
            let inHole = false;
            for (let i = 1; i < polygon.length; i++) {
              if (pointInPolygon(point, polygon[i])) {
                inHole = true;
                break;
              }
            }
            if (!inHole) return true;
          }
        }
        return false;
      }
      return false;
    }

    function generateDotsInPolygon(feature: any, dotSpacing = 16) {
      const dots: [number, number][] = [];
      const bounds = d3.geoBounds(feature);
      const [[minLng, minLat], [maxLng, maxLat]] = bounds;
      const stepSize = dotSpacing * 0.12;

      for (let lng = minLng; lng <= maxLng; lng += stepSize) {
        for (let lat = minLat; lat <= maxLat; lat += stepSize) {
          const point: [number, number] = [lng, lat];
          if (pointInFeature(point, feature)) {
            dots.push(point);
          }
        }
      }
      return dots;
    }

    function render() {
      if (!context) return;
      context.clearRect(0, 0, containerWidth, containerHeight);
      const currentScale = projection.scale();
      const scaleFactor = currentScale / radius;

      // Draw globe ocean background sphere outline
      context.beginPath();
      context.arc(containerWidth / 2, containerHeight / 2, currentScale, 0, 2 * Math.PI);
      context.fillStyle = "rgba(0, 0, 0, 0.65)";
      context.fill();
      context.strokeStyle = "rgba(255, 255, 255, 0.08)";
      context.lineWidth = 1 * scaleFactor;
      context.stroke();

      if (landFeatures) {
        // Draw graticule grid lines
        const graticule = d3.geoGraticule()();
        context.beginPath();
        path(graticule);
        context.strokeStyle = "rgba(255, 184, 0, 0.04)";
        context.lineWidth = 0.5 * scaleFactor;
        context.stroke();

        // Draw land outlines
        context.beginPath();
        landFeatures.features.forEach((feature: any) => {
          path(feature);
        });
        context.strokeStyle = "rgba(255, 255, 255, 0.04)";
        context.lineWidth = 0.8 * scaleFactor;
        context.stroke();

        // Draw dots over land features (visible front hemisphere only)
        const center = projection.invert!([containerWidth / 2, containerHeight / 2]);
        allDots.forEach((dot) => {
          const projected = projection([dot.lng, dot.lat]);
          if (
            projected &&
            projected[0] >= 0 &&
            projected[0] <= containerWidth &&
            projected[1] >= 0 &&
            projected[1] <= containerHeight
          ) {
            const distance = d3.geoDistance([dot.lng, dot.lat], center);
            if (distance < Math.PI / 2) {
              context.beginPath();
              context.arc(projected[0], projected[1], 1.25 * scaleFactor, 0, 2 * Math.PI);
              context.fillStyle = "rgba(255, 184, 0, 0.65)";
              context.fill();
            }
          }
        });

        // Draw connection links (curves passing from one place to another)
        links.forEach((link) => {
          if (link.delay > 0) return;
          const interpolator = d3.geoInterpolate(link.start, link.end);
          const steps = 30;

          context.beginPath();
          let first = true;
          for (let s = 0; s <= steps; s++) {
            const pt = interpolator(s / steps);
            const distance = d3.geoDistance(pt, center);
            
            // Draw segment only if it is on the visible front side
            if (distance < Math.PI / 2) {
              const projected = projection(pt);
              if (projected) {
                if (first) {
                  context.moveTo(projected[0], projected[1]);
                  first = false;
                } else {
                  context.lineTo(projected[0], projected[1]);
                }
              }
            }
          }
          context.strokeStyle = "rgba(255, 184, 0, 0.28)";
          context.lineWidth = 1.2 * scaleFactor;
          context.stroke();

          // Draw traveling glow dot along the curve
          const currentPos = interpolator(link.progress);
          const distance = d3.geoDistance(currentPos, center);
          if (distance < Math.PI / 2) {
            const projectedPos = projection(currentPos);
            if (projectedPos) {
              context.beginPath();
              context.arc(projectedPos[0], projectedPos[1], 2.5 * scaleFactor, 0, 2 * Math.PI);
              context.fillStyle = "#FFB800";
              context.shadowColor = "#FFB800";
              context.shadowBlur = 4 * scaleFactor;
              context.fill();
              context.shadowBlur = 0;
            }
          }
        });
      }
    }

    async function loadWorldData() {
      try {
        isLoading = true;
        const response = await fetch(
          "https://raw.githubusercontent.com/martynafford/natural-earth-geojson/refs/heads/master/110m/physical/ne_110m_land.json"
        );
        if (!response.ok) throw new Error("Failed to load map");
        landFeatures = await response.json();

        landFeatures.features.forEach((feature: any) => {
          const dots = generateDotsInPolygon(feature, 16);
          dots.forEach(([lng, lat]) => {
            allDots.push({ lng, lat });
          });
        });

        render();
        isLoading = false;
      } catch (err) {
        error = "Map loading failed";
        isLoading = false;
      }
    }

    function rotate() {
      if (autoRotate) {
        rotation[0] += rotationSpeed;
        projection.rotate([rotation[0], rotation[1]]);

        // Update animated connection arcs progress
        links.forEach((link) => {
          if (link.delay > 0) {
            link.delay--;
          } else {
            link.progress += link.speed;
            if (link.progress > 1) {
              link.progress = 0;
              link.delay = Math.random() * 80 + 30; // Delay before next trigger
            }
          }
        });

        render();
      }
    }

    rotationTimer = d3.timer(rotate);

    function handleMouseDown(event: MouseEvent) {
      autoRotate = false;
      const startX = event.clientX;
      const startY = event.clientY;
      const startRotation = [...rotation];

      function handleMouseMove(moveEvent: MouseEvent) {
        const sensitivity = 0.25;
        const dx = moveEvent.clientX - startX;
        const dy = moveEvent.clientY - startY;

        rotation[0] = startRotation[0] + dx * sensitivity;
        rotation[1] = startRotation[1] - dy * sensitivity;
        rotation[1] = Math.max(-90, Math.min(90, rotation[1]));

        projection.rotate([rotation[0], rotation[1]]);
        render();
      }

      function handleMouseUp() {
        document.removeEventListener("mousemove", handleMouseMove);
        document.removeEventListener("mouseup", handleMouseUp);
        setTimeout(() => {
          autoRotate = true;
        }, 1500);
      }

      document.addEventListener("mousemove", handleMouseMove);
      document.addEventListener("mouseup", handleMouseUp);
    }

    canvasEl.addEventListener("mousedown", handleMouseDown);
    loadWorldData();

    return () => {
      rotationTimer.stop();
      if (canvasEl) {
        canvasEl.removeEventListener("mousedown", handleMouseDown);
      }
    };
  });
</script>

<div class={cn("relative flex items-center justify-center select-none touch-none", className)}>
  <canvas
    bind:this={canvasEl}
    class="block bg-transparent cursor-grab active:cursor-grabbing"
  />
  {#if isLoading}
    <div class="absolute inset-0 flex items-center justify-center text-xs text-amber-500/50 uppercase font-mono tracking-widest">
      Rendering Dotted Globe...
    </div>
  {/if}
</div>
