<template>
  <!--todo追加部分-->
  <div class="stock__todo-area">
    <div v-for="(todo, index) in todos" :key="todo" class="todo__list-contents">
      <!--todo左側-->
      <div class="todo__list-text">
        <label class="label" v-if="!todo.isEditMode">
          <input type="checkbox" @click="saveCheckState(index);" v-model="todo.isCheck"
            class="checkbox" hidden>
          <span class="todo__list-item">{{ todo.item }}</span>
        </label>

        <input type="text" v-if="todo.isEditMode" class="input__todo-edit" :id="'editTextArea' + index">
      </div>
      <!--todo右側のボタン-->
      <div class="todo__list-btn">
        <div class="btn__left">
          <button @click="deleteItem(index);" class="btn delete" v-if="!todo.isEditMode">削除</button>
          <button @click="editItem(index, 'cancel');" v-if="todo.isEditMode" class="btn cancel">中止</button>
        </div>
        <div class="btn__right">
          <button v-if="!todo.isEditMode" @click="editItem(index);" class="btn">編集</button>
          <button v-if="todo.isEditMode" @click="editItem(index);" class="btn">確定</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todoarea'
};
</script>

<style scoped>
.stock__todo-area {
  border: solid 1px #000;
  margin-top: 10px;
}

.todo__list-contents {
  border-bottom: solid 1px #ddd;
  padding: 10px;
  display: flex;
}

.todo__list-contents .todo__list-text {
  width: 70%;
  word-break: break-all;
}

.todo__list-text .label {
  width: 100%;
  display: block;
}
</style>
