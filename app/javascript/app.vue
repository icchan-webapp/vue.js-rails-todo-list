<template>
  <div class="container">
    <h2>TODO List</h2>
    <form v-on:submit.prevent>
      <input type="text" v-model="body">
      <button @click.prevent="createTodo">保存</button>
    </form>
    <ul>
      <li v-for="(todo, index) in todos" :key="todo.id">
        <input
          type="checkbox"
          v-on:change="doneChange(index, todo.id, todo.done)"
          v-model="todo.done"
        >
        <span :class="{ done: todo.done }">{{ todo.body }}</span>
        <button v-on:click="deleteTodo(index, todo.id)">削除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data: function () {
    return {
　　　 //todoリスト
      todos: [],
      //テキストボックスの値
      body: "",
    }
  },
  //一覧読み込み
  mounted () {
    axios.get('/api/v1/todos').then((response) => {
      this.todos = response.data
      }).catch(() => {
        alert("エラー");
      });
  },
  methods: {
    //Todo作成
    createTodo() {
      axios.post("/api/v1/todos", { body: this.body }).then((response) => {
        this.todos.unshift(response.data);
        this.body = "";
      }).catch(() => {
        alert("エラー");
      });
    },
    //checkボックスで発火する済んだかどうかのやつ
    doneChange(index, donechange_id, done_flg) {
      axios.put(`/api/v1/todos/${donechange_id}`, { done: done_flg }).then((response) => {
      }).catch(() => {
        alert("エラー");
      });
    },
    //Todo削除
    deleteTodo(index, delete_id) {
      axios.delete(`/api/v1/todos/${delete_id}`).then((response) => {
        this.todos.splice(index, 1);
      }).catch(() => {
        alert("エラー");
      });
    }
  }
}
</script>

<style lang="scss" scoped>
  ul {
    list-style: none;
    li {
      span.done {
        //チェックボックスで発火する取り消し線
        text-decoration: line-through;
      }
    }
  }
</style>