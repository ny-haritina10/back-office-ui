`<template>
  <div class="flex h-screen bg-gray-50">
    <!-- Sidebar -->
    <aside 
      :class="[
        'bg-white border-r border-gray-200 transition-all duration-300 ease-in-out',
        sidebarCollapsed ? 'w-20' : 'w-64'
      ]"
      class="overflow-y-auto overflow-x-auto" 
    >
      <div class="flex items-center justify-between h-16 px-4 border-b border-gray-200">
        <div class="flex items-center">
          <component :is="Layers" class="h-8 w-8 text-primary" />
          <span 
            :class="[
              'ml-2 text-lg font-semibold text-gray-800 transition-opacity duration-200',
              sidebarCollapsed ? 'opacity-0 hidden' : 'opacity-100'
            ]"
          >
            ERP System
          </span>
        </div>
        <button 
          @click="toggleSidebar" 
          class="p-1 rounded-md hover:bg-gray-100 text-gray-500"
        >
          <chevron-left v-if="!sidebarCollapsed" class="h-5 w-5" />
          <chevron-right v-else class="h-5 w-5" />
        </button>
      </div>
      
      <nav class="mt-4 px-2">
        <div v-for="(section, index) in navigationSections" :key="index" class="mb-6">
          <div 
            :class="[
              'text-xs font-semibold text-gray-500 uppercase tracking-wider mb-2 px-3',
              sidebarCollapsed ? 'text-center' : ''
            ]"
            v-if="!sidebarCollapsed"
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
                sidebarCollapsed ? 'justify-center' : 'justify-start'
              ]"
            >
              <component :is="item.icon" class="h-5 w-5" />
              <span 
                :class="[
                  'transition-opacity duration-200',
                  sidebarCollapsed ? 'hidden' : 'ml-3'
                ]"
              >
                {{ item.name }}
              </span>
            </button>
          </div>
        </div>
      </nav>
    </aside>

    <!-- Main Content -->
    <div class="flex flex-col flex-1 overflow-hidden">
      <!-- Header -->
      <header class="bg-white border-b border-gray-200 h-16 flex items-center justify-between px-6">
        <div class="flex items-center">
          <button class="p-1 rounded-md hover:bg-gray-100 text-gray-500 md:hidden">
            <menu class="h-5 w-5" />
          </button>
          <div class="ml-4 relative">
            <div class="flex items-center border border-gray-300 rounded-md px-3 py-1.5 bg-gray-50 focus-within:ring-2 focus-within:ring-primary focus-within:border-primary">
              <search class="h-4 w-4 text-gray-500" />
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
            <bell class="h-5 w-5" />
            <span class="absolute top-0 right-0 h-2 w-2 rounded-full bg-red-500"></span>
          </button>
          
          <button class="p-1.5 rounded-md hover:bg-gray-100 text-gray-500">
            <settings class="h-5 w-5" />
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
              <chevron-down class="h-4 w-4 text-gray-500 hidden md:block" />
            </button>
          </div>
        </div>
      </header>

      <!-- Main Content Area -->
      <main class="flex-1 overflow-auto p-6 bg-gray-50">
        <div class="mb-6">
          <h1 class="text-2xl font-semibold text-gray-800">Dashboard</h1>
          <p class="text-gray-600">Welcome back, here's an overview of your ERP system.</p>
        </div>

        <!-- Stats Cards -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
          <div 
            v-for="(stat, index) in stats" 
            :key="index" 
            class="bg-white rounded-lg shadow p-6 border border-gray-100"
          >
            <div class="flex items-center justify-between">
              <div>
                <p class="text-sm font-medium text-gray-500">{{ stat.title }}</p>
                <p class="text-2xl font-semibold text-gray-800 mt-1">{{ stat.value }}</p>
                <div class="flex items-center mt-2">
                  <component 
                    :is="stat.trend === 'up' ? 'trend-up' : 'trend-down'" 
                    :class="[
                      'h-4 w-4 mr-1',
                      stat.trend === 'up' ? 'text-green-500' : 'text-red-500'
                    ]" 
                  />
                  <span 
                    :class="[
                      'text-xs font-medium',
                      stat.trend === 'up' ? 'text-green-500' : 'text-red-500'
                    ]"
                  >
                    {{ stat.percentage }}
                  </span>
                  <span class="text-xs text-gray-500 ml-1">vs last month</span>
                </div>
              </div>
              <div 
                :class="[
                  'p-3 rounded-full',
                  `bg-${stat.color}-100`
                ]"
              >
                <component :is="stat.icon" :class="`h-6 w-6 text-${stat.color}-500`" />
              </div>
            </div>
          </div>
        </div>

        <!-- Recent Activity and Tasks -->
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
          <!-- Recent Activity -->
          <div class="bg-white rounded-lg shadow border border-gray-100">
            <div class="p-6 border-b border-gray-100">
              <div class="flex items-center justify-between">
                <h2 class="text-lg font-semibold text-gray-800">Recent Activity</h2>
                <button class="text-sm text-primary font-medium hover:underline">View All</button>
              </div>
            </div>
            <div class="p-6">
              <div class="space-y-6">
                <div v-for="(activity, index) in recentActivities" :key="index" class="flex">
                  <div class="mr-4">
                    <div class="flex items-center justify-center h-10 w-10 rounded-full bg-gray-100">
                      <component :is="activity.icon" class="h-5 w-5 text-gray-500" />
                    </div>
                  </div>
                  <div>
                    <p class="text-sm font-medium text-gray-800">{{ activity.title }}</p>
                    <p class="text-xs text-gray-500 mt-1">{{ activity.time }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Tasks -->
          <div class="bg-white rounded-lg shadow border border-gray-100">
            <div class="p-6 border-b border-gray-100">
              <div class="flex items-center justify-between">
                <h2 class="text-lg font-semibold text-gray-800">Tasks</h2>
                <button class="text-sm text-primary font-medium hover:underline">View All</button>
              </div>
            </div>
            <div class="p-6">
              <div class="space-y-4">
                <div 
                  v-for="(task, index) in tasks" 
                  :key="index" 
                  class="flex items-center p-3 rounded-md hover:bg-gray-50"
                >
                  <input 
                    type="checkbox" 
                    :checked="task.completed" 
                    class="h-4 w-4 text-primary rounded border-gray-300 focus:ring-primary"
                  />
                  <div class="ml-3 flex-1">
                    <p 
                      :class="[
                        'text-sm font-medium',
                        task.completed ? 'text-gray-400 line-through' : 'text-gray-800'
                      ]"
                    >
                      {{ task.title }}
                    </p>
                    <p class="text-xs text-gray-500 mt-1">Due {{ task.dueDate }}</p>
                  </div>
                  <div 
                    :class="[
                      'text-xs px-2 py-1 rounded-full',
                      `bg-${task.priority}-100 text-${task.priority}-700`
                    ]"
                  >
                    {{ task.priority }}
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Recent Orders -->
        <div class="bg-white rounded-lg shadow border border-gray-100">
          <div class="p-6 border-b border-gray-100">
            <div class="flex items-center justify-between">
              <h2 class="text-lg font-semibold text-gray-800">Recent Orders</h2>
              <button class="text-sm text-primary font-medium hover:underline">View All</button>
            </div>
          </div>
          <div class="overflow-x-auto">
            <table class="min-w-full divide-y divide-gray-200">
              <thead class="bg-gray-50">
                <tr>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Order ID
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Customer
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Date
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Amount
                  </th>
                  <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Status
                  </th>
                  <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">
                    Actions
                  </th>
                </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
                <tr v-for="(order, index) in recentOrders" :key="index" class="hover:bg-gray-50">
                  <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-800">
                    {{ order.id }}
                  </td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                    {{ order.customer }}
                  </td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                    {{ order.date }}
                  </td>
                  <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">
                    {{ order.amount }}
                  </td>
                  <td class="px-6 py-4 whitespace-nowrap">
                    <span 
                      :class="[
                        'px-2 py-1 text-xs rounded-full',
                        order.status === 'Completed' ? 'bg-green-100 text-green-800' : 
                        order.status === 'Processing' ? 'bg-blue-100 text-blue-800' : 
                        order.status === 'Pending' ? 'bg-yellow-100 text-yellow-800' : 
                        'bg-red-100 text-red-800'
                      ]"
                    >
                      {{ order.status }}
                    </span>
                  </td>
                  <td class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium">
                    <button class="text-primary hover:text-primary-dark">View</button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </main>

      <!-- Footer -->
      <footer class="bg-white border-t border-gray-200 py-4 px-6">
        <div class="flex flex-col md:flex-row justify-between items-center">
          <div class="text-sm text-gray-500">
            &copy; 2025 ERP System. All rights reserved.
          </div>
          <div class="flex space-x-4 mt-2 md:mt-0">
            <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Privacy Policy</a>
            <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Terms of Service</a>
            <a href="#" class="text-sm text-gray-500 hover:text-gray-700">Contact Support</a>
          </div>
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup>
import {
BarChart2,
Bell,
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
Menu,
Package,
Search,
Settings,
Settings as SettingsIcon,
ShoppingCart,
Truck,
User,
Users
} from 'lucide-vue-next';
import { ref } from 'vue';

// Sidebar state
const sidebarCollapsed = ref(false);

const toggleSidebar = () => {
  sidebarCollapsed.value = !sidebarCollapsed.value;
};

// Navigation sections
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

// Stats data
const stats = [
  { 
    title: 'Total Revenue', 
    value: '$54,350.25', 
    trend: 'up', 
    percentage: '12.5%',
    icon: DollarSign,
    color: 'green'
  },
  { 
    title: 'Total Orders', 
    value: '1,254', 
    trend: 'up', 
    percentage: '8.2%',
    icon: ShoppingCart,
    color: 'blue'
  },
  { 
    title: 'New Customers', 
    value: '358', 
    trend: 'down', 
    percentage: '3.1%',
    icon: Users,
    color: 'purple'
  },
  { 
    title: 'Pending Shipments', 
    value: '42', 
    trend: 'up', 
    percentage: '5.4%',
    icon: Truck,
    color: 'yellow'
  }
];

// Recent activities
const recentActivities = [
  {
    icon: User,
    title: 'New customer registered',
    time: '2 minutes ago'
  },
  {
    icon: ShoppingCart,
    title: 'New order #38294 received',
    time: '15 minutes ago'
  },
  {
    icon: DollarSign,
    title: 'Payment of $1,250.00 received',
    time: '45 minutes ago'
  },
  {
    icon: Package,
    title: 'Inventory updated for 12 products',
    time: '1 hour ago'
  }
];

// Tasks
const tasks = [
  {
    title: 'Review new order requests',
    completed: false,
    dueDate: 'Today',
    priority: 'high'
  },
  {
    title: 'Prepare monthly financial report',
    completed: false,
    dueDate: 'Tomorrow',
    priority: 'medium'
  },
  {
    title: 'Update product inventory',
    completed: true,
    dueDate: 'Yesterday',
    priority: 'low'
  },
  {
    title: 'Contact supplier about delayed shipment',
    completed: false,
    dueDate: 'Today',
    priority: 'high'
  }
];

// Recent orders
const recentOrders = [
  {
    id: 'ORD-38294',
    customer: 'John Smith',
    date: 'Apr 7, 2025',
    amount: '$1,250.00',
    status: 'Completed'
  },
  {
    id: 'ORD-38293',
    customer: 'Sarah Johnson',
    date: 'Apr 7, 2025',
    amount: '$890.50',
    status: 'Processing'
  },
  {
    id: 'ORD-38292',
    customer: 'Michael Brown',
    date: 'Apr 6, 2025',
    amount: '$2,350.75',
    status: 'Pending'
  },
  {
    id: 'ORD-38291',
    customer: 'Emily Davis',
    date: 'Apr 6, 2025',
    amount: '$450.25',
    status: 'Cancelled'
  },
  {
    id: 'ORD-38290',
    customer: 'Robert Wilson',
    date: 'Apr 5, 2025',
    amount: '$1,875.00',
    status: 'Completed'
  }
];
</script>

<style>
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
</style>`