<script setup lang="ts">
import { ChevronDown, User, Wallet } from "lucide-vue-next";
import { Button } from "~/components/ui/button";
import { Badge } from "~/components/ui/badge";
import { Sheet, SheetContent, SheetTrigger } from "~/components/ui/sheet";

const isConnected = ref(false);
const address = ref("0x1234...5678");
const balance = ref("1.2453");

const handleConnect = () => {
  isConnected.value = true;
};
</script>

<template>
  <header
    class="sticky top-0 z-50 w-full border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60"
  >
    <div class="container flex h-14 items-center justify-between px-4">
      <div class="flex items-center space-x-2">
        <div
          class="h-8 w-8 rounded-lg gradient-primary flex items-center justify-center"
        >
          <span class="text-white font-bold text-sm">C</span>
        </div>
        <span class="font-semibold text-lg">1$</span>
      </div>

      <Button
        v-if="!isConnected"
        @click="handleConnect"
        size="sm"
        class="gradient-primary text-white"
      >
        <Wallet class="mr-2 h-4 w-4 text-white"  />
        Connect Wallet
      </Button>

      <Sheet v-if="isConnected">
        <SheetTrigger asChild>
          <Button variant="ghost" size="sm" class="h-9 px-3">
            <div class="flex items-center space-x-2">
              <div
                class="h-6 w-6 rounded-full bg-accent/20 flex items-center justify-center"
              >
                <User class="h-3 w-3" />
              </div>
              <span class="text-sm font-medium">{{ address }}</span>
              <ChevronDown class="h-3 w-3 opacity-50" />
            </div>
          </Button>
        </SheetTrigger>
        <SheetContent side="right" class="w-80">
          <div class="py-6">
            <div class="space-y-4">
              <div class="flex items-center space-x-3">
                <div
                  class="h-12 w-12 rounded-full bg-accent/20 flex items-center justify-center"
                >
                  <User class="h-6 w-6" />
                </div>
                <div>
                  <p class="font-medium">{{ address }}</p>
                  <Badge variant="secondary" class="text-xs"> Ethereum </Badge>
                </div>
              </div>

              <div class="rounded-xl border p-4 space-y-2">
                <p class="text-sm text-muted-foreground">Balance</p>
                <p class="text-2xl font-bold">{{ balance }} ETH</p>
                <p class="text-sm text-muted-foreground">≈ $2,456.78 USD</p>
              </div>

              <div class="space-y-2">
                <Button variant="outline" class="w-full justify-start">
                  <User class="mr-2 h-4 w-4" />
                  View Profile
                </Button>
                <Button variant="outline" class="w-full justify-start">
                  Copy Address
                </Button>
                <Button
                  variant="outline"
                  class="w-full justify-start text-destructive"
                >
                  Disconnect
                </Button>
              </div>
            </div>
          </div>
        </SheetContent>
      </Sheet>
    </div>
  </header>
</template>
