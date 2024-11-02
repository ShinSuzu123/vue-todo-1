<!-- <script setup>
</script> -->

<template>
<head>
  <title>TodoApp</title>
</head>

<body>
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
        {{ todo }}
        <button @click="deleteTodo(index)">削除する</button></li>
    </ul>
  </div>
</body>
</template>

<script>
  export default {
    data() {
      return {
        newTodo: '', // 入力された新しいアイテムの値を保持する
        todos: [] // todoのリスト
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim() !== '') { // 入力が空白のみでないことをチェック
          // リストに追加
          this.todos.push(this.newTodo);
          // 入力フィールドをリセット
          this.newTodo = '';
        }
      },
      deleteTodo(index) {
        // 指定された index の項目を削除
        this.todos.splice(index, 1);
      }
    }
  }
</script>

<style scoped>
</style>
