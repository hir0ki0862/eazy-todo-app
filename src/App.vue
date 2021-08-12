<template>
  <div id="app">
    <h1 class="animate__animated animate__slower animate__pulse animate__infinite">EazyTODO</h1>
    <h2>TODOを管理しよう</h2>
    <div class="wrapper">
      <!--todo追加部分-->
      <div class="title">
        <label for="title">Title</label>
        <input id="title" v-model="title" placeholder="TODOのタイトルを入力してください" type="text">
      </div>
      <div class="content">
        <label for="body">Content</label>
        <input id="body" v-model="body" placeholder="TODOの内容を入力してください" type="text">
      </div>
      <!--追加ボタン-->
      <div>
        <input class="add-todo" type="submit" value="Add" @click="addList">
      </div>

      <!--todo内容表示部分-->
      <div class="todo-wrapper">
        <p v-show="lists.length === 0" class="no-todo_title">Let's Add a TODO</p>
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
    <move-top></move-top>
  </div>
</template>

<script>
import moveTop from './components/move-to-top'

export default {
  name: 'App',
  components: {
    moveTop
  },
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
  width: 100vw;
  min-height: 100vh;
  font-family: 'My Font', 'Helvetica Neue', Arial, 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
  font-size: 16px;
  /*height: 100%;*/
  background-image: url('https://media.giphy.com/media/lkceXNDw4Agryfrwz8/source.gif');
  background-position: center;
  background-size: cover;
}

.wrapper {
  width: 80vw;
  max-width: 1080px;
  margin: 0 auto;
}

h1 {
  width: 80vw;
  padding: 50px 0 40px;
  margin: 0 auto;
  font-family: 'Press Start 2P', cursive;
  font-size: 4rem;
  color: #fff52e;
  text-align: center;
  text-shadow: 1px 1px 0 #ffffff, -1px 1px 0 #ffffff, 1px -1px 0 #ffffff, -1px -1px 0 #ffffff;
}

h2 {
  font-size: 2rem;
  font-weight: bold;
  color: #80ff29;
  text-align: center;
  text-shadow: 0 0 2px #fdfdfd;
}

h2::before {
  content: '『';
}

h2::after {
  content: '』';
}

p {
  padding: 15px 0;
  font-family: 'Press Start 2P', cursive;
  font-size: 1.5rem;
  color: #99ff2e;
  text-align: center;
}

label {
  display: block;
  padding-top: 1rem;
}

#title,
#body {
  width: 100%;
  color: #80ff29;
  text-indent: .5em;
  background-color: #000000;
  border: 2px solid #80ff29;
  border-radius: 5px;
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
  margin: 20px 0 15px;
  font-family: 'Press Start 2P', cursive;
  font-size: 2rem;
  color: #99ff2e;
  text-shadow: 0 0 3px #fdfdfd, 0 0 3px #fdfdfd;
}

.add-todo {
  display: block;
  padding: 5px 10px;
  margin: 30px auto 50px;
  font-family: 'Press Start 2P', cursive;
  font-size: 1.2rem;
  color: #575757;
  background-color: #fff52e;
  border-radius: 10px;
  box-shadow: 0 0 5px #fdfdfd, 0 0 5px #fdfdfd, 0 0 5px #fdfdfd;
}

.todo-wrapper {
  padding: 0 1rem;
  background-color: rgba(77, 77, 77, 0.7);
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
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  font-size: 1.2rem;
  line-height: 1.7;
  color: #99ff2e;
  word-break: break-word;
}

/*チェックボックス*/
.todo-checkbox {
  width: 1rem;
  height: 1rem;
  margin-right: 1rem;
  vertical-align: middle;
  border: 2px solid #fff52e;
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
  width: 80px;
  height: 20px;
  padding: 3px 5px;
  margin-top: 1rem;
  font-family: 'Press Start 2P', cursive;
  font-size: .5rem;
  color: #fdfdfd;
  text-align: center;
  cursor: pointer;
  background-color: #ff0062;
  border-radius: 5px;
  box-shadow: 0 0 3px #fdfdfd, 0 0 3px #fdfdfd, 0 0 3px #fdfdfd;
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
  display: block;
  padding: 5px 10px;
  margin: 0 auto;
  font-family: 'Press Start 2P', cursive;
  font-size: 1.2rem;
  color: #fdfdfd;
  text-align: center;
  cursor: pointer;
  background-color: #ff0000;
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
    padding: 2rem 0;
    font-size: 2rem;
  }

  h2 {
    font-size: 1.5rem;
  }

  .title label,
  .content label {
    margin: 1rem 0 10px;
    font-size: 1.2rem;
    text-shadow: 0 0 2px #fdfdfd, 0 0 2px #fdfdfd;
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
    font-family: 'My Font', sans-serif;
    font-size: 1rem;
  }

  .no-todo_title {
    font-family: 'My Font', sans-serif;
  }
}
</style>
