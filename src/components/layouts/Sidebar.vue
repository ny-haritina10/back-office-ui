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
       <div v-for="(section, index) in navigationSections" :key="`section-${index}`" class="mb-6">
         <div
          :class="[
            'text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2 px-3',
            collapsed ? 'text-center' : ''
          ]"
          v-if="!collapsed && section.title"
         >
          {{ section.title }}
         </div>
         <div v-else-if="collapsed && section.title" class="border-b border-gray-200 my-4"></div>

         <div class="space-y-1">
            <div v-for="item in section.items" :key="item.name">
              <button
                @click="item.isDropdown ? toggleDropdown(item.name) : null"
                :class="[
                  'flex items-center w-full px-3 py-2 text-sm rounded-md transition-colors',
                   // Apply active styles only if it's NOT a dropdown parent OR if it is and IS active (optional)
                  !item.isDropdown && item.active
                    ? 'bg-primary text-white'
                    : 'text-gray-700 hover:bg-gray-100',
                  collapsed ? 'justify-center' : 'justify-start',
                  item.isDropdown ? 'font-medium' : '' // Maybe slightly bolder parent
                ]"
                :disabled="item.isDropdown && collapsed"
              >
                <component :is="item.icon" class="h-5 w-5 flex-shrink-0" />
                <span
                  :class="[
                    'flex-1 text-left transition-opacity duration-200', // Use flex-1 for text alignment
                    collapsed ? 'hidden' : 'ml-3'
                  ]"
                >
                  {{ item.name }}
                </span>
                <ChevronDown
                   v-if="item.isDropdown && !collapsed"
                   :class="[
                     'h-4 w-4 text-gray-400 transition-transform duration-200 flex-shrink-0',
                     openDropdowns[item.name] ? 'rotate-180' : 'rotate-0'
                   ]"
                />
              </button>

              <div
                 v-if="item.isDropdown && openDropdowns[item.name] && !collapsed"
                 class="mt-1 ml-4 pl-3 border-l border-gray-200 space-y-1"
              >
                 <button
                   v-for="child in item.children"
                   :key="child.name"
                   :class="[
                     'flex items-center w-full px-3 py-1.5 text-sm rounded-md transition-colors',
                     child.active
                       ? 'text-primary font-medium' // Different active style for children?
                       : 'text-gray-600 hover:bg-gray-100 hover:text-gray-800',
                      collapsed ? 'justify-center' : 'justify-start' // Keep consistent alignment if needed
                   ]"
                 >
                   <component v-if="child.icon" :is="child.icon" class="h-4 w-4 mr-2 flex-shrink-0" />
                   <span v-else class="w-4 h-4 mr-2"></span>

                   <span :class="[ collapsed ? 'hidden' : '']">
                     {{ child.name }}
                   </span>
                 </button>
              </div>

            </div>
         </div>
       </div>
    </nav>
  </aside>
</template>

<script setup>
import {
BarChart2,
Calendar,
ChevronDown,
ChevronLeft,
ChevronRight,
CreditCard,
DollarSign,
FileText,
HelpCircle,
Layers,
LogOut,
Package,
PlusCircle,
Settings as SettingsIcon,
ShoppingCart,
Truck,
User,
Users
} from 'lucide-vue-next';
import { ref } from 'vue';

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

const openDropdowns = ref({}); // Use ref for reactivity

const toggleDropdown = (itemName) => {
  // Don't toggle if sidebar is collapsed
  if (props.collapsed) return;
  // Create property if it doesn't exist, otherwise toggle it
  openDropdowns.value[itemName] = !openDropdowns.value[itemName];
};

// Navigation sections (specific to Sidebar)
const navigationSections = [
 {
    title: 'Main',
    items: [
      { name: 'Dashboard', icon: BarChart2, active: true },
      {
        name: 'Orders',
        icon: ShoppingCart,
        active: false, 
        isDropdown: true, 
        children: [ 
          { name: 'All Orders', icon: FileText, active: false },
          { name: 'Add New Order', icon: PlusCircle, active: false }
        ]
      },
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