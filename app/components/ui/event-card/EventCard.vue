<script setup lang="ts">
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Progress } from "@/components/ui/progress";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Clock, Users, DollarSign } from "lucide-vue-next";

interface EventCardProps {
  id: string;
  title: string;
  image: string;
  seller: {
    name: string;
    avatar?: string;
    verified?: boolean;
  };
  target: number;
  joined: number;
  timeLeft: string;
  price: number;
  status: "active" | "ended" | "winner-selected";
  category: string;
  onBuy?: () => void;
  onView?: () => void;
}

const {
  id,
  title,
  image,
  seller,
  target,
  joined,
  timeLeft,
  price,
  status,
  category,
  onBuy,
  onView,
} = defineProps<EventCardProps>();

const progress = (joined / target) * 100;
const isActive = status === "active";
const isTargetMet = joined >= target;
</script>

<template>
  <Card
    :class="[
      'overflow-hidden transition-all duration-300 hover:shadow-card cursor-pointer pt-0',
    ]"
    @click="onView"
  >
    <div class="relative">
      <img :src="image" :alt="title" class="w-full h-48 object-cover" />
      <div class="absolute top-3 left-3">
        <Badge variant="secondary" class="text-xs"> {{category}} </Badge>
      </div>
      <div class="absolute top-3 right-3">
        <Badge
          :variant="isActive ? 'default' : 'secondary'"
          :class="['text-xs', isActive ? 'gradient-primary' : '']"
        >
          {{ isActive ? timeLeft : status.replace("-", " ") }}
        </Badge>
      </div>
    </div>

    <CardContent class="p-4 gap-x-4">
      <!-- Title and Seller -->
      <div class="space-y-2">
        <h3 class="font-semibold text-lg leading-tight line-clamp-2">
          {{ title }}
        </h3>

        <div class="flex items-center space-x-2">
          <Avatar class="h-6 w-6">
            <AvatarImage :src="seller.avatar ?? ''" />
            <AvatarFallback class="text-xs">
              {{ seller.name.charAt(0) }}
            </AvatarFallback>
          </Avatar>
          <span class="text-sm text-muted-foreground">
            {{ seller.name }}
          </span>
          <Badge
            v-if="seller.verified"
            variant="secondary"
            class="text-xs px-1.5 py-0.5"
          >
            ✓
          </Badge>
        </div>
      </div>

      <!-- Progress -->
      <div class="space-y-2">
        <div class="flex items-center justify-between text-sm">
          <span class="text-muted-foreground">Progress</span>
          <span class="font-medium">{{ joined }}/{{ target }}</span>
        </div>
        <Progress
          :value="progress"
          :class="['h-2', isTargetMet ? 'gradient-success' : '']"
        ></Progress>
      </div>

      <!-- {/* Stats */} -->
      <div
        class="flex items-center justify-between text-sm text-muted-foreground"
      >
        <div class="flex items-center space-x-1">
          <Users class="h-4 w-4" />
          <span>{{ joined }} joined</span>
        </div>
        <div class="flex items-center space-x-1">
          <Clock class="h-4 w-4" />
          <span>{{ timeLeft }}</span>
        </div>
      </div>

      <!-- {/* Action Button */} -->
      <Button
        :disabled="!isActive || isTargetMet"
        :class="[
          'w-full mt-4',
          isActive && !isTargetMet ? 'gradient-primary text-white' : '',
        ]"
        :variant="isActive && !isTargetMet ? 'default' : 'secondary'"
      >
        <DollarSign class="mr-2 h-4 w-4" />
        {{ isTargetMet ? "Target Reached" : `Buy $${price}` }}
      </Button>
    </CardContent>
  </Card>
</template>
