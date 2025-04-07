<template>
  <header class="bg-white border-b border-gray-200 h-16 flex items-center justify-between px-6 flex-shrink-0">
     <div class="flex items-center">
       <button class="p-1 rounded-md hover:bg-gray-100 text-gray-500 md:hidden">
         <Menu class="h-5 w-5" />
       </button>
       <div class="ml-4 relative">
         <div class="flex items-center border border-gray-300 rounded-md px-3 py-1.5 bg-gray-50 focus-within:ring-2 focus-within:ring-primary focus-within:border-primary">
           <Search class="h-4 w-4 text-gray-500" />
           <input
            type="text"
            placeholder="Search..."
            class="ml-2 bg-transparent border-none focus:outline-none text-sm text-gray-700 w-48 md:w-64"
           />
         </div>
       </div>
     </div>

     <div class="flex items-center space-x-4">
       <button class="p-1.5 rounded-md hover:bg-gray-100 text-gray-500 relative">
         <Bell class="h-5 w-5" />
         <span class="absolute top-0 right-0 h-2 w-2 rounded-full bg-red-500"></span>
       </button>

       <button class="p-1.5 rounded-md hover:bg-gray-100 text-gray-500">
         <Settings class="h-5 w-5" />
       </button>

       <div class="relative">
         <button class="flex items-center space-x-2">
             <div class="h-8 w-8 rounded-full bg-gray-200 flex items-center justify-center text-gray-500">
                <component :is="User" class="h-5 w-5" />
             </div>
           <div class="hidden md:block text-left">
             <div class="text-sm font-medium text-gray-800">John Doe</div>
             <div class="text-xs text-gray-500">Administrator</div>
           </div>
           <ChevronDown class="h-4 w-4 text-gray-500 hidden md:block" />
         </button>
         </div>
     </div>
  </header>
</template>

<script setup>
import {
Bell,
ChevronDown,
Menu,
Search,
Settings,
User
} from 'lucide-vue-next';

</script>