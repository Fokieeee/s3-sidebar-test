<script setup>
import PageList from "./PageList.vue"

const { pageTree } = defineProps(["pageTree"])
</script>

<template>
  <div class="side-bar">
    <PageList :pageTree="pageTree" />
  </div>
</template>

<style scoped>
.side-bar {
  max-width: 300px;
  border: 1px solid var(--border-color);
  padding: 0.5rem;
}
</style>
