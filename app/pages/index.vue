<script setup lang="ts">
import { Input } from "@/components/ui/input";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
import { EventCard } from "@/components/ui/event-card";
import { Search, Filter, Sparkles } from "lucide-vue-next";
import { Button } from "@/components/ui/button";

const mockEvents = [
  {
    id: "1",
    title: "Premium Coffee Experience",
    image:
      "https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?w=400&h=300&fit=crop",
    seller: {
      name: "Bean & Brew",
      avatar:
        "https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?w=40&h=40&fit=crop&crop=face",
      verified: true,
    },
    target: 100,
    joined: 67,
    timeLeft: "2d 14h",
    price: 1,
    status: "active" as const,
    category: "Food & Drink",
  },
  {
    id: "2",
    title: "Exclusive Tech Gadget Bundle",
    image:
      "https://images.unsplash.com/photo-1468495244123-6c6c332eeece?w=400&h=300&fit=crop",
    seller: {
      name: "TechHub",
      avatar:
        "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=40&h=40&fit=crop&crop=face",
      verified: true,
    },
    target: 50,
    joined: 43,
    timeLeft: "1d 8h",
    price: 1,
    status: "active" as const,
    category: "Electronics",
  },
  {
    id: "3",
    title: "Artisan Bakery Special",
    image:
      "https://images.unsplash.com/photo-1509440159596-0249088772ff?w=400&h=300&fit=crop",
    seller: {
      name: "Local Bakery",
      avatar:
        "https://images.unsplash.com/photo-1494790108755-2616b612b786?w=40&h=40&fit=crop&crop=face",
    },
    target: 75,
    joined: 22,
    timeLeft: "3d 5h",
    price: 1,
    status: "active" as const,
    category: "Food & Drink",
  },
  {
    id: "4",
    title: "Fitness Class Pass",
    image:
      "https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b?w=400&h=300&fit=crop",
    seller: {
      name: "FitLife Studio",
      verified: true,
    },
    target: 30,
    joined: 30,
    timeLeft: "Ended",
    price: 1,
    status: "winner-selected" as const,
    category: "Health & Fitness",
  },
];

const categories = [
  "All Categories",
  "Food & Drink",
  "Electronics",
  "Health & Fitness",
  "Fashion",
  "Services",
  "Entertainment",
];

const statusFilters = ["All Status", "Active", "Ending Soon", "Completed"];
</script>

<template>
  <div className="container max-w-md mx-auto pb-20 pt-4 px-4 space-y-6">
    <!-- Hero Section -->
    <div className="text-center space-y-2 py-6">
      <h1 className="text-2xl font-bold">Discover Amazing Deals</h1>
      <p className="text-muted-foreground">
        Join events for just $1 and win exclusive prizes
      </p>
    </div>

    <!-- Search and Filters -->
    <div className="space-y-3">
      <div className="relative">
        <Search
          className="absolute left-3 top-1/2 transform -translate-y-1/2 h-4 w-4 text-muted-foreground"
        />
        <Input placeholder="Search events..." className="pl-10 h-12" />
      </div>

      <div className="flex space-x-2">
        <Select
        >
          <SelectTrigger className="flex-1">
            <SelectValue />
          </SelectTrigger>
          <SelectContent>
            <SelectItem v-for="value in categories" :key="value" :value="value">
              {{ value }}
            </SelectItem>
          </SelectContent>
        </Select>

        <Select>
          <SelectTrigger className="flex-1">
            <Filter className="mr-2 h-4 w-4" />
            <SelectValue />
          </SelectTrigger>
          <SelectContent>
            <SelectItem
              v-for="value in statusFilters"
              :key="value"
              :value="value"
            >
              {{ value }}
            </SelectItem>
          </SelectContent>
        </Select>
      </div>
    </div>

    <!-- Featured Badge -->
    <div className="flex items-center justify-center space-x-2 py-2">
      <Sparkles className="h-4 w-4 text-primary" />
      <span className="text-sm font-medium text-primary">Featured Events</span>
      <Sparkles className="h-4 w-4 text-primary" />
    </div>

    <!-- Events Grid -->
    <div className="space-y-4">
      <!-- <EventCard v-for="value in mockEvents" :key="value.id" v-bind="value" /> -->
    </div>

    <!-- Load More -->
    <div className="text-center pt-4">
      <Button variant="outline" className="w-full"> Load More Events </Button>
    </div>
  </div>
</template>
