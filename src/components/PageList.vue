<script setup>
import ArrowDown from "../assets/icons/ArrowDown.vue"
import ArrowRight from "../assets/icons/ArrowRight.vue"

import { ref } from "vue"

const { pageList, childs } = defineProps(["pageList", "childs"])
const childrenOpenState = ref(null)

const initialChildrenState = {}
for (let child of childs) {
  initialChildrenState[child] = false
}
childrenOpenState.value = initialChildrenState

const toggleChildren = (key) => {
  childrenOpenState.value[key] = !childrenOpenState.value[key]
}
</script>

<template>
  <ul>
    <li
      v-for="key in childs"
      :key="key"
      @click="toggleChildren(key)"
      class="row"
    >
      <div
        class="text-container"
        :class="{ 'has-children': pageList[key].childPageKeys }"
      >
        <div class="icon-container">
          <ArrowDown
            v-if="pageList[key].childPageKeys && childrenOpenState[key]"
            class="arrow-down"
          />
          <ArrowRight
            v-if="pageList[key].childPageKeys && !childrenOpenState[key]"
            class="arrow-right"
          />
        </div>
        <p class="row-text">{{ pageList[key].name }}</p>
      </div>

      <PageList
        @click.stop
        v-if="pageList[key].childPageKeys && childrenOpenState[key]"
        :pageList="pageList"
        :childs="pageList[key].childPageKeys"
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

.row-text {
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
