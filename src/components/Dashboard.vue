<template>
  <div> <div class="mb-6">
      <h1 class="text-2xl font-semibold text-gray-800">Dashboard</h1>
      <p class="text-gray-600">Welcome back, here's an overview of your ERP system.</p>
    </div>

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
                :is="stat.trend === 'up' ? ArrowUp : ArrowDown"
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
              `bg-${stat.color}-100` // Ensure Tailwind safelisting if colors are dynamic like this
            ]"
          >
            <component :is="stat.icon" :class="`h-6 w-6 text-${stat.color}-500`" />
          </div>
        </div>
      </div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
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
                  // Safelist these potentially dynamic classes in tailwind.config.js
                  `bg-${task.priority === 'high' ? 'red' : task.priority === 'medium' ? 'yellow' : 'green'}-100`,
                  `text-${task.priority === 'high' ? 'red' : task.priority === 'medium' ? 'yellow' : 'green'}-700`
                ]"
              >
                {{ task.priority }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

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
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Order ID</th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Customer</th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Date</th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Amount</th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Status</th>
              <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
            </tr>
          </thead>
          <tbody class="bg-white divide-y divide-gray-200">
            <tr v-for="(order, index) in recentOrders" :key="index" class="hover:bg-gray-50">
              <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-800">{{ order.id }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">{{ order.customer }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">{{ order.date }}</td>
              <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-600">{{ order.amount }}</td>
              <td class="px-6 py-4 whitespace-nowrap">
                <span
                  :class="[
                    'px-2 py-1 text-xs rounded-full',
                    order.status === 'Completed' ? 'bg-green-100 text-green-800' :
                    order.status === 'Processing' ? 'bg-blue-100 text-blue-800' :
                    order.status === 'Pending' ? 'bg-yellow-100 text-yellow-800' :
                    'bg-red-100 text-red-800' // Assuming Cancelled maps to red
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
  </div>
</template>

<script setup>
import {
ArrowDown // Replaced TrendDown
, // For activity icon
ArrowUp,
DollarSign, // For activity icon
Package,
ShoppingCart,
Truck,
User,
Users
} from 'lucide-vue-next';

// Data specific to the Dashboard content
const stats = [
 {
    title: 'Total Revenue',
    value: '$54,350.25',
    trend: 'up',
    percentage: '12.5%',
    icon: DollarSign,
    color: 'green' // Ensure these colors are safelisted in tailwind.config.js if dynamically generated
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

const recentActivities = [
 { icon: User, title: 'New customer registered', time: '2 minutes ago' },
 { icon: ShoppingCart, title: 'New order #38294 received', time: '15 minutes ago' },
 { icon: DollarSign, title: 'Payment of $1,250.00 received', time: '45 minutes ago' },
 { icon: Package, title: 'Inventory updated for 12 products', time: '1 hour ago' }
];

const tasks = [
 { title: 'Review new order requests', completed: false, dueDate: 'Today', priority: 'high' },
 { title: 'Prepare monthly financial report', completed: false, dueDate: 'Tomorrow', priority: 'medium' },
 { title: 'Update product inventory', completed: true, dueDate: 'Yesterday', priority: 'low' },
 { title: 'Contact supplier about delayed shipment', completed: false, dueDate: 'Today', priority: 'high' }
];

const recentOrders = [
 { id: 'ORD-38294', customer: 'John Smith', date: 'Apr 7, 2025', amount: '$1,250.00', status: 'Completed' },
 { id: 'ORD-38293', customer: 'Sarah Johnson', date: 'Apr 7, 2025', amount: '$890.50', status: 'Processing' },
 { id: 'ORD-38292', customer: 'Michael Brown', date: 'Apr 6, 2025', amount: '$2,350.75', status: 'Pending' },
 { id: 'ORD-38291', customer: 'Emily Davis', date: 'Apr 6, 2025', amount: '$450.25', status: 'Cancelled' },
 { id: 'ORD-38290', customer: 'Robert Wilson', date: 'Apr 5, 2025', amount: '$1,875.00', status: 'Completed' }
];
</script>