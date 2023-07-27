<template>
  <div class="app-layout">
    <app-header />
    <div class="content">
      <app-layout-sidebar />
      <slot/>
    </div>
  </div>
</template>

<script setup>
import AppHeader from "@/layouts/AppHeader.vue";
import AppLayoutSidebar from './AppLayoutSidebar.vue'
</script>

