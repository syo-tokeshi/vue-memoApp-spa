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
    updateMemos(memos) {
      this.memos = memos
    },
    saveMemosToLocalStorage(memos) {
      localStorage.setItem('vue-memoapp-spa', JSON.stringify(memos))
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
  <h1 @click="resetMemosEditState()">
    <a href="#">My Vue App Spa!</a>
  </h1>
  <div class="container">
    <div>
      <ul class="memo_list" :class="{ center: !editingMemo.isEditing }">
        <li v-for="memo in memos" :key="memo.id">
          <a href="#" :class="{ select_memo: memo.isEditing }" @click="editMemo(memo)">
            {{ memoTitle(memo.content) }}
          </a>
        </li>
        <li>
          <a href="#" @click="createMemo()">➕</a>
        </li>
      </ul>
    </div>
    <MemoForm
      :currentMemos="memos"
      :editStateMemo="editingMemo"
      @update="updateMemos"
      @toLocalStorage="saveMemosToLocalStorage"
    />
  </div>
</template>

<style lang="scss" scoped>
h1 {
  text-align: center;
  a {
    color: #333333;
  }
}

li {
  margin-bottom: 5px;
  font-size: 20px;
}

li:last-child {
  list-style: none;
}

.container {
  display: flex;
  justify-content: center;
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
