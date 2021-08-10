<template>
  <div>
    <myheader></myheader>
    <addtodo></addtodo>
    <todoarea></todoarea>
  </div>
</template>

<script>
import myheader from "@/components/myheader";
import addtodo from "@/components/addtodo";
import todoarea from "@/components/todoarea";

export default {
  name: "App",
  components: {
    myheader,
    addtodo,
    todoarea,
  },
};
</script>
<style>
body {
  background-color: #7d77a1;
}
</style>
