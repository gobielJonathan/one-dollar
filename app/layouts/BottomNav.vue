<script setup lang="ts">
import { BarChart3, Plus, Search, Settings, Ticket } from "lucide-vue-next";

const navItems = [
  { href: "/", label: "Explore", icon: Search },
  { href: "/my-coupons", label: "Coupons", icon: Ticket },
  { href: "/create", label: "Create", icon: Plus },
  { href: "/dashboard", label: "Dashboard", icon: BarChart3 },
];

const route = useRoute();
</script>

<template>
  <nav
    class="fixed bottom-0 left-0 right-0 z-50 border-t bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60"
  >
    <div class="grid grid-cols-4 h-16">
      <NuxtLink
        v-for="item in navItems"
        :key="item.href"
        :to="item.href"
        class="flex flex-col items-center justify-center space-y-1 text-xs font-medium transition-colors"
      >
        <div
          :class="[
            'rounded-lg p-1.5 transition-all',
            route.path === item.href ? 'gradient-primary shadow-soft' : '',
          ]"
        >
          <component
            :is="item.icon"
            :class="['h-4 w-4', route.path === item.href ? 'text-white' : '']"
          />
        </div>
        <span
          :class="[route.path === item.href && 'text-primary font-semibold']"
        >
          {{ item.label }}
        </span>
      </NuxtLink>
    </div>
  </nav>
</template>
