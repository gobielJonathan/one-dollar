<script setup lang="ts">
defineProps<{
  message: string;
  action: string;
}>();
</script>

<template>
  <div class="text-center py-12 space-y-4">
    <div
      class="w-16 h-16 mx-auto rounded-full bg-muted flex items-center justify-center"
    >
      <Trophy class="h-8 w-8 text-muted-foreground" />
    </div>
    <div class="space-y-2">
      <h3 class="font-medium">{{ message }}</h3>
      <p class="text-sm text-muted-foreground">{{ action }}</p>
    </div>
    <Button variant="outline" class="mt-4"> Explore Events </Button>
  </div>
</template>
