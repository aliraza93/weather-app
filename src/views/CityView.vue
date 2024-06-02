<template>
  <div>
    <Suspense>
      <AsyncView />
      <!-- loading state via #fallback slot -->
      <template #fallback> 
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncView from "@/components/AsyncView.vue";
import CityViewSkeleton from "@/components/CityViewSkeleton.vue"
</script>
