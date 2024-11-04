<!-- <script setup>
</script> -->

<template>
  <div id="app">
    <h1>TodoApp</h1>
    <div>
      <p>タイトル</p>
      <input type="text" v-model="newTodo" placeholder="タイトルを入力してください" />
      <p>詳細</p>
      <input type="text" v-model="detail" placeholder="詳細を入力してください" />
      <p>期限日</p>
      <input type="text" v-model="deadLine" placeholder="期限を入力(例:12/01)"/>
    </div>
    <div>
      <button @click="addTodo">追加する</button>
    </div>

    <p>todo一覧</p>
    <!-- <button @click="sortTodosById">IDで並び替え</button> -->
    <!-- <button @click="sortTodosByDeadLine">期限日で並び替え</button>
    <button @clicl="sortTodosByStatus">ステータスで並び替え</button> -->

      <div v-for="(todo, index) in todos" :key="todo.id">
        <p>---------------------------------------------------------------------------</p>
        <!-- todoの内容と期限 -->
        <span v-if="editIndex !== index">
          {{ todo.id }} - {{ todo.text }} - {{ todo.detail }} (期限: {{ todo.deadLine }})
        </span>

        <!-- 編集モード -->
        <input v-else type="text" v-model="editedTodo" />

        <!-- ステータス表示と変更ボタン -->
        <p>ステータス: {{ todo.status }}</p>
        <button @click="changeStatus(index, '未完了')">未完了</button>
        <button @click="changeStatus(index, '着手中')">着手中</button>
        <button @click="changeStatus(index, '完了')">完了</button>

        <!-- 編集と保存ボタン -->
        <button v-if="editIndex !== index" @click="startEdit(index)">編集する</button>
        <button v-if="editIndex === index" @click="saveEdit(index)">保存する</button>

        <!-- 削除ボタン -->
        <button @click="deleteTodo(index)">削除する</button>
      </div>
  </div>
</template>

<script>
  // import { v4 as uuidv4 } from 'uuid';

  export default {
    data() {
      return {
        newTodo: '', // 新規入力のTodoの入力内容
        detail: '',
        todos: [], // todoのリスト
        editIndex: null, // 編集の対象のindex
        editTodo: '', //編集中のTodoの内容
        deadLine: '', // 新規入力の期限日の入力内容
        nextId: 1 // 次に割り当てるID()
      }
    },
    methods: {
      // 追加
      addTodo() {
        if (this.newTodo.trim() !== '' && this.deadLine.trim() !== '') { // 入力が空白のみでないことを確認
          const newTodoItem = {
            // id: uuidv4(), //UUIDを生成してidとして追加
            id: this.nextId, //次のIDの割り当て
            text: this.newTodo, // Todoの内容をtextとして追加
            detail: this.detail,
            deadLine: this.deadLine, // 期限日
            status: '未完了' // 初期ステータスを「未完了」にする
          };
          this.todos.push(newTodoItem); // リストに追加
          this.nextId += 1; //次のTodo用にIDをインクリメント
          this.newTodo = ''; // 入力フィールドをリセット
          this.deadLine = ''; // 期限日をリセット
          this.detail = ''; // 詳細をリセット
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
        this.editedTodo = this.todos[index].text; // 現在の内容を編集用に設定
      },

      // 編集内容を保存
      saveEdit(index) {
        if (this.editedTodo.trim() !== '') { //編集内容が空でないことを確認
          this.todos[index].text = this.editedTodo; // 編集した内容で更新
        }
        this.cancelEdit();
      },

      // 編集をキャンセルする、リセットする
      cancelEdit() {
        this.editIndex = null;
        this.editedTodo = '';
      },

      // ステータスを変更する
      changeStatus(index, newStatus) {
        this.todos[index].status = newStatus;
      },

      // //IDで並び替え
      // sortTodosById() {
      //   this.todos.sort((a,b) => a.id - b.id)
      // }
    }
  }
</script>

<style scoped>
</style>
