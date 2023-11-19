<script lang="ts">
  import '../app.postcss';

  import { ModeWatcher } from 'mode-watcher';
  import { Button } from '$lib/components/ui/button';
  import { Sun, Moon, Github } from 'lucide-svelte';
  import { toggleMode } from 'mode-watcher';
</script>

<ModeWatcher />
<header
  class="flex h-14 flex-col justify-center bg-[#ebebeb] dark:bg-[#171717]"
>
  <div class="container flex items-center justify-between">
    <div class="flex">
      <svg
        class="ml-2 mr-3 h-6 w-6 text-gray-800 dark:text-white sm:h-8"
        aria-hidden="true"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 18 20"
      >
        <path
          stroke="currentColor"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M5 5h8m-1-3a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1m6 0v3H6V2m6 0h4a1 1 0 0 1 1 1v15a1 1 0 0 1-1 1H2a1 1 0 0 1-1-1V3a1 1 0 0 1 1-1h4m0 9.464 2.025 1.965L12 9.571"
        />
      </svg>
      <span class="self-center whitespace-nowrap text-xl font-semibold"
        >ToDo</span
      >
    </div>

    <div>
      <Button
        variant="link"
        size="icon"
        href="https://github.com/rochajulian/ToDo"
        target="_blank"
      >
        <Github class="h-[1.2rem] w-[1.2rem]" />
      </Button>
      <Button on:click={toggleMode} variant="ghost" size="icon">
        <Sun
          class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
        />
        <Moon
          class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
        />
        <span class="sr-only">Toggle theme</span>
      </Button>
    </div>
  </div>
</header>

<slot />
