<script>
  import '../app.css'
  import '../prism.css'
  import { browser } from '$app/environment'
  import { name, logo } from '$lib/info'
  import { page } from '$app/stores'

  let nightMode = `<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 stroke-current fill-none stroke-2" viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round">
                      <path d="M12 3c.132 0 .263 0 .393 0a7.5 7.5 0 0 0 7.92 12.446a9 9 0 1 1 -8.313 -12.454z"></path>
                   </svg>`

  let dayMode = `<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 stroke-current fill-none stroke-2" viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M12 12m-4 0a4 4 0 1 0 8 0a4 4 0 1 0 -8 0"></path>
                    <path d="M3 12h1m8 -9v1m8 8h1m-9 8v1m-6.4 -15.4l.7 .7m12.1 -.7l-.7 .7m0 11.4l.7 .7m-12.1 -.7l-.7 .7"></path>
                 </svg>`

  let isDarkMode = browser ? Boolean(document.documentElement.classList.contains('dark')) : true

  function disableTransitionsTemporarily() {
    document.documentElement.classList.add('[&_*]:!transition-none')
    window.setTimeout(() => {
      document.documentElement.classList.remove('[&_*]:!transition-none')
    }, 0)
  }
</script>

<div class="flex flex-col min-h-screen ">
  <div class="flex flex-col flex-grow w-full px-4 py-2">
    <header class="flex items-center justify-between w-full max-w-3xl py-4 mx-auto lg:pb-8">
      <a
        class="text-lg font-bold sm:text-2xl !text-transparent bg-clip-text bg-gradient-to-r from-green-500 to-green-600 dark:to-green-400"
        href="/"
      >
        <!-- {name} -->
        {@html logo}
      </a>

      <button
        type="button"
        role="switch"
        aria-label="Toggle Dark Mode"
        aria-checked={isDarkMode}
        class="w-5 h-5 sm:h-8 sm:w-8 sm:p-1"
        on:click={() => {
          isDarkMode = !isDarkMode
          localStorage.setItem('isDarkMode', isDarkMode.toString())

          disableTransitionsTemporarily()

          if (isDarkMode) {
            document.querySelector('html').classList.add('dark')
          } else {
            document.querySelector('html').classList.remove('dark')
          }
        }}
      >
        <!-- <MoonIcon class="hidden text-zinc-500 dark:block" />
        <SunIcon class="block text-zinc-400 dark:hidden" /> -->
        <span class="hidden dark:block">{@html nightMode}</span>
        <span class="block dark:hidden">{@html dayMode}</span>

      </button>
    </header>
    <main
      class="flex flex-col flex-grow w-full mx-auto"
      class:max-w-3xl={!$page.data.layout?.fullWidth}
    >
      <slot />
    </main>
  </div>
</div>
