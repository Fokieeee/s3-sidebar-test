<script setup>
import PageList from "./PageList.vue"

const props = defineProps(["pageList"])
</script>

<template>
  <div class="side-bar">
    <PageList
      :pageList="props.pageList.pages"
      :childPageKeys="props.pageList.rootLevelKeys"
    />
  </div>
</template>

<style scoped>
.side-bar {
  max-width: 300px;
  border: 1px solid var(--border-color);
  padding: 0.5rem;
}
</style>
