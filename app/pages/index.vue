<script setup>
import { Github } from "lucide-vue-next";
</script>
<template>
  <div class="min-h-screen bg-background flex flex-col items-center gap-16">
    <!-- Headers -->
    <header
      class="border-b border-border bg-card/50 backdrop-blur-sm sticky top-0 z-50 w-full flex justify-center"
    >
      <div
        class="container max-w-[1320px] px-4 py-4 flex items-center justify-between"
      >
        <div class="flex items-center space-x-2">
          <!-- <Shield class="h-8 w-8 text-primary" /> -->
          <img src="/logo.png" class="h-8 w-8 object-contain" />
          <span class="text-xl font-bold text-foreground">Pentulz</span>
        </div>
        <nav class="hidden md:flex items-center space-x-6">
          <a
            href="#deploy"
            class="text-muted-foreground hover:text-foreground transition-colors"
          >
            Deploy
          </a>
          <a
            href="#execute"
            class="text-muted-foreground hover:text-foreground transition-colors"
          >
            Execute
          </a>
          <a
            href="#analyze"
            class="text-muted-foreground hover:text-foreground transition-colors"
          >
            Analyze
          </a>
        </nav>
        <div class="flex items-center space-x-3">
          <Button
            variant="outline"
            size="sm"
            class="hidden sm:flex items-center space-x-2 bg-transparent"
            as-child
          >
            <a href="https://github.com/Pentulz/pentulz">
              <Github class="h-4 w-4" />
              <span>GitHub</span>
            </a>
          </Button>
          <Button size="sm" class="bg-primary hover:bg-primary/90" as-child>
            <a href="https://github.com/Pentulz/Pentulz/blob/main/README.md">
              Get Started
            </a>
          </Button>
        </div>
      </div>
    </header>

    <div class="flex flex-col max-w-[1096px] px-8 w-full pt-8">
      <section class="flex flex-col gap-6">
        <h1 class="text-7xl font-bold">
          Pen<span class="text-blue-500">tulz</span>
        </h1>
        <h2 class="text-4xl md:max-w-4/5">
          Orchestrate pentest tools across distributed agents. Unified results,
          clean UI.
        </h2>

        <div class="flex flex-row gap-6">
          <Button
            variant="default"
            size="lg"
            class="flex items-center gap-2"
            as-child
          >
            <a href="https://github.com/Pentulz/Pentulz/blob/main/README.md">
              Get Started
            </a>
          </Button>

          <Button
            variant="outline"
            size="lg"
            class="flex items-center gap-2"
            as-child
          >
            <a href="https://github.com/Pentulz/pentulz">
              <Github class="h-4 w-4" />
              <span>View on GitHub</span>
            </a>
          </Button>
        </div>

        <picture class="flex w-full flex-col items-center">
          <source
            srcset="/mockups/dashboard-desktop.png"
            media="(min-width: 640px)"
          />
          <img
            src="/mockups/dashboard-mobile.png"
            alt="dashboard screenshot"
            class="shadow-md rounded-lg border-1 border-neutral-200"
          />
        </picture>

        <p class="w-full text-center text-4xl">
          Pentest complexity, simplified.
        </p>
      </section>
    </div>

    <div class="flex flex-col max-w-[1320px] px-8 w-full gap-24 pt-16">
      <section class="flex flex-col lg:flex-row items-center gap-6 w-full">
        <div class="lg:w-2/5 flex flex-col gap-4">
          <h2 id="deploy" class="text-4xl">Deploy</h2>
          <p class="text-3xl">
            Set up Pentulz across your infrastructure with simple deployment
            commands. Each agent connects automatically and reports its status
            in real-time.
          </p>
        </div>

        <picture class="lg:w-3/5">
          <source
            srcset="/mockups/agent-desktop.png"
            media="(min-width: 640px)"
          />
          <img
            src="/mockups/agent-mobile.png"
            alt="agent screenshot"
            class="shadow-md rounded-lg border-1 border-neutral-200"
          />
        </picture>
      </section>

      <section
        class="flex flex-col lg:flex-row-reverse items-center gap-6 w-full"
      >
        <div class="lg:w-2/5 flex flex-col gap-4">
          <h2 id="execute" class="text-4xl">Execute</h2>
          <p class="text-3xl">
            Launch multiple pentest tools simultaneously across all connected
            agents.
          </p>
        </div>

        <picture class="lg:w-3/5">
          <source
            srcset="/mockups/job-desktop.png"
            media="(min-width: 640px)"
          />
          <img
            src="/mockups/job-mobile.png"
            alt="job screenshot"
            class="shadow-md rounded-lg border-1 border-neutral-200"
          />
        </picture>
      </section>

      <section class="flex flex-col lg:flex-row items-center gap-6 w-full">
        <div class="lg:w-2/5 flex flex-col gap-4">
          <h2 id="analyze" class="text-4xl">Analyze</h2>
          <p class="text-3xl">
            All tool outputs are automatically parsed into a consistent JSON
            format. Filter findings, track progress, and export reports from a
            single dashboard.
          </p>
        </div>

        <picture class="lg:w-3/5">
          <source
            srcset="/mockups/report-desktop.png"
            media="(min-width: 640px)"
          />
          <img
            src="/mockups/report-mobile.png"
            alt="report screenshot"
            class="shadow-md rounded-lg border-1 border-neutral-200"
          />
        </picture>
      </section>
    </div>

    <footer
      class="border-t border-border bg-card/50 backdrop-blur-sm w-full flex justify-center"
    >
      <div
        class="container max-w-[1320px] px-4 py-4 flex items-center justify-center"
      >
        <p>
          © 2024 Pentulz. Built by
          <a href="https://github.com/NATSIIRT" class="hover:underline"
            >NATSIIRT</a
          >,
          <a href="https://github.com/Thynkon" class="hover:underline"
            >Thynkon</a
          >,<a href="https://github.com/Vicolet" class="hover:underline"
            >Vicolet</a
          >
          &
          <a href="https://github.com/Mondotosz" class="hover:underline"
            >Mondotosz</a
          >
        </p>
      </div>
    </footer>
  </div>
</template>
