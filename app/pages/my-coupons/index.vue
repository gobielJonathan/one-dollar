<script setup lang="ts">
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import CouponCard from "./components/CouponCard.vue";
import EmptyState from "./components/EmptyState.vue";

const activeTab = ref("all");

 const mockCoupons = [
  {
    id: "1",
    eventTitle: "Premium Coffee Experience",
    eventImage:
      "https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?w=100&h=100&fit=crop",
    seller: "Bean & Brew",
    status: "active",
    timeLeft: "2d 14h",
    purchaseDate: "2024-01-15",
    isWinner: false,
  },
  {
    id: "2",
    eventTitle: "Exclusive Tech Gadget Bundle",
    eventImage:
      "https://images.unsplash.com/photo-1468495244123-6c6c332eeece?w=100&h=100&fit=crop",
    seller: "TechHub",
    status: "winner",
    timeLeft: "6h 23m to redeem",
    purchaseDate: "2024-01-10",
    isWinner: true,
    redeemCode: "TH2024001",
  },
  {
    id: "3",
    eventTitle: "Artisan Bakery Special",
    eventImage:
      "https://images.unsplash.com/photo-1509440159596-0249088772ff?w=100&h=100&fit=crop",
    seller: "Local Bakery",
    status: "refunded",
    timeLeft: "Expired",
    purchaseDate: "2024-01-08",
    isWinner: false,
    refundAmount: "$1.00",
  },
];
 const filterCoupons = (
  status: "all" | "winner" | "joined" | "refunded" = "all"
) => {
  if (status === "all") return mockCoupons;
  if (status === "winner") return mockCoupons.filter((c) => c.isWinner);
  if (status === "joined")
    return mockCoupons.filter((c) => c.status === "active");
  if (status === "refunded")
    return mockCoupons.filter((c) => c.status === "refunded");
  return mockCoupons;
};



const all = filterCoupons("all");
const joined = filterCoupons("joined");
const winner = filterCoupons("winner");
const refunded = filterCoupons("refunded");

</script>

<template>
  <div class="container max-w-md mx-auto pb-20 pt-4 px-4">
    <div class="space-y-6">
      <!-- {/* Header */} -->
      <div class="text-center space-y-2">
        <h1 class="text-2xl font-bold">My Coupons</h1>
        <p class="text-muted-foreground">Track your participations and wins</p>
      </div>

      <!-- {/* Tabs */} -->
      <Tabs v-model="activeTab" class="w-full">
        <TabsList class="grid w-full grid-cols-4">
          <TabsTrigger value="all" class="text-xs">All</TabsTrigger>
          <TabsTrigger value="joined" class="text-xs">Joined</TabsTrigger>
          <TabsTrigger value="winner" class="text-xs">Winner</TabsTrigger>
          <TabsTrigger value="refunded" class="text-xs">Refunded</TabsTrigger>
        </TabsList>

        <TabsContent value="all" class="space-y-4 mt-6">
          <EmptyState
            v-if="all.length === 0"
            message="No coupons yet"
            action="Join your first event to get started!"
          />

          <CouponCard v-for="value in all" :key="value.id" :coupon="value" />
        </TabsContent>

        <TabsContent value="joined" class="space-y-4 mt-6">
          <EmptyState
            v-if="joined.length === 0"
            message="No active participations"
            action="Browse events and join for just $1!"
          />

          <CouponCard v-for="value in joined" :key="value.id" :coupon="value" />
        </TabsContent>

        <TabsContent value="winner" class="space-y-4 mt-6">
          <EmptyState
            v-if="winner.length === 0"
            message="No wins yet"
            action="Keep participating for a chance to win!"
          />

          <CouponCard v-for="value in winner" :key="value.id" :coupon="value" />
        </TabsContent>

        <TabsContent value="refunded" class="space-y-4 mt-6">
          <EmptyState
            v-if="refunded.length === 0"
            message="No refunds"
            action="Refunds appear here when events don't reach their target"
          />

          <CouponCard
            v-for="value in refunded"
            :key="value.id"
            :coupon="value"
          />
        </TabsContent>
      </Tabs>
    </div>
  </div>
</template>
