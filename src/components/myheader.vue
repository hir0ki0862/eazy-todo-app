<template>
  <div>
    <header class="header">
      <h1 class="header__title">Eazy TODO</h1>
    </header>
  </div>
</template>

<script>
export default {
  name: 'myheader'
};
</script>

<style scoped>
.header {
  width: 100vw;
  height: 100px;
  background-color: #cb862d;
}

.header__title {
  font-size: 2.5rem;
  text-align: center;
  text-shadow: #666666 2px 2px 3px;
  font-family: 'Pacifico',cursive;
  line-height: 100px;
}
</style>
