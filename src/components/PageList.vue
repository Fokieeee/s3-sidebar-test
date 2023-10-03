<script setup>
import ArrowDown from "../assets/icons/ArrowDown.vue"
import ArrowRight from "../assets/icons/ArrowRight.vue"

import { computed, ref } from "vue"

const { pageTree } = defineProps(["pageTree"])

const childrenOpenState = ref({})
for (const page of pageTree) {
  childrenOpenState.value[page.key] = false
}

const toggleChildren = (key) => {
  childrenOpenState.value[key] = !childrenOpenState.value[key]
}

const hasChildren = computed(
  () => (page) => Boolean(page.children && page.children.length > 0)
)

const hasChildrenOpen = computed(() => (key) => childrenOpenState.value[key])
</script>

<template>
  <ul>
    <li v-for="page in pageTree" :key="page.key" class="row">
      <div
        class="text-container"
        :class="{ 'has-children': hasChildren(page) }"
        @click="toggleChildren(page.key)"
      >
        <div class="icon-container">
          <ArrowDown
            v-if="hasChildren(page) && hasChildrenOpen(page.key)"
            class="arrow-down"
          />
          <ArrowRight
            v-if="hasChildren(page) && !hasChildrenOpen(page.key)"
            class="arrow-right"
          />
        </div>
        <p class="text">{{ page.name }}</p>
      </div>

      <PageList
        v-if="hasChildren(page) && hasChildrenOpen(page.key)"
        :pageTree="page.children"
      />
    </li>
  </ul>
</template>

<style scoped>
.has-children {
  cursor: pointer;
}

.has-children:hover {
  background: var(--bg-secondary-color);
  border-radius: 5px;
}
.row {
  list-style: none;
  margin-top: 10px;
  margin-left: 20px;
}

.text {
  margin-left: 10px;
}
.icon-container {
  width: 10px;
}
.arrow-down {
  width: 10px;
}

.arrow-right {
  width: 8px;
}

.text-container {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  padding: 0.5rem;
}
</style>
