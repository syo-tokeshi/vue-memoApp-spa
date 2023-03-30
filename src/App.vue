<script setup>
import { ref,reactive,onMounted } from "vue";
import MemoForm from "./components/MemoForm.vue";
import { v4 as uuidv4 } from 'uuid';

const STORAGE_KEY = "vue-memoapp-spa";
const memos = ref(JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]"));
const memoContent = ref("")

const createMemo = () => {
  const memo = {
    id: uuidv4(),
    content: memoContent.value,
    isEditing: false,
  };
  memos.value.push(memo);
  saveMemos();
  memoContent.value = "";
}
const saveMemos = () => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(memos.value));
}
</script>

<template>
  <div>
    <div>
      <form @submit.prevent="createMemo">
        <textarea v-model="memoContent"></textarea>
        <button>新規作成</button>
      </form>
    </div>
    <div>
      <ul>
        <li v-for="memo in memos" :key="memo.id">
          <span>{{ memo.content }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>

</style>