<template>
  <div>
    <div class="flex flex-col justify-center my-6">
      <img src="https://sheinsight.github.io/react-use/logo.svg" class="h-16 mb-4" />
      <div class='text-center'><code>@shined/react-use</code></div>
    </div>
    <div
      class='size-full text-2xl flex flex-col justify-center items-center hover:text-black/60 dark:hover:text-white/60'>
      <div class='flex'>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Animation')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-sparkles-outline class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Animation
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Browser')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-google-chrome class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Browser
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Element')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-hexagon-slice-6 class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Element
        </div>
      </div>
      <div class='flex'>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Lifecycle')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-react class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Lifecycle
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Network')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-internet class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Network
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=ProUtilities')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-toolbox-outline class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />ProUtilities
        </div>
      </div>
      <div class='flex'>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Sensors')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-access-point class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Sensors
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=State')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-atom class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />State
        </div>
        <div onclick="window.open('https://sheinsight.github.io/react-use/?category=Utilities')"
          class='cursor-pointer w-48 py-4 group gap-1 flex flex-col items-center transition-all hover:bg-zinc/12 rounded hover:text-black dark:hover:text-white'>
          <mdi-tools class='mx-2 text-[#2e8555]/80 transition-all group-hover:text-amber/80' />Utilities
        </div>
      </div>
    </div>
  </div>
</template>
