<script setup>
import { ref, onMounted } from 'vue'
import AppSidebar from './AppSidebar.vue'
import SidebarNavigation from '../../components/sidebar/SidebarNavigation.vue'
import SidebarProfile from '../../components/sidebar/SidebarProfile.vue'
import SidebarToggle from '../../components/sidebar/SidebarToggle.vue'
import AppHeader from '../../components/header/AppHeader.vue'
import IconSidebarLogo from '../../components/icons/sidebar/IconSidebarLogo.vue'

// Start closed by default (will be opened on desktop via onMounted)
const sidebarOpen = ref(false)
const emit = defineEmits(['create'])

// On mount, check if we're on desktop (md breakpoint = 768px) and open sidebar if so
onMounted(() => {
  const isDesktop = window.innerWidth >= 768
  if (isDesktop) {
    sidebarOpen.value = true
  }
})

function toggleSidebar() {
  sidebarOpen.value = !sidebarOpen.value
}

function closeSidebar() {
  sidebarOpen.value = false
}
</script>

<template>
  <div class="relative flex h-screen overflow-hidden bg-slate-100 text-slate-900">
    <AppSidebar :open="sidebarOpen">
      <template #logo>
        <IconSidebarLogo />
      </template>

      <template #default>
        <SidebarNavigation />
      </template>

      <template #profile>
        <SidebarProfile />
      </template>
    </AppSidebar>

    <SidebarToggle
      :open="sidebarOpen"
      @toggle="toggleSidebar"
    />

    <div
      v-if="sidebarOpen"
      class="fixed inset-0 z-30 bg-slate-900/40 md:hidden"
      @click="closeSidebar"
    />

    <div class="flex flex-1 min-w-0 flex-col">
      <AppHeader @create="emit('create')" />
      <main class="mx-auto w-full max-w-6xl flex-1 overflow-y-auto px-4 py-6">
        <slot />
      </main>
    </div>
  </div>
</template>
