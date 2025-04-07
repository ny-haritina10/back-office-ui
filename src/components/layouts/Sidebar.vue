<template>
  <aside
    :class="[
      'bg-white border-r border-gray-200 transition-all duration-300 ease-in-out overflow-y-auto overflow-x-hidden',
      collapsed ? 'w-20' : 'w-64'
    ]"
  >
    <div class="flex items-center justify-between h-16 px-4 border-b border-gray-200 flex-shrink-0">
      <div class="flex items-center">
         <component :is="Layers" class="h-8 w-8 text-primary" />
         <span
          :class="[
            'ml-2 text-lg font-semibold text-gray-800 transition-opacity duration-200',
            collapsed ? 'opacity-0 hidden' : 'opacity-100'
          ]"
         >
          ERP System
         </span>
      </div>
       <button
         @click="emitToggle"
         class="p-1 rounded-md hover:bg-gray-100 text-gray-500"
       >
         <chevron-left v-if="!collapsed" class="h-5 w-5" />
         <chevron-right v-else class="h-5 w-5" />
       </button>
    </div>

    <nav class="mt-4 px-2">
       <div v-for="(section, index) in navigationSections" :key="index" class="mb-6">
         <div
          :class="[
            'text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2 px-3',
            collapsed ? 'text-center' : ''
          ]"
          v-if="!collapsed"
         >
          {{ section.title }}
         </div>
         <div v-else class="border-b border-gray-200 my-4"></div>

         <div class="space-y-1">
          <button
            v-for="item in section.items"
            :key="item.name"
            :class="[
              'flex items-center w-full px-3 py-2 text-sm rounded-md transition-colors',
              item.active
                ? 'bg-primary text-white'
                : 'text-gray-700 hover:bg-gray-100',
              collapsed ? 'justify-center' : 'justify-start'
            ]"
          >
            <component :is="item.icon" class="h-5 w-5" />
            <span
              :class="[
                'transition-opacity duration-200',
                collapsed ? 'hidden' : 'ml-3'
              ]"
            >
              {{ item.name }}
            </span>
          </button>
         </div>
       </div>
    </nav>
  </aside>
</template>

<script setup>
import {
BarChart2,
Calendar,
ChevronLeft,
ChevronRight,
CreditCard,
DollarSign,
FileText,
HelpCircle,
Layers,
LogOut,
Package,
Settings as SettingsIcon,
ShoppingCart,
Truck,
User,
Users
} from 'lucide-vue-next';

// Define props received from App.vue
const props = defineProps({
  collapsed: {
    type: Boolean,
    required: true
  }
});

// Define emits to send events up to App.vue
const emit = defineEmits(['toggle-sidebar']);

const emitToggle = () => {
  emit('toggle-sidebar');
};

// Navigation sections (specific to Sidebar)
const navigationSections = [
 {
    title: 'Main',
    items: [
      { name: 'Dashboard', icon: BarChart2, active: true },
      { name: 'Orders', icon: ShoppingCart, active: false },
      { name: 'Customers', icon: Users, active: false },
      { name: 'Products', icon: Package, active: false },
      { name: 'Inventory', icon: Layers, active: false },
    ]
  },
  {
    title: 'Finance',
    items: [
      { name: 'Invoices', icon: FileText, active: false },
      { name: 'Payments', icon: CreditCard, active: false },
      { name: 'Expenses', icon: DollarSign, active: false },
    ]
  },
  {
    title: 'Management',
    items: [
      { name: 'Employees', icon: User, active: false },
      { name: 'Calendar', icon: Calendar, active: false },
      { name: 'Reports', icon: BarChart2, active: false },
      { name: 'Shipping', icon: Truck, active: false }, 
    ]
  },
  {
    title: 'Settings',
    items: [
      { name: 'Settings', icon: SettingsIcon, active: false },
      { name: 'Help', icon: HelpCircle, active: false },
      { name: 'Logout', icon: LogOut, active: false },
    ]
  }
];
</script>