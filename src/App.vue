<script>
import { v4 as uuidv4 } from 'uuid'
export default {
  data() {
    return {
      memos: JSON.parse(localStorage.getItem('vue-memoapp-spa') || '[]'),
      editingMemo: { id: '', content: '', isEditing: false }
    }
  },
  methods: {
    memoTitle(memo) {
      return memo.split(/\n/)[0]
    },
    editMemo(memo) {
      this.setUpMemosEditState(memo);
      this.editingMemo.id = memo.id
      this.editingMemo.content = memo.content
      this.editingMemo.isEditing = true
    },
    setUpMemosEditState(memo){
      this.memos.filter((memo) => (memo.isEditing = false))
      memo.isEditing = true
    },
    createMemo() {
      this.editingMemo.id = uuidv4()
      this.editingMemo.content = ''
      this.editingMemo.isEditing = true
    },
    deleteMemo() {
      this.memos = this.memos.filter((memo) => memo.id !== this.editingMemo.id)
      this.saveMemosToLocalStorage()
      this.editingMemo.content = ''
      this.editingMemo.isEditing = false
    },
    saveMemosToLocalStorage(){
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
    createOrFindByMemo(editingMemo){
      if (editingMemo) {
        editingMemo.content = this.editingMemo.content
        editingMemo.isEditing = false
      } else {
        const newMemo = { ...this.editingMemo }
        newMemo.isEditing = false
        this.memos.push(newMemo)
      }
    },
    resetMemosEditState() {
      this.memos.filter((memo) => (memo.isEditing = false))
      this.editingMemo.isEditing = false
    }
  },
  updated() {
    if (this.$refs.textAreaEditingMemo) {
      this.$refs.textAreaEditingMemo.focus()
    }
  }
}
</script>

<template>
  <h1 v-on:click="resetMemosEditState()"><a class="heading" href="#">My Vue App Spa!</a></h1>
  <div class="container">
    <div>
      <ul class="memo_list" v-bind:class="{ center: !editingMemo.isEditing }">
        <li class="memo" v-for="memo in memos" :key="memo.id">
          <a href="#" v-bind:class="{ select_memo: memo.isEditing }" v-on:click="editMemo(memo)">{{
            memoTitle(memo.content)
          }}</a>
        </li>
        <a class="create_memo_link" href="#" v-on:click="createMemo()">➕</a>
      </ul>
    </div>
    <div class="memo_form_wrapper">
      <form v-if="editingMemo.isEditing" @submit.prevent="saveMemos">
        <textarea ref="textAreaEditingMemo" cols="40" rows="10" v-model="editingMemo.content"></textarea>
        <div>
          <button class="save_button">保存</button>
          <button class="delete_button" v-on:click="deleteMemo(memo)">削除</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
body {
  background-color: antiquewhite;
}
textarea {
  background-color: blanchedalmond;
  border: 3px solid;
  font-size: 20px;
}
button{
  width: 100px;
  height: 30px;
  cursor: pointer;
  font-weight: bold;
}
h1 {
  text-align: center;
}
h1 > a {
  text-decoration: none;
  color: #333333;
}
li {
  font-size: 20px;
  margin-bottom: 5px;
}
.container {
  display: flex;
  justify-content: center;
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
.create_memo_link {
  text-decoration: none;
}
.memo_list.center {
  width: 200px;
  margin: auto;
  font-size: 20px;
}
.select_memo {
  color: blue;
}
</style>
