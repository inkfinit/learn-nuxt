<template>
  <div class="p-6">
    <UContainer>
      <div class="flex flex-col space-y-6 pt-4">
        
        <!-- Welcome & Status -->
        <div class="bg-white dark:bg-gray-800 p-5 rounded-2xl shadow-sm border border-gray-100 dark:border-gray-700 flex items-center justify-between">
          <div class="flex items-center space-x-4">
            <UAvatar src="https://i.pravatar.cc/150?u=dummy" alt="User" size="lg" />
            <div>
              <h2 class="text-lg font-bold text-gray-900 dark:text-white">Hi, John Doe 👋</h2>
              <p class="text-sm text-gray-500 dark:text-gray-400">Welcome back!</p>
            </div>
          </div>
          
          <div class="flex items-center space-x-2">
            <span class="relative flex h-3 w-3">
              <span v-if="isOnline" class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
              <span class="relative inline-flex rounded-full h-3 w-3" :class="isOnline ? 'bg-green-500' : 'bg-red-500'"></span>
            </span>
            <span class="text-sm font-medium" :class="isOnline ? 'text-green-600 dark:text-green-400' : 'text-red-600 dark:text-red-400'">
              {{ isOnline ? 'Online' : 'Offline' }}
            </span>
          </div>
        </div>

        <!-- Add Coordinate Feature -->
        <div class="bg-white dark:bg-gray-800 p-5 rounded-2xl shadow-sm border border-gray-100 dark:border-gray-700">
          <div class="flex items-center space-x-3 mb-4">
            <div class="p-2 bg-primary-50 dark:bg-primary-900/20 text-primary-500 rounded-lg">
              <UIcon name="i-heroicons-map-pin" class="w-6 h-6" />
            </div>
            <h3 class="text-md font-semibold text-gray-900 dark:text-white">Location Services</h3>
          </div>
          
          <UButton 
            color="primary" 
            variant="solid" 
            block 
            size="lg"
            :loading="isLoadingLocation"
            @click="getCoordinates"
          >
            Get My Coordinate
          </UButton>

          <div v-if="coordinates" class="mt-4 p-4 bg-gray-50 dark:bg-gray-900 rounded-xl border border-gray-200 dark:border-gray-700">
            <div class="grid grid-cols-2 gap-4">
              <div>
                <p class="text-xs text-gray-500 dark:text-gray-400 mb-1">Latitude</p>
                <p class="text-sm font-medium text-gray-900 dark:text-white">{{ coordinates.lat.toFixed(6) }}</p>
              </div>
              <div>
                <p class="text-xs text-gray-500 dark:text-gray-400 mb-1">Longitude</p>
                <p class="text-sm font-medium text-gray-900 dark:text-white">{{ coordinates.lng.toFixed(6) }}</p>
              </div>
            </div>
          </div>
          
          <div v-if="locationError" class="mt-4 p-3 bg-red-50 dark:bg-red-900/20 text-red-600 dark:text-red-400 rounded-xl border border-red-200 dark:border-red-800/30 text-sm">
            {{ locationError }}
          </div>
        </div>

      </div>
    </UContainer>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isOnline = ref(true)

const updateOnlineStatus = () => {
  isOnline.value = navigator.onLine
}

onMounted(() => {
  isOnline.value = navigator.onLine
  window.addEventListener('online', updateOnlineStatus)
  window.addEventListener('offline', updateOnlineStatus)
})

onUnmounted(() => {
  window.removeEventListener('online', updateOnlineStatus)
  window.removeEventListener('offline', updateOnlineStatus)
})

const coordinates = ref(null)
const locationError = ref(null)
const isLoadingLocation = ref(false)

const getCoordinates = () => {
  if (!navigator.geolocation) {
    locationError.value = 'Geolocation is not supported by your browser'
    return
  }
  
  isLoadingLocation.value = true
  locationError.value = null
  
  navigator.geolocation.getCurrentPosition(
    (position) => {
      coordinates.value = {
        lat: position.coords.latitude,
        lng: position.coords.longitude
      }
      isLoadingLocation.value = false
    },
    (error) => {
      // Handle error cases
      switch(error.code) {
        case error.PERMISSION_DENIED:
          locationError.value = "User denied the request for Geolocation."
          break
        case error.POSITION_UNAVAILABLE:
          locationError.value = "Location information is unavailable."
          break
        case error.TIMEOUT:
          locationError.value = "The request to get user location timed out."
          break
        default:
          locationError.value = "An unknown error occurred."
          break
      }
      isLoadingLocation.value = false
    },
    {
      enableHighAccuracy: true,
      timeout: 10000,
      maximumAge: 0
    }
  )
}
</script>
