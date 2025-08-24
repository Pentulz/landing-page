<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from "vue";
import { useWindowScroll } from "@vueuse/core";
import { useMotion } from "@vueuse/motion";
import { useHead } from "#imports";
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  Github,
  Shield,
  Target,
  CheckCircle,
  ArrowRight,
  Clock,
  Database,
  Menu,
} from "lucide-vue-next";

// Setup motion for elements
const heroBadge = ref(null);
const heroTitle = ref(null);
const heroDescription = ref(null);
const heroButtons = ref(null);
const heroImage = ref(null);

// Navbar scroll state
const isScrolled = ref(false);
const { y } = useWindowScroll();

// Watch scroll position with throttling for better performance
let ticking = false;
watch(y, (newY) => {
  if (!ticking) {
    requestAnimationFrame(() => {
      isScrolled.value = newY > 100;
      ticking = false;
    });
    ticking = true;
  }
});

// Track current section for navigation highlighting
const currentSection = ref("");

// Smooth scroll to section without updating URL
const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId);
  if (element) {
    element.scrollIntoView({
      behavior: "smooth",
      block: "start",
    });
  }
};

// Initialize motion instances
onMounted(() => {
  if (heroBadge.value) {
    useMotion(heroBadge.value, {
      initial: { opacity: 0, y: 20 },
      enter: { opacity: 1, y: 0, transition: { duration: 600, delay: 200 } },
    });
  }

  if (heroTitle.value) {
    useMotion(heroTitle.value, {
      initial: { opacity: 0, y: 30 },
      enter: { opacity: 1, y: 0, transition: { duration: 800, delay: 400 } },
    });
  }

  if (heroDescription.value) {
    useMotion(heroDescription.value, {
      initial: { opacity: 0, y: 20 },
      enter: { opacity: 1, y: 0, transition: { duration: 600, delay: 600 } },
    });
  }

  if (heroButtons.value) {
    useMotion(heroButtons.value, {
      initial: { opacity: 0, y: 20 },
      enter: { opacity: 1, y: 0, transition: { duration: 600, delay: 800 } },
    });
  }

  if (heroImage.value) {
    useMotion(heroImage.value, {
      initial: { opacity: 0, y: 40 },
      enter: { opacity: 1, y: 0, transition: { duration: 1000, delay: 1000 } },
    });
  }

  // Animate stats and cards with a simple delay-based approach
  document.querySelectorAll(".stat-item").forEach((el, index) => {
    useMotion(el, {
      initial: { opacity: 0, y: 20 },
      enter: {
        opacity: 1,
        y: 0,
        transition: { duration: 600, delay: 1200 + index * 100 },
      },
    });
  });

  document.querySelectorAll(".benefit-card").forEach((el, index) => {
    useMotion(el, {
      initial: { opacity: 0, y: 30 },
      enter: {
        opacity: 1,
        y: 0,
        transition: { duration: 800, delay: 1500 + index * 200 },
      },
    });
  });

  // Setup Intersection Observer for navigation highlighting only
  let observer = null;

  const sections = document.querySelectorAll("section[id]");
  if (sections.length > 0) {
    const observerOptions = {
      rootMargin: "-50% 0px -50% 0px",
      threshold: 0,
    };

    observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting && entry.target.id) {
          currentSection.value = entry.target.id;
        }
      });
    }, observerOptions);

    sections.forEach((section) => {
      observer?.observe(section);
    });
  }

  onBeforeUnmount(() => {
    observer?.disconnect();
  });
});

