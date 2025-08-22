<script setup lang="ts">
import "echarts";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Progress } from "@/components/ui/progress";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog";

import {
  BarChart3,
  TrendingUp,
  Clock,
  DollarSign,
  Eye,
  Download,
} from "lucide-vue-next";
import { Card, CardHeader, CardContent, CardTitle, CardDescription } from "~/components/ui/card";
import { toast } from "vue-sonner";

const chartData = [
  { day: "Mon", bids: 12 },
  { day: "Tue", bids: 19 },
  { day: "Wed", bids: 15 },
  { day: "Thu", bids: 25 },
  { day: "Fri", bids: 31 },
  { day: "Sat", bids: 22 },
  { day: "Sun", bids: 18 },
];

const events = [
  {
    id: "1",
    title: "Premium Coffee Beans",
    sold: 45,
    target: 100,
    expiry: "2024-01-15",
    poolBalance: 45,
    status: "active",
  },
  {
    id: "2",
    title: "Handmade Artisan Soap",
    sold: 100,
    target: 100,
    expiry: "2024-01-10",
    poolBalance: 100,
    status: "awaiting_redemption",
  },
  {
    id: "3",
    title: "Organic Honey Jar",
    sold: 25,
    target: 50,
    expiry: "2024-01-20",
    poolBalance: 25,
    status: "active",
  },
];

const stats = [
  {
    title: "Today's Bids",
    value: "18",
    change: "+12%",
    icon: TrendingUp,
  },
  {
    title: "7-Day Bids",
    value: "142",
    change: "+8%",
    icon: BarChart3,
  },
  {
    title: "Active Events",
    value: "2",
    change: "0%",
    icon: Clock,
  },
  {
    title: "Pending Payout",
    value: "$100",
    change: "+$100",
    icon: DollarSign,
  },
];

const chartRange = ref("7d");
const selectedEvent = ref<(typeof events)[0] | null>(null);

const router = useRouter();

const handleWithdraw = (event: (typeof events)[0]) => {
  selectedEvent.value = event;
};

