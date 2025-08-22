<script setup lang="ts">
import { toast } from "vue-sonner";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { Progress } from "@/components/ui/progress";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion";
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table";
import { Input } from "@/components/ui/input";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
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
  Sheet,
  SheetContent,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "@/components/ui/sheet";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselNext,
  CarouselPrevious,
} from "@/components/ui/carousel";

import {
  ArrowLeft,
  Share,
  MoreVertical,
  Star,
  Clock,
  Users,
  DollarSign,
  ExternalLink,
  Search,
  QrCode,
  RefreshCw,
} from "lucide-vue-next";

const eventData = {
  id: "1",
  title: "Premium Coffee Beans - Single Origin Ethiopia",
  description:
    "Experience the rich, full-bodied flavor of our premium single-origin Ethiopian coffee beans. Ethically sourced from small farms in the Yirgacheffe region, these beans offer complex notes of chocolate, citrus, and wine. Perfect for pour-over, espresso, or French press brewing methods.",
  images: ["/placeholder.svg", "/placeholder.svg", "/placeholder.svg"],
  seller: {
    address: "0x742d35Cc6634C0532925a3b8D404d2d8c2d71234",
    avatar: "/placeholder.svg",
    isVerified: true,
    name: "Coffee Roasters Co.",
  },
  currentCount: 45,
  targetCount: 100,
  poolBalance: 45,
  timeLeft: "5 days",
  price: 1,
  status: "active",
  deadline: "2024-01-15T00:00:00Z",
  winner: null,
  userHasBought: false,
  isOwner: false,
};

const bidsData = [
  {
    txHash: "0xabcd...1234",
    from: "0x1234...5678",
    timestamp: "2024-01-10T14:30:00Z",
    status: "success",
  },
  {
    txHash: "0xefgh...5678",
    from: "0x9012...3456",
    timestamp: "2024-01-10T14:25:00Z",
    status: "success",
  },
  {
    txHash: "0xijkl...9012",
    from: "0x7890...1234",
    timestamp: "2024-01-10T14:20:00Z",
    status: "success",
  },
];

const showBuyDialog = ref(false);
const statusFilter = ref("all");
const searchTerm = ref("");

const handleBuy = () => {
  showBuyDialog.value = false;
  toast({
    title: "Processing Purchase",
    description: "Redirecting to confirm transaction...",
  });
  // Would navigate to /tx/buy?eventId=... in real app
};

const handleWithdraw = () => {
  toast({
    title: "Processing Withdrawal",
    description: "Redirecting to confirm transaction...",
  });
  // Would navigate to /tx/withdraw?eventId=... in real app
};

const handleClaimRefund = () => {
  toast({
    title: "Processing Refund",
    description: "Redirecting to confirm transaction...",
  });
  // Would navigate to /tx/refund?eventId=... in real app
};

const formatAddress = (address: string) => {
  return `${address.slice(0, 6)}...${address.slice(-4)}`;
};

const formatTimestamp = (timestamp: string) => {
  return new Date(timestamp).toLocaleString();
};

const filteredBids = bidsData.filter((bid) => {
  const matchesSearch =
    bid.from.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
    bid.txHash.toLowerCase().includes(searchTerm.value.toLowerCase());
  const matchesStatus =
    statusFilter.value === "all" || bid.status === statusFilter.value;
  return matchesSearch && matchesStatus;
});

const progressPercentage =
  (eventData.currentCount / eventData.targetCount) * 100;

const router = useRouter();

const openTrx = (txHash: string) => {
  window.open(`https://etherscan.io/tx/${txHash}`, "_blank");
};

</script>

