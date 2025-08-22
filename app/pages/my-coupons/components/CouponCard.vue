<script setup lang="ts">
import {
  Clock,
  ExternalLink,
  QrCode,
  RefreshCw,
  Trophy,
} from "lucide-vue-next";
import { Badge } from "~/components/ui/badge";
import { Button } from "~/components/ui/button";
import { Card, CardContent } from "~/components/ui/card";

defineProps<{
  coupon: {
    id: string;
    eventImage: string;
    eventTitle: string;
    seller: string;
    status: string;
    isWinner: boolean;
    timeLeft: string;
  };
}>();
</script>

<template>
  <Card class="overflow-hidden">
    <CardContent>
      <div class="flex space-x-3">
        <!-- {/* Event Image */} -->
        <img
          :src="coupon.eventImage"
          :alt="coupon.eventTitle"
          class="w-16 h-16 rounded-lg object-cover flex-shrink-0"
        />

        <!-- {/* Content */} -->
        <div class="flex-1 min-w-0 space-y-2">
          <div class="flex items-start justify-between">
            <div class="space-y-1">
              <h3 class="font-medium text-sm leading-tight line-clamp-2">
                {{ coupon.eventTitle }}
              </h3>
              <p class="text-xs text-muted-foreground">
                {{ coupon.seller }}
              </p>
            </div>
            <Badge v-if="coupon.isWinner" class="status-success">
              <Trophy class="mr-1 h-3 w-3" />
              Winner!
            </Badge>

            <Badge v-else-if="coupon.status === 'active'" variant="default"
              >Active</Badge
            >
            <Badge v-else-if="coupon.status === 'refunded'" variant="secondary"
              >Refunded</Badge
            >
            <Badge v-else-if="coupon.status === 'expired'" variant="outline"
              >Expired</Badge
            >
            <Badge v-else variant="secondary">{{ coupon.status }}</Badge>
          </div>

          <div class="flex items-center justify-between">
            <div
              class="flex items-center space-x-1 text-xs text-muted-foreground"
            >
              <Clock class="h-3 w-3" />
              <span>{{ coupon.timeLeft }}</span>
            </div>
            <Button
              v-if="coupon.isWinner"
              size="sm"
              class="gradient-success"
            >
              <QrCode class="mr-2 h-4 w-4" />
              Show QR
            </Button>

            <Button
              v-else-if="coupon.status === 'refunded'"
              size="sm"
              variant="outline"
              disabled
            >
              <RefreshCw class="mr-2 h-4 w-4" />
              Refunded
            </Button>

            <Button v-else size="sm" variant="outline">
              <ExternalLink class="mr-2 h-4 w-4" />
              View Event
            </Button>
          </div>
        </div>
      </div>
    </CardContent>
  </Card>
</template>
