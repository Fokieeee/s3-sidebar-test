<script setup>
import SideBar from "./components/SideBar.vue"
import axios from "axios"
import { onMounted, ref } from "vue"
import PreLoader from "./components/PreLoader.vue"

const isLoading = ref(true)
const isError = ref(false)
let pageList = null

const getpageList = async () => {
  try {
    const { data } = await axios.get(
      "https://prolegomenon.s3.amazonaws.com/contents.json"
    )
    pageList = data
  } catch (error) {
    console.error("Error fetching data:", error)
    isError.value = true
  } finally {
    isLoading.value = false
  }
}
onMounted(() => {
  getpageList()
})
</script>

<template>
  <PreLoader v-if="isLoading" />
  <SideBar v-else-if="!isError" :pageList="pageList" />

  <p v-if="isError" class="text">Ошибка!</p>
</template>

<style scoped>
.text {
  font: var(--font-xl);
  text-align: center;
  margin-top: 50vh;
}
</style>
