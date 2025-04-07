<template>
  <footer class="bg-white border-t border-gray-200 py-4 px-6 flex-shrink-0">
     <div class="flex flex-col md:flex-row justify-between items-center">
       <div class="text-sm text-gray-500">
         &copy; {{ currentYear }} ERP System. All rights reserved.
       </div>
       <div class="flex space-x-4 mt-2 md:mt-0">
         <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Privacy Policy</a>
         <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Terms of Service</a>
         <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Contact Support</a>
       </div>
     </div>
  </footer>
</template>

<script>
import { computed } from 'vue';

// Make year dynamic
const currentYear = computed(() => new Date().getFullYear());
</script>