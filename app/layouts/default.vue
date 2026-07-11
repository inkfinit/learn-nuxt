<template>
  <div class="h-screen flex flex-col bg-gray-50 dark:bg-gray-900">
    <main class="flex-1 overflow-y-auto pb-16">
      <slot />
    </main>

    <!-- Bottom Navigation Bar -->
    <div class="fixed bottom-0 w-full border-t border-gray-200 dark:border-gray-800 bg-white dark:bg-gray-900 z-50">
      <div class="flex justify-around items-center h-16">
        <NuxtLink 
          to="/" 
          class="flex flex-col items-center justify-center w-full h-full text-gray-500 hover:text-primary-500 dark:text-gray-400 dark:hover:text-primary-400"
          active-class="text-primary-500 dark:text-primary-400"
        >
          <UIcon name="i-heroicons-home" class="w-6 h-6 mb-1" />
          <span class="text-xs font-medium">Home</span>
        </NuxtLink>

        <NuxtLink 
          to="/list" 
          class="flex flex-col items-center justify-center w-full h-full text-gray-500 hover:text-primary-500 dark:text-gray-400 dark:hover:text-primary-400"
          active-class="text-primary-500 dark:text-primary-400"
        >
          <UIcon name="i-heroicons-list-bullet" class="w-6 h-6 mb-1" />
          <span class="text-xs font-medium">List</span>
        </NuxtLink>

        <NuxtLink 
          to="/document" 
          class="flex flex-col items-center justify-center w-full h-full text-gray-500 hover:text-primary-500 dark:text-gray-400 dark:hover:text-primary-400"
          active-class="text-primary-500 dark:text-primary-400"
        >
          <UIcon name="i-heroicons-document-text" class="w-6 h-6 mb-1" />
          <span class="text-xs font-medium">Document</span>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>
