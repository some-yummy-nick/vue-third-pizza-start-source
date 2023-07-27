<template>
  <div class="app-layout">
    <app-header/>
    <slot/>
  </div>
</template>

<script setup>
import AppHeader from "@/layouts/AppHeader.vue";
</script>

