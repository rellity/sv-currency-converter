<script lang="ts">
  import Converter from './lib/Converter.svelte';
  import DarkModeButton from './lib/DarkModeButton.svelte';
  import { SunMoon } from "lucide-svelte";
</script>

<main>
  <div class="flex items-center justify-center mt-5">
    <Converter />
  </div>
    <p class="text-center text-light text-gray-500 text-sm mt-5">
      powered by: <a href="https://hexarate.paikama.co">hexarate</a>
    </p>
  
    <p class="text-center text-light text-gray-500 text-sm mt-5">
      site by <a href="https://rellity.pages.dev">Zachari Iligan</a>
    </p>


  <DarkModeButton classNames="fixed bottom-5 right-5 rounded-full bg-blue-500 text-white">
    <SunMoon />
  </DarkModeButton>
</main>

<style>
	:global(body) {
		@apply bg-white text-gray-800 shadow-black;
		transition: background-color 0.3s
	}
	:global(body.dark-mode) {
    @apply bg-gray-800 text-black;
	}
</style>