<template>
  <div class="flex h-screen bg-gray-50">
    <Sidebar :collapsed="sidebarCollapsed" @toggle-sidebar="toggleSidebar" />

    <div class="flex flex-col flex-1 overflow-hidden">
      <Header />

      <main class="flex-1 overflow-y-auto overflow-x-hidden p-6 bg-gray-50">
        <Dashboard />
      </main>

      <Footer />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Footer from './components/layouts/Footer.vue';
import Header from './components/layouts/Header.vue';
import Sidebar from './components/layouts/Sidebar.vue';
import Dashboard from './components/pages/Dashboard.vue';

// Shared state lifted up to the parent
const sidebarCollapsed = ref(false);

const toggleSidebar = () => {
  sidebarCollapsed.value = !sidebarCollapsed.value;
};
</script>

<style>
/* Keep global styles or move them to a dedicated CSS file */
:root {
  --color-primary: 25 95 125;
  --color-primary-dark: 20 75 100;
}

.bg-primary {
  background-color: rgb(var(--color-primary));
}

.text-primary {
  color: rgb(var(--color-primary));
}

.text-primary-dark {
  color: rgb(var(--color-primary-dark));
}

.focus\:ring-primary:focus {
  --tw-ring-color: rgb(var(--color-primary));
}

.focus-visible\:ring-primary:focus-visible {
  --tw-ring-color: rgb(var(--color-primary));
}

.focus\:border-primary:focus {
  border-color: rgb(var(--color-primary));
}

.focus-visible\:border-primary:focus-visible {
  border-color: rgb(var(--color-primary));
}
</style>