<template>
  <div class="min-h-screen bg-background pb-20">
    <!-- {/* Header */} -->
    <header
      class="sticky top-0 z-40 border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60"
    >
      <div class="flex h-14 items-center justify-between px-4">
        <Button variant="ghost" size="icon" @click="router.back">
          <ArrowLeft class="h-5 w-5" />
        </Button>
        <h1 class="text-lg font-semibold truncate flex-1 mx-4">
          {{ eventData.title }}
        </h1>
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="ghost" size="icon">
              <MoreVertical class="h-5 w-5" />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuItem>
              <Share class="h-4 w-4 mr-2" />
              Share Event
            </DropdownMenuItem>
            <DropdownMenuItem class="text-destructive">
              Report Event
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      </div>
    </header>

    <div class="space-y-6">
      <!-- {/* Hero Card */} -->
      <Card class="rounded-none border-0 border-b">
        <CardContent class="p-0">
          <!-- {/* Image Carousel */} -->
          <div class="relative aspect-[4/3]">
            <Carousel class="w-full">
              <CarouselContent>
                <CarouselItem
                  v-for="(image, index) in eventData.images"
                  :key="index"
                >
                  <img
                    :src="image"
                    :alt="`${eventData.title} ${index + 1}`"
                    class="w-full h-full object-cover"
                  />
                </CarouselItem>
              </CarouselContent>
              <CarouselPrevious class="left-4" />
              <CarouselNext class="right-4" />
            </Carousel>
          </div>

          <!-- {/* Content */} -->
          <div class="p-4 space-y-4">
            <div>
              <h1 class="text-xl font-bold">{{ eventData.title }}</h1>
              <div class="flex items-center gap-2 mt-2">
                <Avatar class="h-6 w-6">
                  <AvatarImage
                    :src="eventData.seller.avatar"
                    :alt="eventData.seller.name"
                  />
                  <AvatarFallback>{{
                    eventData.seller.name[0]
                  }}</AvatarFallback>
                </Avatar>
                <span class="text-sm text-muted-foreground">{{
                  eventData.seller.name
                }}</span>
                <Star
                  v-if="eventData.seller.isVerified"
                  class="h-4 w-4 text-primary fill-primary"
                />
              </div>
            </div>

            <p class="text-muted-foreground">{{ eventData.description }}</p>

            <!-- {/* Stats Chips */} -->
            <div class="flex flex-wrap gap-2">
              <div
                class="inline-flex items-center gap-2 rounded-full px-3 py-1 text-sm bg-primary/10 text-primary"
              >
                <Users class="h-4 w-4" />
                {{ eventData.currentCount }}/{{ eventData.targetCount }} Joined
              </div>
              <div
                class="inline-flex items-center gap-2 rounded-full px-3 py-1 text-sm bg-secondary"
              >
                <Clock class="h-4 w-4" />
                {{ eventData.timeLeft }} left
              </div>
              <div
                class="inline-flex items-center gap-2 rounded-full px-3 py-1 text-sm bg-success/10 text-success"
              >
                <DollarSign class="h-4 w-4" />
                ${{ eventData.poolBalance }} Pool
              </div>
            </div>

            <!-- {/* Progress */} -->
            <div class="space-y-2">
              <div class="flex justify-between text-sm">
                <span>Target Progress</span>
                <span>{{ Math.round(progressPercentage) }}%</span>
              </div>
              <Progress :value="progressPercentage" class="h-2"></Progress>
            </div>

            <!-- {/* CTA */} -->
            <div class="flex gap-3">
              <Button
                v-if="eventData.userHasBought"
                disabled
                class="flex-1 h-11"
              >
                Already Purchased
              </Button>

              <Dialog v-if="!eventData.userHasBought" :open="showBuyDialog">
                <DialogTrigger as-child>
                  <Button class="flex-1 h-11"> Buy $1 Coupon </Button>
                </DialogTrigger>
                <DialogContent>
                  <DialogHeader>
                    <DialogTitle>Confirm Purchase</DialogTitle>
                    <DialogDescription>
                      You are about to purchase a $1 coupon for
                      "{eventData.title}".
                    </DialogDescription>
                  </DialogHeader>
                  <div class="space-y-4">
                    <div class="rounded-lg border p-4 space-y-2">
                      <div class="flex justify-between text-sm">
                        <span>Coupon Price:</span>
                        <span class="font-semibold">$1.00</span>
                      </div>
                      <div class="flex justify-between text-sm">
                        <span>Network Fee:</span>
                        <span>~$0.05</span>
                      </div>
                    </div>
                    <p class="text-sm text-muted-foreground">
                      Maximum 1 coupon per wallet for fairness. Funds are held
                      in escrow until target is reached or deadline passes.
                    </p>
                  </div>
                  <DialogFooter>
                    <Button
                      variant="outline"
                      @click="() => (showBuyDialog = false)"
                    >
                      Cancel
                    </Button>
                    <Button @click="handleBuy()"> Confirm Purchase </Button>
                  </DialogFooter>
                </DialogContent>
              </Dialog>

              <Button
                v-if="
                  eventData.isOwner &&
                  eventData.status === 'awaiting_redemption'
                "
                @click="handleWithdraw()"
                class="flex-1 h-11"
              >
                Withdraw Funds
              </Button>
              <Badge
                v-else
                variant="outline"
                class="flex-1 justify-center h-11"
              >
                Your Event
              </Badge>
            </div>
          </div>
        </CardContent>
      </Card>

      <!-- {/* How It Works */} -->
      <div class="px-4">
        <Card class="rounded-2xl">
          <CardContent>
            <Accordion type="single" collapsible>
              <AccordionItem value="how-it-works" class="border-0">
                <AccordionTrigger class="hover:no-underline py-0">
                  How It Works
                </AccordionTrigger>
                <AccordionContent
                  class="space-y-3 text-sm text-muted-foreground mt-4"
                >
                  <p>• Purchase a $1 coupon to enter the draw</p>
                  <p>• Funds are held in escrow until target is reached</p>
                  <p>• Winner selected via VRF (verifiable random function)</p>
                  <p>• Winner receives QR code for in-person redemption</p>
                  <p>• If target not met by deadline, automatic refunds</p>
                  <p>• Seller receives funds only after redemption approval</p>
                </AccordionContent>
              </AccordionItem>
            </Accordion>
          </CardContent>
        </Card>
      </div>

      <!-- {/* Bids Table */} -->
      <div class="px-4">
        <Card class="rounded-2xl">
          <CardHeader>
            <CardTitle>Bids ({{ bidsData.length }})</CardTitle>
            <CardDescription
              >All coupon purchases for this event</CardDescription
            >
          </CardHeader>
          <CardContent class="space-y-4">
            <!-- {/* Filters */} -->
            <div class="flex gap-2">
              <div class="relative flex-1">
                <Search
                  class="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-muted-foreground"
                />
                <Input
                  placeholder="Search by address or tx..."
                  :value="searchTerm"
                  class="pl-10"
                />
              </div>
              <Select :value="statusFilter">
                <SelectTrigger class="w-32">
                  <SelectValue />
                </SelectTrigger>
                <SelectContent>
                  <SelectItem value="all">All</SelectItem>
                  <SelectItem value="success">Success</SelectItem>
                  <SelectItem value="refunded">Refunded</SelectItem>
                </SelectContent>
              </Select>
            </div>

            <!-- {/* Table */} -->
            <div class="rounded-lg border">
              <Table>
                <TableHeader>
                  <TableRow>
                    <TableHead class="text-xs">Tx Hash</TableHead>
                    <TableHead class="text-xs">From</TableHead>
                    <TableHead class="text-xs">Time</TableHead>
                    <TableHead class="text-xs">Action</TableHead>
                  </TableRow>
                </TableHeader>
                <TableBody>
                  <TableRow v-for="bid in filteredBids" :key="bid.txHash">
                    <TableCell>
                      <span
                        class="text-xs font-mono truncate max-w-[80px] block"
                      >
                        {{ formatAddress(bid.txHash) }}
                      </span>
                    </TableCell>
                    <TableCell>
                      <span class="text-xs font-mono">
                        {{ formatAddress(bid.from) }}
                      </span>
                    </TableCell>
                    <TableCell>
                      <span class="text-xs text-muted-foreground">
                        {{ formatTimestamp(bid.timestamp) }}
                      </span>
                    </TableCell>
                    <TableCell>
                      <Button
                        variant="ghost"
                        size="sm"
                        class="h-8 px-2"
                        @click="
                          () =>
                          openTrx(bid.txHash)
                        "
                      >
                        <ExternalLink class="h-3 w-3" />
                      </Button>
                    </TableCell>
                  </TableRow>
                </TableBody>
              </Table>
            </div>

            <div v-if="filteredBids.length === 0" class="text-center py-8">
              <p class="text-muted-foreground">No bids found</p>
            </div>
          </CardContent>
        </Card>
      </div>

      <!-- {/* Winner Section (conditional) */} -->

      <div v-if="eventData.winner" class="px-4">
        <Card class="rounded-2xl border-success/20 bg-success/5">
          <CardHeader>
            <CardTitle class="text-success">Winner Selected!</CardTitle>
            <CardDescription>
              Congratulations to the randomly selected winner
            </CardDescription>
          </CardHeader>
          <CardContent class="space-y-4">
            <div class="flex items-center justify-between">
              <span class="font-mono text-sm"
                >{formatAddress(eventData.winner)}</span
              >
              <Badge variant="outline" class="text-success border-success">
                Winner
              </Badge>
            </div>

            <Sheet v-if="eventData.winner === '0x1234...5678'">
              <SheetTrigger asChild>
                <Button class="w-full">
                  <QrCode class="h-4 w-4 mr-2" />
                  Open Winner Pass
                </Button>
              </SheetTrigger>
              <SheetContent side="bottom" class="h-[90vh]">
                <SheetHeader>
                  <SheetTitle>Winner Pass</SheetTitle>
                </SheetHeader>
                <div
                  class="flex flex-col items-center justify-center space-y-6 mt-8"
                >
                  <div
                    class="w-64 h-64 bg-background border-2 border-primary rounded-lg flex items-center justify-center"
                  >
                    <QrCode class="h-32 w-32 text-primary" />
                  </div>
                  <div class="text-center space-y-2">
                    <p class="font-semibold">Event ID: {eventData.id}</p>
                    <p class="text-sm text-muted-foreground">
                      Show this QR code to the seller for redemption
                    </p>
                    <p class="text-xs text-warning">
                      Approve redemption only when QR is scanned in person
                    </p>
                  </div>
                </div>
              </SheetContent>
            </Sheet>
          </CardContent>
        </Card>
      </div>

      <!-- {/* Refund Section (conditional) */} -->
      <div v-if="eventData.status === 'failed'" class="px-4">
        <Card class="rounded-2xl border-warning/20 bg-warning/5">
          <CardHeader>
            <CardTitle class="text-warning">Target Not Reached</CardTitle>
            <CardDescription>
              The event didn't reach its target by the deadline
            </CardDescription>
          </CardHeader>
          <CardContent>
            <Button onClick="{handleClaimRefund}" class="w-full">
              <RefreshCw class="h-4 w-4 mr-2" />
              Claim Refund
            </Button>
          </CardContent>
        </Card>
      </div>
    </div>
  </div>
</template>
