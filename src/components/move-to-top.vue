<template>
  <!--pageTopボタン-->
  <transition name="fade">
    <div v-if="show" class="pagetop-container">
      <img class="page-top_button" src="../../public/img/page_top_button.png" alt="page-top-button"
        @click="pageTop">
    </div>
  </transition>
</template>

<script>
export default {
  name: 'moveTop',
  data() {
    return {
      show: false
    };
  },
  methods: {
    pageTop: function () {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      });
    }
  },
  mounted() {
    let style = document.createElement('style');
    style.innerHTML = `
                    .fade-enter-active, .fade-leave-active {
                        transition: opacity .5s;
                    }
                    .fade-enter, .fade-leave-to {
                        opacity: 0;
                    }
                `;
    document.getElementsByTagName('head')[0].appendChild(style);
    window.addEventListener('scroll', () => {
      this.show = (window.scrollY > 150);
    });
  }
};
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

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
