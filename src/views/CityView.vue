<template>
  <div>
    <Suspense>
      <AsyncView />
      <!-- loading state via #fallback slot -->
      <template #fallback> Loading... </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncView from "@/components/AsyncView.vue";
</script>