useHead({
  title:
    "Pentulz - Orchestrate Security Testing at Scale | Open Source Pentest Platform",
  meta: [
    {
      name: "description",
      content:
        "Pentulz orchestrates pentest tools across distributed agents, centralizing findings in a unified dashboard. Accelerate security audits by 80% with automated tool execution and consistent JSON reporting. Open source security orchestration platform.",
    },
    {
      name: "keywords",
      content:
        "pentesting, security testing, vulnerability assessment, cybersecurity, penetration testing, security orchestration, open source, distributed agents, security automation",
    },
    { name: "author", content: "Pentulz Team" },
    { name: "robots", content: "index, follow" },
    { name: "viewport", content: "width=device-width, initial-scale=1.0" },

    // Open Graph
    {
      property: "og:title",
      content: "Pentulz - Orchestrate Security Testing at Scale",
    },
    {
      property: "og:description",
      content:
        "Open source security orchestration platform. Run pentest tools across distributed agents with unified reporting. Accelerate audits by 80%.",
    },
    { property: "og:type", content: "website" },
    { property: "og:url", content: "https://pentulz.xyz" },
    {
      property: "og:image",
      content: "https://pentulz.xyz/mockups/preview.png",
    },
    { property: "og:site_name", content: "Pentulz" },

    // Twitter Card
    { name: "twitter:card", content: "summary_large_image" },
    {
      name: "twitter:title",
      content: "Pentulz - Orchestrate Security Testing at Scale",
    },
    {
      name: "twitter:description",
      content:
        "Open source security orchestration platform for distributed pentest execution.",
    },
    {
      name: "twitter:image",
      content: "https://pentulz.xyz/mockups/preview.png",
    },

    // Additional SEO
    { name: "theme-color", content: "#000000" },
    { name: "msapplication-TileColor", content: "#000000" },
  ],
  link: [
    { rel: "canonical", href: "https://pentulz.xyz" },
    { rel: "icon", type: "image/png", href: "/logo.png" },
    { rel: "apple-touch-icon", href: "/logo.png" },
    { rel: "preload", href: "/mockups/preview.png", as: "image" },
  ],
  script: [
    {
      type: "application/ld+json",
      children: JSON.stringify({
        "@context": "https://schema.org",
        "@type": "SoftwareApplication",
        name: "Pentulz",
        description:
          "Open source security orchestration platform that orchestrates pentest tools across distributed agents with unified reporting.",
        url: "https://pentulz.xyz",
        applicationCategory: "SecurityApplication",
        operatingSystem: "Cross-platform",
        license: "https://opensource.org/licenses",
        author: {
          "@type": "Organization",
          name: "Pentulz Team",
        },
        offers: {
          "@type": "Offer",
          price: "0",
          priceCurrency: "USD",
        },
      }),
    },
  ],
});
</script>

