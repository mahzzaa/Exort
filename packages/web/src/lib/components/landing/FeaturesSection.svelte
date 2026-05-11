<script lang="ts">
  import { onMount } from "svelte";
  import { fade } from "svelte/transition";
  import AppFrame from "$lib/components/landing/AppFrame.svelte";

  type FeatureItem = {
    title: string;
    description: string;
    video: string;
  };

  let { features }: { features: FeatureItem[] } = $props();

  let prefersReducedMotion = $state(false);
  let activeFeatureIndex = $state(0);
  let featureVideoEl = $state<HTMLVideoElement | null>(null);
  let featureVideoDuration = $state(0);
  let featureVideoCurrentTime = $state(0);
  let featuresSection: HTMLElement | null = null;
  let featuresIntroEl: HTMLElement | null = null;
  let featureSidebarEl: HTMLElement | null = null;
  let featurePanelEl: HTMLElement | null = null;
  let gsapRef: Awaited<typeof import("gsap")>["gsap"] | null = null;
  const videoProgressRadius = 20;
  const videoProgressCircumference = 2 * Math.PI * videoProgressRadius;

  const remainingVideoTime = $derived.by(() =>
    Math.max(0, featureVideoDuration - featureVideoCurrentTime),
  );

  const videoProgress = $derived.by(() =>
    featureVideoDuration > 0
      ? Math.min(featureVideoCurrentTime / featureVideoDuration, 1)
      : 0,
  );

  const videoProgressOffset = $derived.by(
    () => videoProgressCircumference * (1 - videoProgress),
  );

  const setActiveFeature = (index: number) => {
    featureVideoCurrentTime = 0;
    featureVideoDuration = 0;
    activeFeatureIndex = index;
  };

  const showPreviousFeature = () => {
    setActiveFeature(
      (activeFeatureIndex - 1 + features.length) % features.length,
    );
  };

  const showNextFeature = () => {
    setActiveFeature((activeFeatureIndex + 1) % features.length);
  };

  const handleFeatureVideoEnded = () => {
    showNextFeature();
  };

  const syncFeatureVideoProgress = () => {
    if (!featureVideoEl) {
      featureVideoCurrentTime = 0;
      return;
    }

    featureVideoCurrentTime = featureVideoEl.currentTime;
  };

  const syncFeatureVideoDuration = () => {
    if (!featureVideoEl) {
      featureVideoDuration = 0;
      return;
    }

    featureVideoDuration = Number.isFinite(featureVideoEl.duration)
      ? featureVideoEl.duration
      : 0;
  };

  const formatRemainingTime = (seconds: number) => {
    const roundedSeconds = Math.max(0, Math.ceil(seconds));
    const minutes = Math.floor(roundedSeconds / 60);
    const remainingSeconds = roundedSeconds % 60;

    return `${minutes}:${remainingSeconds.toString().padStart(2, "0")}`;
  };

  const ensureGsap = async () => {
    if (gsapRef) {
      return gsapRef;
    }

    const { gsap } = await import("gsap");
    gsapRef = gsap;
    return gsap;
  };

  onMount(() => {
    if (typeof window === "undefined") {
      return;
    }

    const mediaQuery = window.matchMedia("(prefers-reduced-motion: reduce)");
    prefersReducedMotion = mediaQuery.matches;

    const syncReducedMotion = (event: MediaQueryListEvent) => {
      prefersReducedMotion = event.matches;
    };

    mediaQuery.addEventListener("change", syncReducedMotion);

    let cleanupAnimations = () => {};

    const loadAnimations = async () => {
      if (mediaQuery.matches) {
        return;
      }

      const gsap = await ensureGsap();
      const { ScrollTrigger } = await import("gsap/ScrollTrigger");

      gsap.registerPlugin(ScrollTrigger);

      const ctx = gsap.context(() => {
        gsap.set(
          [featuresIntroEl, featureSidebarEl, featurePanelEl].filter(Boolean),
          {
            willChange: "transform, opacity",
          },
        );

        if (
          featuresSection &&
          (featuresIntroEl || featureSidebarEl || featurePanelEl)
        ) {
          const featuresTimeline = gsap.timeline({
            defaults: {
              duration: 0.75,
              ease: "power3.out",
            },
            scrollTrigger: {
              trigger: featuresSection,
              start: "top 70%",
              once: true,
              invalidateOnRefresh: true,
            },
          });

          if (featuresIntroEl) {
            featuresTimeline.from(
              featuresIntroEl,
              {
                x: -32,
                opacity: 0,
              },
              0,
            );
          }

          if (featurePanelEl) {
            featuresTimeline.from(
              featurePanelEl,
              {
                x: -48,
                opacity: 0,
              },
              0.08,
            );
          }

          if (featureSidebarEl) {
            featuresTimeline.from(
              featureSidebarEl,
              {
                x: 40,
                opacity: 0,
              },
              0.2,
            );
          }
        }
      }, featuresSection ?? undefined);

      cleanupAnimations = () => {
        ctx.revert();
      };
    };

    void loadAnimations();

    return () => {
      mediaQuery.removeEventListener("change", syncReducedMotion);
      cleanupAnimations();
    };
  });
