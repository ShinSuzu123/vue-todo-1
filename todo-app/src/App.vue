<!-- <script setup>
</script> -->

<template>
  <div id="app">
    <h1>TodoApp</h1>
    <h2>入力フォーム</h2>
    <div>
      <input type="text" v-model="newTodo" placeholder="todoを入力してください" />
    </div>
    <div>
      <button @click="addTodo">追加する</button>
    </div>

    <p>todo一覧</p>

    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <!-- 編集する際にフォームへ切り替え -->
        <span v-if="editIndex !== index">{{ todo }}</span>
        <input v-else type="text" v-model="editedTodo" />

        <button v-if="editIndex !== index" @click="startEdit(index)">編集する</button>
        <button v-if="editIndex === index" @click="saveEdit(index)">保存する</button>

        <button @click="deleteTodo(index)">削除する</button>
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        newTodo: '', // 新規入力のTodoの入力内容
        todos: [], // todoのリスト
        editIndex: null, // 編集の対象のindex
        editTodo: '', //編集中のTodoの内容
      }
    },
    methods: {
      // 追加
      addTodo() {
        if (this.newTodo.trim() !== '') { // 入力が空白のみでないことをチェック
          // リストに追加
          this.todos.push(this.newTodo);
          // 入力フィールドをリセット
          this.newTodo = '';
        }
      },

      // 削除
      deleteTodo(index) {
        // 指定された index の項目を削除
        this.todos.splice(index, 1);
      },

      // todoの編集に切り替え
      startEdit(index) {
        this.editIndex = index; // 編集対象のインデックスを設定
        this.editedTodo = this.todos[index]; // 現在の内容を編集用に設定
      },

      // 編集内容を保存
      saveEdit(index) {
        if (this.editedTodo.trim() !== '') {
          this.todos.splice(index, 1, this.editedTodo); // 編集した内容で更新
        }
        this.cancelEdit();
      },

      // 編集をキャンセルする、リセットする
      cancelEdit() {
        this.editIndex = null;
        this.editedTodo = '';
      }
    }
  }
</script>

<style scoped>
</style>
