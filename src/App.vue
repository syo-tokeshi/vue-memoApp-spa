<script>
import { v4 as uuidv4 } from "uuid";
export default {
  data() {
    return {
      memos: JSON.parse(localStorage.getItem("vue-memoapp-spa") || "[]"),
      editingMemo: { id: '', content: "",isEditing: false }
    };
  },
  methods: {
    memoTitle(memo){
      return memo.split(/\n/)[0];
    },
    editMemo(memo){
      this.editingMemo.id = memo.id
      this.editingMemo.content = memo.content
      this.editingMemo.isEditing = true
    },
    createMemo(){
      this.editingMemo.id = uuidv4()
      this.editingMemo.content = ""
      this.editingMemo.isEditing = true
    },
    deleteMemo(){
      this.memos = this.memos.filter((memo) => memo.id !== this.editingMemo.id);
      localStorage.setItem("vue-memoapp-spa", JSON.stringify(this.memos));
      this.editingMemo.content = ''
      this.editingMemo.isEditing = false
    },
    saveMemos() {
      if (this.editingMemo.content === "") return;
      const memo = this.memos.find((memo) => memo.id === this.editingMemo.id);
      if (memo) {
        memo.content = this.editingMemo.content;
        memo.isEditing = false;
      } else {
        const newMemo = {...this.editingMemo}
        newMemo.isEditing = false;
        this.memos.push(newMemo);
      }
      localStorage.setItem("vue-memoapp-spa", JSON.stringify(this.memos));
      this.editingMemo.content = ''
      this.editingMemo.isEditing = false
    },
  },
};

</script>

<template>
  <div>
    <div>
      <ul>
        <li v-for="memo in memos" :key="memo.id">
          <a href="#" v-on:click="editMemo(memo)">{{ memoTitle(memo.content) }}</a>
        </li>
        <a href="#" v-on:click="createMemo()">X</a>
      </ul>
      <form v-if="editingMemo.isEditing" @submit.prevent="saveMemos">
        <textarea v-model="editingMemo.content"></textarea>
        <button>保存</button>
        <button  v-on:click="deleteMemo(memo)">削除</button>
      </form>
    </div>
  </div>
</template>

<style scoped>

</style>