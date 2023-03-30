<script setup>
import { ref,reactive,onMounted } from "vue";
import MemoForm from "./components/MemoForm.vue";

const STORAGE_KEY = "vue-memoapp-spa";
const memos = ref(JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"));
const memo = reactive({
  content: "",
  isEditing: false,
})
</script>

<template>
  <div>neko</div>
</template>

<style scoped>

</style>