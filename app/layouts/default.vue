<script setup lang="ts">
import { Toaster } from "~/components/ui/sonner";
import Header from "./Header.vue";
import BottomNav from "./BottomNav.vue";
</script>

<template>
  <Toaster position="bottom-right" />
  <main>
    <Header></Header>
    <slot></slot>
    <BottomNav></BottomNav>
  </main>
</template>
