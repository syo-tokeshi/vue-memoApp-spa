<script>
import { v4 as uuidv4 } from 'uuid'
import MemoForm from '@/components/MemoForm.vue'

export default {
  components: { MemoForm },
  data() {
    return {
      memos: JSON.parse(localStorage.getItem('vue-memoapp-spa') || '[]'),
      editingMemo: { id: '', content: '', isEditing: false }
    }
  },
  methods: {
    createMemo() {
      this.editingMemo.id = uuidv4()
      this.editingMemo.content = ''
      this.editingMemo.isEditing = true
    },
    editMemo(memo) {
      this.setUpMemosEditState(memo)
      this.editingMemo.id = memo.id
      this.editingMemo.content = memo.content
      this.editingMemo.isEditing = true
    },
    setUpMemosEditState(memo) {
      this.memos.filter((memo) => (memo.isEditing = false))
      memo.isEditing = true
    },
    memoTitle(memo) {
      return memo.split(/\n/)[0]
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
    <MemoForm :currentMemos="memos" :editStateMemo="editingMemo" />
  </div>
</template>

<style scoped>
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