const confirmWithdraw = () => {
  if (selectedEvent) {
    toast({
      title: "Processing Withdrawal",
      description: "Redirecting to confirm transaction...",
    });
    // Would navigate to /tx/withdraw?eventId=... in real app
    selectedEvent.value = null;
  }
};
</script>
<template>
  <div class="min-h-screen bg-background pb-20">
    <!-- {/* Header */} -->
    <header
      class="sticky top-0 z-40 border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60"
    >
      <div class="flex h-14 items-center justify-between px-4">
        <h1 class="text-lg font-semibold">Dashboard</h1>
        <div class="flex items-center gap-2">
          <Badge variant="outline" class="text-xs"> Ethereum </Badge>
        </div>
      </div>
    </header>

    <div class="px-4 py-6 space-y-6">
      <!-- {/* Stats Row */} -->
      <div class="grid grid-cols-2 gap-3">
        <Card v-for="value in stats" :key="value.title" class="rounded-2xl">
          <CardContent>
            <div class="flex items-center justify-between">
              <div>
                <p class="text-sm text-muted-foreground">
                  {{ value.title }}
                </p>
                <p class="text-2xl font-bold">{{ value.value }}</p>
                <p class="text-xs text-success">{{ value.change }}</p>
              </div>
              <component :is="value.icon" class="h-8 w-8 text-primary" />
            </div>
          </CardContent>
        </Card>
      </div>

      <!-- {/* Chart Section */} -->
      <Card class="rounded-2xl">
        <CardHeader class="pb-3">
          <div class="flex items-center justify-between">
            <div>
              <CardTitle>Bids per Day</CardTitle>
              <CardDescription>Daily coupon purchases</CardDescription>
            </div>
            <div class="flex gap-1">
              <Button
                v-for="range in ['7d', '14d', '30d']"
                :key="range"
                :variant="chartRange === range ? 'default' : 'ghost'"
                :size="'sm'"
                @click="() => (chartRange = range)"
                class="h-8 px-3 text-xs"
              >
                {{ range }}
              </Button>
            </div>
          </div>
        </CardHeader>
        <CardContent class="p-4">
          <div class="h-64">
            <!-- <ResponsiveContainer width="100%" height="100%">
                <LineChart data={chartData}>
                  <CartesianGrid strokeDasharray="3 3" class="stroke-muted" />
                  <XAxis 
                    dataKey="day" 
                    class="text-xs"
                    tick={{ fontSize: 12 }}
                  />
                  <YAxis 
                    class="text-xs"
                    tick={{ fontSize: 12 }}
                  />
                  <Tooltip 
                    contentStyle={{
                      backgroundColor: 'hsl(var(--background))',
                      border: '1px solid hsl(var(--border))',
                      borderRadius: '8px',
                    }}
                  />
                  <Line 
                    type="monotone" 
                    dataKey="bids" 
                    stroke="hsl(var(--primary))" 
                    strokeWidth={2}
                    dot={{ fill: 'hsl(var(--primary))', strokeWidth: 2, r: 4 }}
                    activeDot={{ r: 6, fill: 'hsl(var(--primary))' }}
                  />
                </LineChart>
              </ResponsiveContainer> -->
          </div>
        </CardContent>
      </Card>

      <!-- {/* Events Table */} -->
      <Card class="rounded-2xl">
        <CardHeader>
          <CardTitle>My Events</CardTitle>
          <CardDescription
            >Manage your active and completed events</CardDescription
          >
        </CardHeader>
        <CardContent class="p-2">
          <div v-if="events.length === 0" class="text-center py-12">
            <BarChart3
              class="h-12 w-12 mx-auto text-muted-foreground mb-4"
            />
            <h3 class="text-lg font-semibold mb-2">No events yet</h3>
            <p class="text-muted-foreground mb-4">
              Create your first event to start selling.
            </p>
            <Button @click="() => router.push('/create')">
              Create Event
            </Button>
          </div>

          <div v-if="events.length > 0" class="rounded-lg border">
            <Table>
              <TableHeader>
                <TableRow>
                  <TableHead class="text-xs">Event</TableHead>
                  <TableHead class="text-xs">Progress</TableHead>
                  <TableHead class="text-xs">Status</TableHead>
                  <TableHead class="text-xs">Action</TableHead>
                </TableRow>
              </TableHeader>
              <TableBody>
                <TableRow v-for="event in events" :key="event.id">
                  <TableCell>
                    <div>
                      <p class="font-medium text-sm truncate max-w-[120px]">
                        {{ event.title }}
                      </p>
                      <p class="text-xs text-muted-foreground">
                        Pool: {{ event.poolBalance }}
                      </p>
                    </div>
                  </TableCell>
                  <TableCell>
                    <div class="space-y-1">
                      <div
                        class="flex items-center justify-between text-xs"
                      >
                        <span>{{ event.sold }}/{{ event.target }}</span>
                        <span
                          >{{
                            Math.round((event.sold / event.target) * 100)
                          }}%</span
                        >
                      </div>
                      <Progress
                        :value="(event.sold / event.target) * 100"
                        class="h-2"
                      ></Progress>
                    </div>
                  </TableCell>
                  <TableCell>
                    <Badge v-if="event.status === 'active'" variant="default"
                      >Active</Badge
                    >
                    <Badge
                      v-else-if="event.status === 'awaiting_redemption'"
                      class="bg-warning text-warning-foreground"
                      >Awaiting Redemption</Badge
                    >
                    <Badge
                      v-else-if="event.status === 'completed'"
                      variant="secondary"
                      >Completed</Badge
                    >
                    <Badge v-else variant="outline">{{ event.status }}</Badge>
                  </TableCell>
                  <TableCell>
                    <div class="flex gap-1">
                      <Button
                        variant="ghost"
                        size="icon"
                        class="h-8 w-8"
                        @click="() => router.push(`/event/${event.id}`)"
                      >
                        <Eye class="h-4 w-4" />
                      </Button>
                      <Button
                        v-if="event.status === 'awaiting_redemption'"
                        variant="default"
                        size="sm"
                        class="h-8 px-2 text-xs inline-flex items-center"
                        @click="() => handleWithdraw(event)"
                      >
                        <Download class="h-3 w-3 mr-1" />
                        Withdraw
                      </Button>
                    </div>
                  </TableCell>
                </TableRow>
              </TableBody>
            </Table>
          </div>
        </CardContent>
      </Card>
    </div>

    <!-- {/* Withdraw Dialog */} -->
    <Dialog :open="!!selectedEvent">
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Confirm Withdrawal</DialogTitle>
          <DialogDescription>
            You are about to withdraw funds from "{selectedEvent?.title}".
          </DialogDescription>
        </DialogHeader>
        <div class="space-y-4">
          <div class="rounded-lg border p-4 space-y-2">
            <div class="flex justify-between text-sm">
              <span>Pool Balance:</span>
              <span class="font-semibold"
                >${selectedEvent?.poolBalance}</span
              >
            </div>
            <div class="flex justify-between text-sm">
              <span>Platform Fee (2.5%):</span>
              <span
                >-${selectedEvent ? (selectedEvent.poolBalance *
                0.025).toFixed(2) : 0}</span
              >
            </div>
            <div class="border-t pt-2 flex justify-between font-semibold">
              <span>You Receive:</span>
              <span
                >${selectedEvent ? (selectedEvent.poolBalance *
                0.975).toFixed(2) : 0}</span
              >
            </div>
          </div>
          <p class="text-sm text-muted-foreground">
            Funds will be sent to your payout address. This action cannot be
            undone.
          </p>
        </div>
        <DialogFooter>
          <Button variant="outline" @click="() => (selectedEvent = null)">
            Cancel
          </Button>
          <Button @click="confirmWithdraw()"> Confirm Withdrawal </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  </div>
</template>
