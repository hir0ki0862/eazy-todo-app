<template>
  <div id="app">
    <h1>Eazy TODO</h1>
    <h2>TODOを管理しよう</h2>
    <div class="wrapper">
      <!--todo追加部分-->
      <div class="title">
        <label for="title">Title</label>
        <input id="title" v-model="title" placeholder="TODOのタイトルを入力してください" type="text">
      </div>
      <div class="content">
        <label for="body">Content</label>
        <input type="text" id="body" v-model="body" placeholder="TODOの内容を入力してください">
      </div>
      <!--追加ボタン-->
      <div>
        <input class="add-todo" type="submit" value="Add" @click="addList">
      </div>

      <!--todo内容表示部分-->
      <div class="todo-wrapper">
        <p v-show="lists.length === 0">Let's Add a TODO</p>
        <ul v-for="(list, i) in lists" :key="i" class="todo-list">
          <li id="checkbox" class="todo-list_item">
            <label :class="{done: list.isChecked}" class="check">
              <input
                v-model="list.isChecked"
                class="todo-checkbox"
                type="checkbox" @change="saveTodo"><span
              class="check-mark"></span>{{ list.title }}
              <br>『{{ list.body }}』</label>
            <button class="delete-todo" @click="deleteList(i)">Delete</button>
          </li>
        </ul>
      </div>
      <!--すべて削除するボタン-->
      <div class="alldelete">
        <button v-if="lists.length !== 0" class="all-delete_btn" @click="deleteAll">Delete checked
          TODOs
        </button>
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
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap');

@font-face {
  font-family: 'My Font';
  src: url('~@/assets/fonts/PixelMplus12-Regular.ttf') format('truetype');
}

#app {
  background-image: url('https://media.giphy.com/media/lkceXNDw4Agryfrwz8/source.gif');
  background-size: cover;
  background-position: center;
  width: 100vw;
  /*height: 100%;*/
  font-family: 'My Font', 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
  font-size: 16px;
  min-height: 100vh;
}

.wrapper {
  width: 80vw;
  max-width: 1080px;
  margin: 0 auto;
}

h1 {
  padding: 50px 0 40px;
  font-size: 4rem;
  text-align: center;
  font-family: 'Press Start 2P', cursive;
  color: #fff52e;
  text-shadow: 1px 1px 0 #ffffff, -1px 1px 0 #ffffff, 1px -1px 0 #ffffff, -1px -1px 0 #ffffff;
}

h2 {
  font-size: 2rem;
  text-align: center;
  color: #80ff29;
  text-shadow: 0 0 2px #fdfdfd;
  font-weight: bold;
}

h2::before {
  content: '『';
}

h2::after {
  content: '』';
}

p {
  padding: 15px 0;
  font-size: 1.5rem;
  text-align: center;
  color: #99ff2e;
  font-family: 'Press Start 2P', cursive;
}

label {
  display: block;
  padding-top: 1rem;
}

#title,
#body {
  width: 100%;
  color: #80ff29;
  background-color: #000000;
  border: 2px solid #80ff29;
  border-radius: 5px;
  text-indent: .5em;
}

#title::placeholder,
#body::placeholder {
  color: #fdfdfd;
  background-color: #000000;
}

.content {
  margin-top: 30px;
}

.title,
.content {
  font-size: 1.1rem;
}

.title label,
.content label {
  font-size: 2rem;
  font-family: 'Press Start 2P', cursive;
  margin: 20px 0 15px;
  color: #99ff2e;
  text-shadow: 0 0 3px #fdfdfd, 0 0 3px #fdfdfd;
}

.add-todo {
  display: block;
  padding: 5px 10px;
  margin: 30px auto 50px;
  font-size: 1.2rem;
  color: #575757;
  background-color: #fff52e;
  border-radius: 10px;
  box-shadow: 0 0 5px #fdfdfd, 0 0 5px #fdfdfd, 0 0 5px #fdfdfd;
  font-family: 'Press Start 2P', cursive;
}

.todo-wrapper {
  background-color: rgba(77, 77, 77, 0.7);
  padding: 0 1rem;
  border: 7px solid #fff52e;
  border-radius: 10px;
}

.todo-list {
  padding: 1rem 0 .5rem 0;
  margin-bottom: 1rem;
  font-size: 1.2rem;
  border-bottom: 3px solid #fff52e;

}

.todo-list_item {
  font-size: 1.2rem;
  line-height: 1.7;
  color: #99ff2e;
  word-break: break-word;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-wrap: wrap;
}

/*チェックボックス*/
.todo-checkbox {
  width: 1rem;
  height: 1rem;
  margin-right: 1rem;
  border: 2px solid #fff52e;
  vertical-align: middle;
}

/*label*/
.check {
  position: relative;
  width: fit-content;
  cursor: pointer;
  user-select: none;
}

/*チェックマーク*/
.check-mark::after {
  position: absolute;
  top: .8rem;
  left: 0.2rem;
  width: 13px;
  height: 22px;
  content: '';
  border: solid #80ff29;
  border-width: 0 5px 5px 0;
  opacity: 0;
  transform: rotate(45deg);
}

/*チェックされたとき*/
.check input:checked + .check-mark::after {
  opacity: 1;
}

/*削除ボタン*/
.delete-todo {
  cursor: pointer;
  margin-top: 1rem;
  width: 80px;
  height: 20px;
  padding: 3px 5px;
  font-size: .5rem;
  color: #fdfdfd;
  text-align: center;
  background-color: #ff0062;
  border-radius: 5px;
  box-shadow: 0 0 3px #fdfdfd, 0 0 3px #fdfdfd, 0 0 3px #fdfdfd;
  font-family: 'Press Start 2P', cursive;
}

/*チェック時に打ち消し線*/
.done {
  color: #fdfdfd;
  text-decoration: line-through 2px solid #ffffff;
}

.alldelete {
  padding: 30px 0 50px;
}

.all-delete_btn {
  cursor: pointer;
  font-size: 1.2rem;
  display: block;
  padding: 5px 10px;
  margin: 0 auto;
  color: #fdfdfd;
  text-align: center;
  background-color: #ff0000;
  font-family: 'Press Start 2P', cursive;
  border-radius: 5px;
  box-shadow: 0 0 3px #fdfdfd, 0 0 3px #fdfdfd, 0 0 3px #fdfdfd;
}

@media (max-width: 599px) {
  #app {
    font-size: 14px;
  }

  .wrapper {
    width: 90vw;
  }

  h1 {
    font-size: 2rem;
    padding: 2rem 0;
  }

  h2 {
    font-size: 1.5rem;
  }

  .title label,
  .content label {
    font-size: 1.2rem;
    text-shadow: 0 0 2px #fdfdfd, 0 0 2px #fdfdfd;
    margin: 1rem 0 10px;
  }

  .content label {
    padding-top: 0;
  }

  #title,
  #body {
    font-size: 1rem;
  }

  .add-todo {
    margin: 1rem auto 1.5rem;
    font-size: 1rem;
  }

  .todo-wrapper {
    border: 5px solid #fff52e;
  }

  .delete-todo {
    font-size: 10px;
  }

  .check {
    padding-top: 0;
  }

  .check-mark::after {
    top: 0;
  }

  .alldelete {
    padding-top: 15px;
  }

  .all-delete_btn {
    font-size: 1rem;
    font-family: "My Font", sans-serif;
  }
}
</style>
