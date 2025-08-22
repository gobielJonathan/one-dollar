<script setup lang="ts">
import { useDropzone } from "vue3-dropzone";
import { ref } from "vue";
import { cn } from "~/lib/utils";
import { toTypedSchema } from "@vee-validate/zod";
import * as z from "zod";
import { Input } from "~/components/ui/input";
import { Button } from "~/components/ui/button";
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";
import { Calendar as CalendarComponent } from "@/components/ui/calendar";
import { Textarea } from "~/components/ui/textarea";
import { Card, CardContent, CardHeader, CardTitle } from "~/components/ui/card";
import {
  AccordionItem,
  Accordion,
  AccordionContent,
  AccordionTrigger,
} from "~/components/ui/accordion";
import { EventCard } from "~/components/ui/event-card";
import { Info, Upload } from "lucide-vue-next";
import {
  Sheet,
  SheetClose,
  SheetContent,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "~/components/ui/sheet";

const dropZoneRef = ref<HTMLDivElement>();

function onDrop(files: File[] | null) {
  // called when files are dropped on zone
}

const formSchema = toTypedSchema(
  z.object({
    username: z.string().min(2).max(50),
  })
);

const { getRootProps, getInputProps, isDragActive } = useDropzone({
  accept: [".jpeg", ".jpg", ".png", ".webp"],
  maxFiles: 6,
  onDrop: (acceptedFiles) => {},
});

const previewEvent = {
  id: "preview",
  title: "Your Event Name",
  description: "Event description will appear here...",
  imageUrl: "/placeholder.svg",
  seller: {
    address: "0x1234...5678",
    avatar: "/placeholder.svg",
    isVerified: true,
  },
  currentCount: 0,
  targetCount: 100,
  timeLeft: "7 days",
  price: 1,
};
</script>

<template>
  <div class="min-h-screen bg-background">
    <!-- {/* Header */} -->
    <header
      class="sticky top-0 z-40 border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60"
    >
      <div class="flex h-14 items-center justify-between px-4">
        <Button variant="ghost" size="icon">
          <ArrowLeft class="h-5 w-5" />
        </Button>
        <h1 class="text-lg font-semibold">Create Event</h1>
        <div class="w-10"></div>
      </div>
    </header>

    <div class="px-4 py-6 space-y-6 pb-20">
      <!-- {/* Form Card */} -->
      <Card class="rounded-2xl shadow-sm">
        <CardHeader>
          <CardTitle>Event Details</CardTitle>
        </CardHeader>
        <CardContent class="space-y-6">
          <!-- {/* Image Upload */} -->
          <div class="space-y-2">
            <Label>Images *</Label>
            <div
              v-bind="getRootProps()"
              :class="
                cn(
                  'border-2 border-dashed rounded-xl p-6 text-center cursor-pointer transition-colors',
                  isDragActive
                    ? 'border-primary bg-primary/5'
                    : 'border-border hover:border-primary/50'
                  //   errors.images && 'border-destructive'
                )
              "
            >
              <input v-bind="getInputProps()" />
              <Upload class="h-8 w-8 mx-auto mb-3 text-muted-foreground" />
              <p class="text-sm text-muted-foreground">
                {{
                  isDragActive ? "Drop images here..." : "Upload up to 6 images"
                }}
              </p>
              <p class="text-xs text-muted-foreground mt-1">
                JPG, PNG, WEBP up to 10MB each
              </p>
            </div>
            <!-- {errors.images && <p class="text-xs text-destructive">{errors.images}</p>} -->

            <!-- {/* Image Thumbnails */} -->
            <!-- {formData.images.length > 0 && (
                <div class="grid grid-cols-3 gap-2 mt-4">
                  {formData.images.map((file, index) => (
                    <div key={index} class="relative aspect-square">
                      <img
                        src={URL.createObjectURL(file)}
                        alt={`Upload ${index + 1}`}
                        class="w-full h-full object-cover rounded-lg"
                      />
                      <Button
                        variant="destructive"
                        size="icon"
                        class="absolute -top-2 -right-2 h-6 w-6 rounded-full"
                        onClick={() => removeImage(index)}
                      >
                        <X class="h-3 w-3" />
                      </Button>
                    </div>
                  ))}
                </div>
              )} -->
          </div>

          <!-- {/* Name */} -->
          <div class="space-y-2">
            <Label htmlFor="name">Name of Goods *</Label>
            <Input id="name" placeholder="e.g., Premium Coffee Beans" />
            <!-- {errors.name && <p class="text-xs text-destructive">{errors.name}</p>} -->
          </div>

          <!-- {/* Description */} -->
          <div class="space-y-2">
            <Label htmlFor="description">Description</Label>
            <Textarea
              id="description"
              value="{formData.description}"
              placeholder="Describe your product or service..."
              rows="3"
            ></Textarea>
          </div>

          <!-- {/* Price */} -->
          <div class="space-y-2">
            <Label htmlFor="price">Price of Goods</Label>
            <div class="relative">
              <span
                class="absolute left-3 top-1/2 -translate-y-1/2 text-sm text-muted-foreground"
                >$</span
              >
              <Input id="price" type="number" step="0.01" class="pl-8" />
            </div>
          </div>

          <!-- {/* Target */} -->
          <div class="space-y-2">
            <Label htmlFor="target">Target Coupons Sold *</Label>
            <Input
              id="target"
              type="number"
              value="{formData.target}"
              placeholder="e.g., 100"
            />
            <!-- {errors.target && <p class="text-xs text-destructive">{errors.target}</p>} -->
          </div>

          <!-- {/* Deadline */} -->
          <div class="space-y-2">
            <Label>Deadline Date *</Label>
            <Popover>
              <PopoverTrigger asChild>
                <Button
                  variant="outline"
                  :class="
                    cn(
                      'w-full justify-start text-left font-normal'
                      //   !formData.deadline && 'text-muted-foreground',
                      //   errors.deadline && 'border-destructive'
                    )
                  "
                >
                  <Calendar class="mr-2 h-4 w-4" />
                  <!-- {{formData.deadline ? format(formData.deadline, "PPP") : "Pick a date"}} -->
                </Button>
              </PopoverTrigger>
              <PopoverContent align="start" class="w-auto p-0">
                <CalendarComponent
                  initial-focus
                  class="p-3 pointer-events-auto"
                />
              </PopoverContent>
            </Popover>
            <!-- {errors.deadline && <p class="text-xs text-destructive">{errors.deadline}</p>} -->
          </div>

          <!-- {/* Payout Address */} -->
          <div class="space-y-2">
            <Label htmlFor="payout">Payout Address *</Label>
            <div class="flex gap-2">
              <Input
                id="payout"
                value="{formData.payoutAddress}"
                placeholder="0x..."
                :class="cn('flex-1')"
              />
              <Button variant="outline" size="icon" onClick="{useMyWallet}">
                <Wallet class="h-4 w-4" />
              </Button>
            </div>
            <!-- {errors.payoutAddress && <p class="text-xs text-destructive">{errors.payoutAddress}</p>} -->
          </div>
        </CardContent>
      </Card>

      <!-- {/* Rules Summary */} -->
      <Card class="rounded-2xl shadow-sm">
        <CardContent>
          <Accordion type="single" collapsible>
            <AccordionItem value="rules" class="border-0">
              <AccordionTrigger class="hover:no-underline py-0">
                <div class="flex items-center gap-2">
                  <Info class="h-4 w-4 text-primary" />
                  <span>Platform Rules</span>
                </div>
              </AccordionTrigger>
              <AccordionContent class="space-y-3 text-sm text-muted-foreground pt-4">
                <p>• Maximum 1 coupon per wallet address for fairness</p>
                <p>• Funds are held in escrow until target is reached</p>
                <p>• If target not met by deadline, automatic refunds issued</p>
                <p>• Winner selected via VRF (verifiable random function)</p>
                <p>• Winner must scan QR code for in-person redemption</p>
                <p>• You can only withdraw after winner approves redemption</p>
              </AccordionContent>
            </AccordionItem>
          </Accordion>
        </CardContent>
      </Card>

      <!-- {/* Actions */} -->
      <div class="flex gap-3">
        <Sheet>
          <SheetTrigger asChild>
            <Button variant="outline" class="flex-1 h-11"> Preview </Button>
          </SheetTrigger>
          <SheetContent side="bottom" class="h-[90vh]">
            <SheetHeader>
              <SheetTitle>Event Preview</SheetTitle>
            </SheetHeader>
            <div class="mt-6 mx-6">
              <EventCard
                :id="previewEvent.id"
                :title="previewEvent.title"
                :image="previewEvent.imageUrl"
                :target="previewEvent.targetCount"
                :joined="previewEvent.currentCount"
                :timeLeft="previewEvent.timeLeft"
                :price="previewEvent.price"
                :status="'active'"
                :category="'preview'"
                :seller="{ name: '', avatar: '', verified: false }"
              />
            </div>
          </SheetContent>
        </Sheet>

        <Button onClick="{handlePublish}" class="flex-1 h-11">
          Publish Event
        </Button>
      </div>

      <p class="text-xs text-muted-foreground text-center px-4">
        By publishing, you agree to the platform rules and terms of service.
      </p>
    </div>
  </div>
</template>
