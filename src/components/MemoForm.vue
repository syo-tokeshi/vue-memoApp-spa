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
        <button type="button" class="delete_button" @click="deleteMemo">削除</button>
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
      if (window.confirm('削除してもよろしいでしょうか？')) {
        this.memos = this.memos.filter((memo) => memo.id !== this.editingMemo.id)
        this.$emit('toLocalStorage', this.memos)
        this.$emit('update', this.memos)
        this.editingMemo.content = ''
        this.editingMemo.isEditing = false
      }
    },
    saveMemos() {
      if (this.editingMemo.content === '') return
      const editingMemo = this.memos.find((memo) => memo.id === this.editingMemo.id)
      this.createOrUpdateMemo(editingMemo)
      this.$emit('toLocalStorage', this.memos)
      this.editingMemo.content = ''
      this.editingMemo.isEditing = false
    },
    createOrUpdateMemo(editingMemo) {
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

<style lang="scss" scoped>
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
