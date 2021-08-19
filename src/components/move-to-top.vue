<template>
  <!--pageTopボタン-->
  <div class="pagetop-container">
    <transition name="fade">
      <img
        v-if="show"
        alt="page-top-button"
        class="page-top_button"
        src="../../public/img/page_top_button.png"
        @click="pageTop"
      />
    </transition>
  </div>
</template>

<script>
export default {
  name: 'moveTop',
  data() {
    return {
      show: false
    }
  },
  methods: {
    pageTop: function () {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    }
  },
  mounted() {
    window.addEventListener('scroll', () => {
      this.show = (window.scrollY > 150)
    })
  }
}
</script>

<style scoped>
.page-top_button {
  position: fixed;
  right: 15px;
  bottom: 20px;
  z-index: 1;
  width: 50px;
  height: auto;
  cursor: pointer;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease-in-out;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
