<script setup>
import ArrowDown from "../assets/icons/ArrowDown.vue"
import ArrowRight from "../assets/icons/ArrowRight.vue"

import { computed, ref } from "vue"

const { pageList, childPageKeys } = defineProps(["pageList", "childPageKeys"])
const childrenOpenState = ref({})

for (const key of childPageKeys) {
  childrenOpenState.value[key] = false
}

const toggleChildren = (key) => {
  childrenOpenState.value[key] = !childrenOpenState.value[key]
}

const hasChildPages = computed(
  () => (key) => Boolean(pageList[key].childPageKeys)
)

const isChildPageOpened = computed(
  () => (key) => Boolean(childrenOpenState.value[key])
)
</script>

<template>
  <ul>
    <li
      v-for="key in childPageKeys"
      :key="key"
      @click="toggleChildren(key)"
      class="row"
    >
      <div
        class="text-container"
        :class="{ 'has-children': hasChildPages(key) }"
      >
        <div class="icon-container">
          <ArrowDown
            v-if="hasChildPages(key) && isChildPageOpened(key)"
            class="arrow-down"
          />
          <ArrowRight
            v-if="hasChildPages(key) && !isChildPageOpened(key)"
            class="arrow-right"
          />
        </div>
        <p class="text">{{ pageList[key].name }}</p>
      </div>

      <PageList
        @click.stop
        v-if="hasChildPages(key) && isChildPageOpened(key)"
        :pageList="pageList"
        :childPageKeys="pageList[key].childPageKeys"
      >
      </PageList>
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