</script>

<section
  id="features"
  bind:this={featuresSection}
  class="mx-auto max-w-6xl px-6 py-16 lg:px-8 lg:py-24"
>
  <div bind:this={featuresIntroEl} class="flex w-full justify-center">
    <span class="text-sm uppercase tracking-[0.24em] text-gruvbox-accent-soft">
      Core Features
    </span>
  </div>

  <div class="mt-12">
    <div
      class="grid gap-6 lg:grid-cols-[minmax(0,1fr)_18rem] lg:items-center lg:gap-8"
    >
      <div bind:this={featurePanelEl} class="min-w-0">
        <div class="grid gap-6">
          <div class="relative mx-auto w-full max-w-4xl">
            <AppFrame contentClass="p-2 sm:p-3" showIndicator={true}>
              {#snippet indicator()}
                <div
                  class="flex items-center gap-3 rounded-full px-3 py-2 text-gruvbox-fg0"
                >
                  <div class="relative h-8 w-8 shrink-0">
                    <svg
                      class="-rotate-90 h-full w-full"
                      viewBox="0 0 48 48"
                      aria-hidden="true"
                    >
                      <circle
                        cx="24"
                        cy="24"
                        r={videoProgressRadius}
                        fill="none"
                        stroke="rgba(251, 241, 199, 0.14)"
                        stroke-width="3.5"
                      ></circle>
                      <circle
                        cx="24"
                        cy="24"
                        r={videoProgressRadius}
                        fill="none"
                        stroke="currentColor"
                        stroke-linecap="round"
                        stroke-width="3.5"
                        stroke-dasharray={videoProgressCircumference}
                        stroke-dashoffset={videoProgressOffset}
                      ></circle>
                    </svg>
                    <div
                      class="absolute inset-0 flex items-center justify-center text-[10px] font-medium tracking-[0.08em] text-gruvbox-fg0"
                    >
                      {Math.max(0, Math.ceil(remainingVideoTime))}
                    </div>
                  </div>
                </div>
              {/snippet}
              <div
                class="group/feature-video relative aspect-[16/10] w-full overflow-hidden bg-gruvbox-ink-strong"
                aria-label={`${features[activeFeatureIndex].title} feature preview`}
                style="border-radius: 10px !important;"
              >
                {#key activeFeatureIndex}
                  <div
                    class="absolute inset-0"
                    in:fade={{ duration: prefersReducedMotion ? 0 : 180 }}
                    out:fade={{ duration: prefersReducedMotion ? 0 : 140 }}
                  >
                    <video
                      bind:this={featureVideoEl}
                      src={features[activeFeatureIndex].video}
                      aria-label={`${features[activeFeatureIndex].title} demo video`}
                      class="block h-full w-full object-contain object-top"
                      autoplay
                      onloadedmetadata={syncFeatureVideoDuration}
                      ondurationchange={syncFeatureVideoDuration}
                      ontimeupdate={syncFeatureVideoProgress}
                      onended={handleFeatureVideoEnded}
                      muted
                      playsinline
                      preload="metadata"
                    ></video>
                  </div>
                {/key}
                <div
                  class="pointer-events-none absolute inset-0 translate-x-full bg-feature-overlay opacity-0 backdrop-blur-md transition-[transform,opacity] duration-300 ease-out group-hover/feature-video:translate-x-0 group-hover/feature-video:opacity-100 motion-reduce:translate-x-0 motion-reduce:transition-none"
                  aria-hidden="true"
                ></div>
                <div
                  class="pointer-events-none absolute inset-0 flex items-center justify-center p-4 sm:p-6"
                >
                  <div
                    class="w-full max-w-sm translate-x-10 opacity-0 transition-[transform,opacity] duration-300 ease-out group-hover/feature-video:translate-x-0 group-hover/feature-video:opacity-100 motion-reduce:translate-x-0 motion-reduce:opacity-100 motion-reduce:transition-none"
                  >
                    <div
                      class="aspect-square bg-gruvbox-ink px-5 py-4 shadow-card-md backdrop-blur-sm sm:px-6 sm:py-5"
                    >
                      <div
                        class="relative flex h-full flex-col items-start justify-center text-start"
                      >
                        <p
                          class="absolute top-10 left-0 text-[0.68rem] uppercase tracking-[0.34em] text-gruvbox-accent-soft"
                        >
                          Active Capability
                        </p>
                        <h3
                          class=" text-2xl font-light tracking-[-0.04em] text-gruvbox-fg0 sm:text-3xl"
                        >
                          {features[activeFeatureIndex].title}
                        </h3>
                        <p
                          class="mt-4 text-sm leading-7 text-gruvbox-fg1 sm:text-base text-start"
                        >
                          {features[activeFeatureIndex].description}
                        </p>
                        <img
                          src="/exort-logo.png"
                          alt=""
                          aria-hidden="true"
                          class="absolute bottom-0 right-0 h-9 w-9 object-contain"
                        />
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </AppFrame>
          </div>
        </div>
      </div>

      <div class="mt-1.5 flex items-center justify-center gap-2.5 lg:hidden">
        {#each features as _, index}
          <button
            type="button"
            aria-label={`Show feature ${index + 1}`}
            aria-pressed={index === activeFeatureIndex}
            class={`h-0.5 w-4 rounded-full transition-colors ${
              index === activeFeatureIndex
                ? "bg-gruvbox-accent"
                : "bg-gruvbox-muted/70"
            }`}
            onclick={() => setActiveFeature(index)}
          ></button>
        {/each}
      </div>

      <aside bind:this={featureSidebarEl} class="hidden lg:block">
        <div class="sticky top-24 space-y-3">
          {#each features as feature, index}
            <button
              type="button"
              class={`group relative w-full bg-transparent py-[0.7rem] pr-[14px] text-right text-base font-light leading-[1.35] transition-[color,transform,opacity] duration-200 ease-out motion-reduce:transition-none ${
                index === activeFeatureIndex
                  ? "font-semibold text-gruvbox-accent "
                  : "text-gruvbox-muted hover:-translate-x-1 hover:text-gruvbox-text"
              }`}
              onclick={() => setActiveFeature(index)}
              aria-pressed={index === activeFeatureIndex}
            >
              <span
                class={`absolute right-0 top-1/2 h-1.5 w-px -translate-y-1/2 transition-opacity duration-200 ease-out motion-reduce:transition-none ${
                  index === activeFeatureIndex
                    ? "bg-current opacity-100"
                    : "bg-current opacity-70"
                }`}
                aria-hidden="true"
              ></span>
              {feature.title}
            </button>
          {/each}
        </div>
      </aside>
    </div>
  </div>
</section>
