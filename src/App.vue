<template>
  <div id="app">
    <h1>Eazy TODO</h1>
    <div class="wrapper">
      <!--todo追加部分-->
      <div class="title">
        <label for="title">Title</label>
        <input type="text" v-model="title" id="title" placeholder="タイトルを入力してください">
      </div>
      <div class="content">
        <label for="body">Content</label>
        <textarea v-model="body" id="body" placeholder="内容を入力してください"></textarea>
      </div>
      <!--追加ボタン-->
      <div>
        <input class="add-todo" type="submit" value="追加" @click="addList">
      </div>

      <!--todo内容表示部分-->
      <div class="todo-wrapper">
        <ul class="todo-list" v-for="(list, i) in lists" :key="i">
          <li id="checkbox" class="todo-list_item">
            <label class="check" :class="{done: list.isChecked}">
              <input
                type="checkbox"
                class="todo-checkbox"
                v-model="list.isChecked" @change="saveTodo"><span
              class="check-mark"></span>{{ list.title }}
              <br>{{ list.body }}</label>
            <button class="delete-todo" @click="deleteList(i)">削除</button>
          </li>
        </ul>
        <!--すべて削除するボタン-->
        <button class="all-delete_btn" @click="deleteAll">チェック済みのTODOを削除</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      lists: [],
      title: '',
      body: ''
    };
  },
  methods: {
    // todoの追加
    addList: function () {
      // 入力されていなければデータを追加しない
      if (this.title === '' || this.body === '') return;
      // 入力されたデータをlistsに追加する
      this.lists.push({title: this.title, body: this.body, isChecked: false});
      // 追加し終わったらフォームのvalueを空にする
      this.title = '';
      this.body = '';
      // ブラウザに保存
      this.saveTodo();
    },
    // todoの削除
    deleteList: function (i) {
      this.lists.splice(i, 1);
      this.saveTodo();
    },
    // チェック済みを全て削除
    deleteAll: function () {
      this.lists = this.lists.filter(function (list) {
        return list.isChecked === false;
      });
      this.saveTodo();
    },
    saveTodo: function () {
      localStorage.setItem('lists', JSON.stringify(this.lists));
    },
    loadTodo: function () {
      this.lists = JSON.parse(localStorage.getItem('lists'));
      if (!this.lists) {
        this.lists = [];
      }
    }
  },
  mounted() {
    this.loadTodo();
  }
};
</script>
<style>
#app {
  font-size: 16px;
  background-color: #ffb465;
  width: 100vw;
  height: 100vh;
  font-family: 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
}

.wrapper {
  width: 80vw;
  margin: 0 auto;
}

h1 {
  font-size: 3rem;
  text-align: center;
  padding: 15px 0;
}

label {
  display: block;
  padding-top: 1rem;
}

#title {
  width: 100%;
  background-color: #fdfdfd;
  color: #666666;
  padding: 0 .5rem;
}

#body {
  width: 100%;
  background-color: #fdfdfd;
  color: #666666;
  padding: 0 .5rem;
}

.add-todo {
  font-size: 1.2rem;
  display: block;
  margin: 1rem auto;
  padding: 5px 10px;
  border-radius: 10px;
  color: #fdfdfd;
  background-color: #42b983;
  box-shadow: 0 0 3px #666666;
}

.todo-wrapper {
  border: 2px solid #fdfdfd;
  border-radius: 10px;
  padding: 0 1rem;
}

.todo-list {
  font-size: 1.2rem;
  padding: 1rem 0 .5rem 0;
  margin-bottom: 1rem;
  border-bottom: 1px solid #666666;

}

.todo-list_item {
  line-height: 1.7;
  word-break: break-word;
}

/*チェックボックス*/
.todo-checkbox {
  width: 1rem;
  height: 1rem;
  border: 1px solid #fdfdfd;
  margin-right: 1rem;
}

/*label*/
.check {
  position: relative;
  cursor: pointer;
  user-select: none;
  width: fit-content;
}

/*チェックマーク*/
.check-mark::after {
  content: '';
  position: absolute;
  top: 1.2rem;
  left: 0.4rem;
  width: 10px;
  height: 18px;
  border: solid #dc322f;
  border-width: 0 4px 4px 0;
  transform: rotate(45deg);
  opacity: 0;
}

/*チェックされたとき*/
.check input:checked + .check-mark::after {
  opacity: 1;
}

/*削除ボタン*/
.delete-todo {
  display: block;
  font-size: .8rem;
  margin: 1rem auto 0;
  padding: 3px 5px;
  width: 80px;
  text-align: center;
  border-radius: 5px;
  color: #fdfdfd;
  background-color: #dc2f2f;
  box-shadow: 0 0 3px #666666;
}

/*チェック時に打ち消し線*/
.done {
  text-decoration: line-through 2px solid black;
  color: #8f8f8f;
}

.all-delete_btn {
  display: block;
  text-align: center;
  margin: 30px auto;
  border-radius: 5px;
  color: #fdfdfd;
  background-color: #ff0000;
  box-shadow: 0 0 3px #666666;
  padding: 5px 10px;
}
</style>
