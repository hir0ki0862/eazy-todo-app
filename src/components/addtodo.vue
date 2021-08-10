<template>
  <div class="main-wrapper">
    <main class="main">
      <!--TODO追加部分-->
      <div class="input__area">
        <input
          type="text"
          v-model="inputAdd"
          placeholder="入力してください"
          class="input__todo"
        />
        <button @click="addItem" class="input__btn">追加</button>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'addtodo'
};
</script>

<style scoped>
.main-wrapper {
  width: 80vw;
  margin: 20px auto;
}

.input__area {
  height: 40px;
  display: flex;
  justify-content: space-between;
}

.input__todo {
  width: 75%;
  height: 34px;
  display: inline-block;
  border: 1px solid #42b983;
}

.input__todo::placeholder {
  padding: 0 1rem;
  color: #fdfdfd;
}

.input__btn {
  width: 20%;
  height: 34px;
  display: inline-block;
  padding: 0 1em;
  text-decoration: none;
  background: #6bb72b;
  color: #ffffff;
  font-weight: bold;
  border-radius: 3px;
  cursor: pointer;
  outline: none;
  appearance: none;
  line-height: 34px;
  text-align: center;
}

.input__btn:active {
  border: none;
  -webkit-transform: translate(3px, 3px);
  transform: translate(3px, 3px);
}
</style>