<template>
  <div class="min-h-screen bg-background overflow-x-hidden">
    <!-- Header -->
    <header
      class="fixed top-0 z-50 w-full transition-all duration-300 ease-out gpu-accelerated"
      :class="{
        'py-6': isScrolled,
        'py-2': !isScrolled,
      }"
    >
      <div
        class="container mx-auto px-4 sm:px-6 transition-all duration-300 ease-in-out"
      >
        <div
          class="transition-all duration-300 ease-in-out flex items-center justify-between bg-card/80 backdrop-blur-lg border border-border/30 rounded-xl shadow-md mx-auto"
          :class="{
            'px-4 py-3 shadow-lg rounded-2xl bg-card/95 border-border/50 max-w-xl md:max-w-2xl':
              isScrolled,
            'px-4 py-4 shadow-md rounded-xl bg-card/80 border-border/30 max-w-4xl':
              !isScrolled,
          }"
        >
          <div class="flex items-center space-x-2 sm:space-x-3">
            <img
              src="/logo.png"
              alt="Pentulz Logo"
              class="object-contain transition-all duration-300"
              :class="{
                'h-7 w-7 sm:h-8 sm:w-8': isScrolled,
                'h-9 w-9 sm:h-10 sm:w-10': !isScrolled,
              }"
              width="40"
              height="40"
            />
            <span
              class="font-bold text-foreground transition-all duration-300"
              :class="{
                'text-lg sm:text-xl': isScrolled,
                'text-xl sm:text-2xl': !isScrolled,
              }"
              >Pentulz</span
            >
          </div>

          <nav
            class="hidden md:flex items-center transition-all duration-300"
            :class="{
              'space-x-6': isScrolled,
              'space-x-8': !isScrolled,
            }"
          >
            <button
              class="text-base font-medium transition-colors relative"
              :class="
                currentSection === 'benefits'
                  ? 'text-primary'
                  : 'text-muted-foreground hover:text-foreground'
              "
              aria-label="Navigate to Features section"
              @click="scrollToSection('benefits')"
            >
              Features
              <span
                v-if="currentSection === 'benefits'"
                class="absolute -bottom-1 left-0 w-full h-0.5 bg-primary rounded-full transition-all duration-300"
              />
            </button>
            <button
              class="text-base font-medium transition-colors relative"
              :class="
                currentSection === 'how-it-works'
                  ? 'text-primary'
                  : 'text-muted-foreground hover:text-foreground'
              "
              aria-label="Navigate to How It Works section"
              @click="scrollToSection('how-it-works')"
            >
              How It Works
              <span
                v-if="currentSection === 'how-it-works'"
                class="absolute -bottom-1 left-0 w-full h-0.5 bg-primary rounded-full transition-all duration-300"
              />
            </button>
          </nav>

          <div class="flex items-center space-x-1 sm:space-x-2">
            <!-- Mobile dropdown menu (only when not scrolled) -->
            <div v-if="!isScrolled" class="flex sm:hidden relative">
              <DropdownMenu>
                <DropdownMenuTrigger as-child>
                  <Button
                    variant="ghost"
                    size="default"
                    class="px-3 py-2 hover:bg-muted/50"
                  >
                    <Menu class="h-5 w-5" />
                  </Button>
                </DropdownMenuTrigger>
                <DropdownMenuContent
                  align="end"
                  side="bottom"
                  class="w-56 bg-card/95 backdrop-blur-lg border border-border/50 rounded-xl shadow-lg mt-2"
                >
                  <div class="p-2">
                    <DropdownMenuItem
                      class="cursor-pointer rounded-lg px-4 py-3 text-base font-medium hover:bg-primary/10 focus:bg-primary/10"
                      @click="scrollToSection('benefits')"
                    >
                      Features
                    </DropdownMenuItem>
                    <DropdownMenuItem
                      class="cursor-pointer rounded-lg px-4 py-3 text-base font-medium hover:bg-primary/10 focus:bg-primary/10"
                      @click="scrollToSection('how-it-works')"
                    >
                      How It Works
                    </DropdownMenuItem>
                    <div class="border-t border-border/30 my-2" />
                    <DropdownMenuItem as-child>
                      <a
                        href="https://github.com/Pentulz/pentulz"
                        class="flex items-center space-x-3 cursor-pointer rounded-lg px-4 py-3 text-base font-medium hover:bg-muted/50 focus:bg-muted/50"
                        rel="noopener noreferrer"
                      >
                        <Github class="h-5 w-5" />
                        <span>GitHub</span>
                      </a>
                    </DropdownMenuItem>
                    <DropdownMenuItem as-child>
                      <a
                        href="https://github.com/Pentulz/Pentulz/blob/main/README.md"
                        class="flex items-center space-x-3 cursor-pointer rounded-lg px-4 py-3 text-base font-medium bg-primary/10 hover:bg-primary/20 focus:bg-primary/20 text-primary"
                      >
                        <ArrowRight class="h-5 w-5" />
                        <span>Get Started</span>
                      </a>
                    </DropdownMenuItem>
                  </div>
                </DropdownMenuContent>
              </DropdownMenu>
            </div>

            <!-- Desktop and scrolled state buttons -->
            <Button
              variant="ghost"
              :size="isScrolled ? 'default' : 'default'"
              class="hidden sm:flex items-center space-x-2"
              as-child
            >
              <a
                href="https://github.com/Pentulz/pentulz"
                aria-label="View Pentulz source code on GitHub"
                rel="noopener noreferrer"
              >
                <Github
                  :class="isScrolled ? 'h-4 w-4' : 'h-5 w-5'"
                  aria-hidden="true"
                />
                <span :class="isScrolled ? 'text-sm' : 'text-base'"
                  >GitHub</span
                >
              </a>
            </Button>
            <Button
              :size="isScrolled ? 'default' : 'default'"
              class="bg-primary hover:bg-primary/90 transition-all duration-300"
              :class="[
                {
                  'px-3 sm:px-4 py-2 text-sm': isScrolled,
                  'px-4 sm:px-5 py-2 text-sm sm:text-base': !isScrolled,
                },
                !isScrolled ? 'hidden sm:flex' : 'flex',
              ]"
              as-child
            >
              <a href="https://github.com/Pentulz/Pentulz/blob/main/README.md">
                Start
              </a>
            </Button>
          </div>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section id="hero" class="relative py-20 sm:py-32 pt-32">
      <div class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="text-center">
          <div class="flex items-center justify-center mb-8">
            <div
              ref="heroBadge"
              class="flex items-center space-x-2 bg-gradient-to-r from-primary/10 to-blue-500/10 backdrop-blur-sm px-6 py-3 rounded-full border border-primary/20 shadow-lg"
            >
              <Shield class="h-4 w-4 text-primary animate-pulse" />
              <span
                class="text-sm font-semibold bg-gradient-to-r from-primary to-blue-600 bg-clip-text text-transparent"
                >Security Orchestration Platform</span
              >
            </div>
          </div>

          <h1
            ref="heroTitle"
            class="text-4xl sm:text-6xl lg:text-7xl font-bold tracking-tight mb-8"
          >
            <span
              class="bg-gradient-to-r from-foreground via-foreground to-muted-foreground bg-clip-text text-transparent"
              >Orchestrate Security</span
            >
            <br />
            <span
              class="bg-gradient-to-r from-primary via-blue-500 to-purple-600 bg-clip-text text-transparent animate-pulse"
              >Testing at Scale</span
            >
          </h1>

          <p
            ref="heroDescription"
            class="text-xl sm:text-2xl text-muted-foreground mb-12 max-w-4xl mx-auto leading-relaxed"
          >
            Deploy agents across your infrastructure, execute pentest tools, and
            get unified results in a clean dashboard.
            <span class="font-semibold text-primary"
              >Simplify complexity. Accelerate security.</span
            >
          </p>

          <div
            ref="heroButtons"
            class="flex flex-col sm:flex-row gap-6 justify-center items-center mb-20"
          >
            <Button
              size="lg"
              class="bg-gradient-to-r from-primary to-blue-600 hover:from-primary/90 hover:to-blue-600/90 text-white px-10 py-4 text-lg font-semibold shadow-xl hover:shadow-2xl transition-all duration-300 hover:scale-105"
              as-child
            >
              <a
                href="https://github.com/Pentulz/Pentulz/blob/main/README.md"
                class="flex items-center gap-3"
              >
                Get Started Free
                <ArrowRight
                  class="h-5 w-5 group-hover:translate-x-1 transition-transform"
                />
              </a>
            </Button>

            <Button
              variant="outline"
              size="lg"
              class="px-10 py-4 text-lg border-2 hover:bg-muted/50 backdrop-blur-sm"
              as-child
            >
              <a
                href="https://github.com/Pentulz/pentulz"
                class="flex items-center gap-3"
              >
                <Github class="h-5 w-5" />
                View on GitHub
              </a>
            </Button>
          </div>

          <div ref="heroImage" class="relative px-4 sm:px-8">
            <div class="perspective-1000">
              <picture class="block transform-gpu">
                <source
                  srcset="/mockups/dashboard-desktop.png"
                  media="(min-width: 640px)"
                  type="image/png"
                />
                <img
                  src="/mockups/dashboard-mobile.png"
                  srcset="
                    /mockups/dashboard-mobile.png   600w,
                    /mockups/dashboard-desktop.png 1200w
                  "
                  sizes="(min-width: 1024px) 1000px, (min-width: 640px) 80vw, 100vw"
                  alt="Pentulz security orchestration dashboard showing distributed agents, running jobs, and unified vulnerability findings in a clean interface"
                  class="w-full max-w-5xl mx-auto shadow-2xl rounded-xl border border-border transform rotate-x-2 rotate-y-2 hover:rotate-x-0 hover:rotate-y-0 transition-transform duration-700 hover:scale-105"
                  loading="eager"
                  fetchpriority="high"
                  width="1200"
                  height="800"
                />
              </picture>
            </div>
            <div
              class="absolute left-4 right-4 top-4 bottom-4 sm:left-0 sm:right-0 sm:top-0 sm:bottom-0 sm:-inset-8 bg-gradient-to-r from-primary/20 via-blue-500/10 to-purple-500/20 rounded-2xl blur-2xl -z-10 animate-pulse"
            />
            <div
              class="absolute bottom-2 left-6 sm:-bottom-10 sm:-left-10 w-24 h-24 sm:w-32 sm:h-32 bg-primary/10 rounded-full blur-2xl -z-10"
            />
            <div
              class="absolute top-2 right-6 sm:-top-10 sm:-right-10 w-20 h-20 sm:w-24 sm:h-24 bg-blue-500/10 rounded-full blur-xl -z-10"
            />
          </div>
        </div>
      </div>
    </section>

    <!-- Stats Section -->
    <section class="py-12 bg-muted/30">
      <div class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-8 text-center">
          <div class="stat-item group p-4">
            <div
              class="text-2xl md:text-3xl font-bold text-primary mb-2 group-hover:scale-110 transition-transform duration-300"
            >
              3+
            </div>
            <div class="text-xs md:text-sm text-muted-foreground">
              Supported Tools
            </div>
          </div>
          <div class="stat-item group p-4">
            <div
              class="text-2xl md:text-3xl font-bold text-primary mb-2 group-hover:scale-110 transition-transform duration-300"
            >
              ∞
            </div>
            <div class="text-xs md:text-sm text-muted-foreground">
              Scalable Agents
            </div>
          </div>
          <div class="stat-item group p-4">
            <div
              class="text-2xl md:text-3xl font-bold text-primary mb-2 group-hover:scale-110 transition-transform duration-300"
            >
              100%
            </div>
            <div class="text-xs md:text-sm text-muted-foreground">
              Open Source
            </div>
          </div>
          <div class="stat-item group p-4">
            <div
              class="text-2xl md:text-3xl font-bold text-primary mb-2 group-hover:scale-110 transition-transform duration-300"
            >
              1
            </div>
            <div class="text-xs md:text-sm text-muted-foreground">
              Unified Dashboard
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Key Benefits -->
    <section
      id="benefits"
      class="py-20 bg-background"
      aria-labelledby="benefits-title"
    >
      <div class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <h2 id="benefits-title" class="text-3xl sm:text-4xl font-bold mb-4">
            Why Choose Pentulz?
          </h2>
          <p class="text-xl text-muted-foreground max-w-2xl mx-auto">
            Transform your security testing with distributed orchestration and
            unified reporting.
          </p>
        </div>

        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6 lg:gap-8">
          <Card
            class="benefit-card p-6 lg:p-8 hover:shadow-xl transition-all duration-300 hover:-translate-y-2 border-0 bg-gradient-to-br from-card to-card/50 backdrop-blur-sm"
          >
            <div class="flex items-center mb-4 lg:mb-6">
              <div
                class="p-2 lg:p-3 bg-gradient-to-br from-primary/20 to-primary/10 rounded-lg lg:rounded-xl mr-3 lg:mr-4 group-hover:scale-110 transition-transform duration-300"
              >
                <Clock class="h-5 w-5 lg:h-7 lg:w-7 text-primary" />
              </div>
              <h3 class="font-bold text-lg lg:text-xl">80% Faster Audits</h3>
            </div>
            <p
              class="text-muted-foreground text-sm lg:text-base leading-relaxed"
            >
              Run multiple security tools across distributed agents. What used
              to take days now takes hours.
            </p>
            <div
              class="mt-4 lg:mt-6 flex items-center text-xs lg:text-sm text-primary font-medium"
            >
              <ArrowRight class="h-3 w-3 lg:h-4 lg:w-4 mr-2" />
              Easy execution across infrastructure
            </div>
          </Card>

          <Card
            class="benefit-card p-6 lg:p-8 hover:shadow-xl transition-all duration-300 hover:-translate-y-2 border-0 bg-gradient-to-br from-card to-card/50 backdrop-blur-sm"
          >
            <div class="flex items-center mb-4 lg:mb-6">
              <div
                class="p-2 lg:p-3 bg-gradient-to-br from-green-500/20 to-green-500/10 rounded-lg lg:rounded-xl mr-3 lg:mr-4 group-hover:scale-110 transition-transform duration-300"
              >
                <Target class="h-5 w-5 lg:h-7 lg:w-7 text-green-600" />
              </div>
              <h3 class="font-bold text-lg lg:text-xl">Zero Human Error</h3>
            </div>
            <p
              class="text-muted-foreground text-sm lg:text-base leading-relaxed"
            >
              Standardized tool execution and automated result parsing eliminate
              inconsistencies and manual mistakes.
            </p>
            <div
              class="mt-4 lg:mt-6 flex items-center text-xs lg:text-sm text-green-600 font-medium"
            >
              <ArrowRight class="h-3 w-3 lg:h-4 lg:w-4 mr-2" />
              Consistent, reliable results every time
            </div>
          </Card>

          <Card
            class="benefit-card p-6 lg:p-8 hover:shadow-xl transition-all duration-300 hover:-translate-y-2 border-0 bg-gradient-to-br from-card to-card/50 backdrop-blur-sm sm:col-span-2 lg:col-span-1"
          >
            <div class="flex items-center mb-4 lg:mb-6">
              <div
                class="p-2 lg:p-3 bg-gradient-to-br from-blue-500/20 to-blue-500/10 rounded-lg lg:rounded-xl mr-3 lg:mr-4 group-hover:scale-110 transition-transform duration-300"
              >
                <Database class="h-5 w-5 lg:h-7 lg:w-7 text-blue-600" />
              </div>
              <h3 class="font-bold text-lg lg:text-xl">Unified Intelligence</h3>
            </div>
            <p
              class="text-muted-foreground text-sm lg:text-base leading-relaxed"
            >
              All results normalized to consistent JSON format. Advanced
              filtering, correlation, and export capabilities.
            </p>
            <div
              class="mt-4 lg:mt-6 flex items-center text-xs lg:text-sm text-blue-600 font-medium"
            >
              <ArrowRight class="h-3 w-3 lg:h-4 lg:w-4 mr-2" />
              Single source of truth for findings
            </div>
          </Card>
        </div>
      </div>
    </section>

    <!-- How It Works -->
    <section
      id="how-it-works"
      class="py-20 bg-muted/30"
      aria-labelledby="how-it-works-title"
    >
      <div class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div class="text-center mb-16">
          <h2
            id="how-it-works-title"
            class="text-3xl sm:text-4xl font-bold mb-4"
          >
            How It Works
          </h2>
          <p class="text-xl text-muted-foreground max-w-2xl mx-auto">
            Three simple steps to orchestrate your security testing at scale.
          </p>
        </div>

        <div class="space-y-20">
          <!-- Step 1: Deploy -->
          <div class="grid lg:grid-cols-2 gap-12 items-center">
            <div class="order-2 lg:order-1">
              <div class="flex items-center mb-6">
                <div
                  class="flex items-center justify-center w-10 h-10 bg-primary text-primary-foreground rounded-full font-bold mr-4"
                >
                  1
                </div>
                <h3 class="text-2xl font-semibold">Deploy Agents</h3>
              </div>
              <p class="text-lg text-muted-foreground mb-6">
                Set up Pentulz agents across your infrastructure with simple
                deployment commands. Each agent connects automatically and
                reports its status with periodic heartbeats.
              </p>
              <div class="space-y-3">
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Fast agent deployment
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Periodic health monitoring
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Secure encrypted communication
                </div>
              </div>
            </div>
            <div class="order-1 lg:order-2">
              <div class="relative group">
                <picture>
                  <source
                    srcset="/mockups/agent-desktop.png"
                    media="(min-width: 640px)"
                    type="image/png"
                  />
                  <img
                    src="/mockups/agent-mobile.png"
                    srcset="
                      /mockups/agent-mobile.png   600w,
                      /mockups/agent-desktop.png 1200w
                    "
                    sizes="(min-width: 1024px) 800px, (min-width: 640px) 80vw, 100vw"
                    alt="Pentulz agent deployment interface showing connected agents across distributed infrastructure with status monitoring"
                    class="w-full shadow-2xl rounded-xl border border-border transform transition-all duration-700 group-hover:rotate-y-3 group-hover:-rotate-x-2 group-hover:scale-105 perspective-1000"
                    loading="lazy"
                    width="800"
                    height="600"
                  />
                </picture>
                <div
                  class="absolute -inset-3 sm:-inset-6 bg-gradient-to-br from-primary/10 to-blue-500/5 rounded-2xl blur-xl -z-10 group-hover:blur-2xl transition-all duration-700"
                />
              </div>
            </div>
          </div>

          <!-- Step 2: Execute -->
          <div class="grid lg:grid-cols-2 gap-12 items-center">
            <div>
              <div class="relative group">
                <picture>
                  <source
                    srcset="/mockups/job-desktop.png"
                    media="(min-width: 640px)"
                    type="image/png"
                  />
                  <img
                    src="/mockups/job-mobile.png"
                    srcset="
                      /mockups/job-mobile.png   600w,
                      /mockups/job-desktop.png 1200w
                    "
                    sizes="(min-width: 1024px) 800px, (min-width: 640px) 80vw, 100vw"
                    alt="Pentulz job execution interface displaying multiple security tools running across distributed agents with progress tracking"
                    class="w-full shadow-2xl rounded-xl border border-border transform transition-all duration-700 group-hover:-rotate-y-3 group-hover:-rotate-x-2 group-hover:scale-105 perspective-1000"
                    loading="lazy"
                    width="800"
                    height="600"
                  />
                </picture>
                <div
                  class="absolute -inset-3 sm:-inset-6 bg-gradient-to-br from-green-500/10 to-primary/5 rounded-2xl blur-xl -z-10 group-hover:blur-2xl transition-all duration-700"
                />
              </div>
            </div>
            <div>
              <div class="flex items-center mb-6">
                <div
                  class="flex items-center justify-center w-10 h-10 bg-primary text-primary-foreground rounded-full font-bold mr-4"
                >
                  2
                </div>
                <h3 class="text-2xl font-semibold">Execute Tools</h3>
              </div>
              <p class="text-lg text-muted-foreground mb-6">
                Launch multiple pentest tools across all connected agents with a
                single command.
              </p>
              <div class="space-y-3">
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Easy execution job deployment across agents
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Progress tracking and logging
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Automatic retry and error handling
                </div>
              </div>
            </div>
          </div>

          <!-- Step 3: Analyze -->
          <div class="grid lg:grid-cols-2 gap-12 items-center">
            <div class="order-2 lg:order-1">
              <div class="flex items-center mb-6">
                <div
                  class="flex items-center justify-center w-10 h-10 bg-primary text-primary-foreground rounded-full font-bold mr-4"
                >
                  3
                </div>
                <h3 class="text-2xl font-semibold">Analyze Results</h3>
              </div>
              <p class="text-lg text-muted-foreground mb-6">
                All tool outputs are automatically parsed into a consistent JSON
                format. Filter findings, track progress, and export reports from
                a single dashboard.
              </p>
              <div class="space-y-3">
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Normalized JSON output format
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  Advanced filtering and search
                </div>
                <div class="flex items-center text-sm text-muted-foreground">
                  <CheckCircle class="h-4 w-4 text-green-500 mr-2" />
                  JSON export
                </div>
              </div>
            </div>
            <div class="order-1 lg:order-2">
              <div class="relative group">
                <picture>
                  <source
                    srcset="/mockups/report-desktop.png"
                    media="(min-width: 640px)"
                    type="image/png"
                  />
                  <img
                    src="/mockups/report-mobile.png"
                    srcset="
                      /mockups/report-mobile.png   600w,
                      /mockups/report-desktop.png 1200w
                    "
                    sizes="(min-width: 1024px) 800px, (min-width: 640px) 80vw, 100vw"
                    alt="Pentulz results analysis dashboard with normalized vulnerability findings, advanced filtering, and comprehensive export capabilities in unified JSON format"
                    class="w-full shadow-2xl rounded-xl border border-border transform transition-all duration-700 group-hover:rotate-y-3 group-hover:-rotate-x-2 group-hover:scale-105 perspective-1000"
                    loading="lazy"
                    width="800"
                    height="600"
                  />
                </picture>
                <div
                  class="absolute -inset-3 sm:-inset-6 bg-gradient-to-br from-blue-500/10 to-purple-500/5 rounded-2xl blur-xl -z-10 group-hover:blur-2xl transition-all duration-700"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA Section -->
    <section
      class="py-24 bg-gradient-to-br from-primary via-blue-600 to-purple-700 text-white relative overflow-hidden"
    >
      <div class="absolute inset-0 bg-grid-white/10 bg-grid-16" />
      <div class="absolute inset-0 overflow-hidden">
        <div
          class="absolute top-0 -left-2 sm:-left-4 w-48 h-48 sm:w-72 sm:h-72 bg-white/5 rounded-full blur-3xl"
        />
        <div
          class="absolute bottom-0 -right-2 sm:-right-4 w-64 h-64 sm:w-96 sm:h-96 bg-white/5 rounded-full blur-3xl"
        />
      </div>
      <div
        class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 text-center relative z-10"
      >
        <h2
          class="text-4xl sm:text-5xl font-bold mb-6 bg-gradient-to-r from-white to-blue-100 bg-clip-text text-transparent"
        >
          Ready to Scale Your Security Testing?
        </h2>
        <p
          class="text-xl sm:text-2xl mb-12 opacity-90 max-w-3xl mx-auto leading-relaxed"
        >
          Join security professionals who trust Pentulz to orchestrate their
          pentest workflows. Start in minutes, scale infinitely.
        </p>
        <div class="flex flex-col sm:flex-row gap-6 justify-center">
          <Button
            size="lg"
            variant="secondary"
            class="px-10 py-4 text-lg font-semibold bg-white text-primary hover:bg-gray-100 shadow-2xl hover:shadow-3xl transition-all duration-300 hover:scale-105"
            as-child
          >
            <a
              href="https://github.com/Pentulz/Pentulz/blob/main/README.md"
              class="flex items-center gap-3"
            >
              Get Started Free
              <ArrowRight
                class="h-5 w-5 group-hover:translate-x-1 transition-transform"
              />
            </a>
          </Button>
          <Button
            size="lg"
            variant="outline"
            class="px-10 py-4 text-lg border-2 border-white/30 bg-white/10 backdrop-blur-sm text-white hover:bg-white/20 hover:border-white/50"
            as-child
          >
            <a
              href="https://github.com/Pentulz/pentulz"
              class="flex items-center gap-3"
            >
              <Github class="h-5 w-5" />
              Star on GitHub
            </a>
          </Button>
        </div>

        <div
          class="mt-12 lg:mt-16 flex flex-col sm:flex-row items-center justify-center gap-4 sm:gap-8 opacity-70"
        >
          <div class="flex items-center space-x-2 text-sm">
            <CheckCircle class="h-4 w-4" />
            <span>100% Open Source</span>
          </div>
          <div class="flex items-center space-x-2 text-sm">
            <CheckCircle class="h-4 w-4" />
            <span>No Vendor Lock-in</span>
          </div>
          <div class="flex items-center space-x-2 text-sm">
            <CheckCircle class="h-4 w-4" />
            <span>Community Driven</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="border-t border-border bg-card/50 backdrop-blur-sm">
      <div class="container mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-8">
        <div class="grid md:grid-cols-3 gap-8">
          <div>
            <div class="flex items-center space-x-3 mb-4">
              <img src="/logo.png" class="h-6 w-6 object-contain" />
              <span class="font-bold text-foreground">Pentulz</span>
            </div>
            <p class="text-sm text-muted-foreground">
              Orchestrate security testing at scale with distributed agents and
              unified reporting.
            </p>
          </div>

          <div>
            <h4 class="font-semibold mb-4">Resources</h4>
            <div class="space-y-2 text-sm">
              <a
                href="https://github.com/Pentulz/Pentulz/blob/main/README.md"
                class="text-muted-foreground hover:text-foreground transition-colors block"
              >
                Documentation
              </a>
              <a
                href="https://github.com/Pentulz/pentulz"
                class="text-muted-foreground hover:text-foreground transition-colors block"
              >
                GitHub Repository
              </a>
            </div>
          </div>

          <div>
            <h4 class="font-semibold mb-4">Built by</h4>
            <div
              class="flex flex-wrap gap-x-4 gap-y-1 text-sm text-muted-foreground"
            >
              <a
                href="https://github.com/Mondotosz"
                class="hover:text-foreground transition-colors"
                >Mondotosz</a
              >
              <a
                href="https://github.com/NATSIIRT"
                class="hover:text-foreground transition-colors"
                >NATSIIRT</a
              >
              <a
                href="https://github.com/Thynkon"
                class="hover:text-foreground transition-colors"
                >Thynkon</a
              >
              <a
                href="https://github.com/Vicolet"
                class="hover:text-foreground transition-colors"
                >Vicolet</a
              >
            </div>
          </div>
        </div>

        <div
          class="border-t border-border mt-8 pt-6 text-center text-sm text-muted-foreground"
        >
          <p>
            &copy; 2025 Pentulz. Open source security orchestration platform.
          </p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>
.hero-badge,
.hero-title,
.hero-description,
.hero-buttons,
.hero-image,
.stat-item,
.benefit-card {
  opacity: 0;
  transform: translateY(20px);
}
.hero-image {
  transform: translateY(40px);
}
.benefit-card {
  transform: translateY(30px);
}

@media (prefers-reduced-motion: reduce) {
  .hero-badge,
  .hero-title,
  .hero-description,
  .hero-buttons,
  .hero-image,
  .stat-item,
  .benefit-card {
    opacity: 1 !important;
    transform: none !important;
  }
}
</style>
