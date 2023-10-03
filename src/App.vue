<script setup>
import SideBar from "./components/SideBar.vue"
import axios from "axios"
import { onMounted, ref } from "vue"
import PreLoader from "./components/PreLoader.vue"

const isLoading = ref(true)
const isError = ref(false)
const pageTree = ref([])

const createTree = (pagesObject, parentKey) => {
  const parent = pagesObject[parentKey]

  if (!parent) {
    return null
  }

  const tree = {
    key: parent.key,
    name: parent.name,
    level: parent.level,
    link: parent.link,
  }

  if (parent.childPageKeys) {
    tree.children = parent.childPageKeys.map((childKey) =>
      createTree(pagesObject, childKey)
    )
  }

  return tree
}

const getPageList = async () => {
  try {
    const { data } = await axios.get(
      "https://prolegomenon.s3.amazonaws.com/contents.json"
    )
    return data
  } catch (error) {
    console.error("Error fetching data:", error)
    isError.value = true
  } finally {
    isLoading.value = false
  }
}

onMounted(async () => {
  const pageList = await getPageList()

  pageTree.value = pageList.rootLevelKeys.map((key) =>
    createTree(pageList.pages, key)
  )
})
</script>

<template>
  <PreLoader v-if="isLoading" />
  <SideBar v-else-if="!isError" :pageTree="pageTree" />

  <p v-if="isError" class="text">Ошибка!</p>
</template>

<style scoped>
.text {
  font: var(--font-xl);
  text-align: center;
  margin-top: 50vh;
}
</style>
