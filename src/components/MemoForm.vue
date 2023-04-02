<template>
  <div class="memo_form_wrapper">
    <form v-if="editingMemo.isEditing" @submit.prevent="saveMemos">
      <textarea
        ref="textAreaEditingMemo"
        cols="40"
        rows="10"
        v-model="editingMemo.content"
      ></textarea>
      <div>
        <button class="save_button">保存</button>
        <button class="delete_button" v-on:click="deleteMemo">削除</button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  name: 'MemoForm',
  props: {
    editStateMemo: { type: Object, required: true },
    currentMemos: { type: Array, required: true }
  },
  data() {
    return {
      editingMemo: this.editStateMemo,
      memos: this.currentMemos
    }
  },
  methods: {
    deleteMemo() {
      if (window.confirm("削除してもよろしいでしょうか？")) {
        this.memos = this.memos.filter((memo) => memo.id !== this.editingMemo.id)
        this.saveMemosToLocalStorage()
        location.reload()
        this.editingMemo.content = ''
        this.editingMemo.isEditing = false
      }
    },
    saveMemosToLocalStorage() {
      localStorage.setItem('vue-memoapp-spa', JSON.stringify(this.memos))
    },
    saveMemos() {
      if (this.editingMemo.content === '') return
      const editingMemo = this.memos.find((memo) => memo.id === this.editingMemo.id)
      this.createOrFindByMemo(editingMemo)
      this.saveMemosToLocalStorage()
      this.editingMemo.content = ''
      this.editingMemo.isEditing = false
    },
    createOrFindByMemo(editingMemo) {
      if (editingMemo) {
        editingMemo.content = this.editingMemo.content
        editingMemo.isEditing = false
      } else {
        const newMemo = { ...this.editingMemo }
        newMemo.isEditing = false
        this.memos.push(newMemo)
      }
    }
  }
}
</script>
<style scoped>
textarea {
  background-color: blanchedalmond;
  border: 3px solid;
  font-size: 20px;
}

button {
  width: 100px;
  height: 30px;
  cursor: pointer;
  font-weight: bold;
}

.memo_form_wrapper {
  margin-top: 20px;
  margin-left: 100px;
}

.save_button {
  margin-right: 10px;
  background-color: #e5c6a9;
}

.delete_button {
  background-color: #e19191;
}
</style>
