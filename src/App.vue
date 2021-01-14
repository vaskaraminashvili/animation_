<template>
  <div class="container">
    <list-data></list-data>
  </div>
  <div class="container">
    <div class="block" :class="{ animate: animateBlock }"></div>
    <button @click="animateBlockFunc">Animate</button>
  </div>
  <div class="container">
    <transition
      leave-active-class=""
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      :css="false"
    >
      <p v-if="togglePara" class="para">this is onlu partly visible</p>
    </transition>
    <button @click="animateParagraph">Toggle paragraphe</button>
  </div>
  <div class="container">
    <transition>
      <p v-if="toggleMe">this is second paragra</p>
    </transition>
    <button @click="toggleParagraph">Toggle this</button>
  </div>
  <div class="container">
    <transition name="fade-button" mode="out-in">
      <button @click="showUsers" v-if="!usersAreVisible">Show users</button>
      <button @click="showUsers" v-else>Hide users</button>
    </transition>
  </div>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>
  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
import ListData from './components/ListData.vue';
export default {
  components: {
    ListData
  },
  data() {
    return {
      toggleMe: false,
      animateBlock: false,
      dialogIsVisible: false,
      togglePara: false,
      usersAreVisible: false
    };
  },
  methods: {
    toggleParagraph() {
      this.toggleMe = !this.toggleMe;
    },
    beforeEnter(el) {
      console.log('before enter');
      el.style.opacity = 0;
    },
    animateBlockFunc() {
      this.animateBlock = !this.animateBlock;
      console.log('asdsa');
    },
    enter(el, done) {
      console.log('enter');
      console.log(el);
      let round = 1;
      const inerval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(inerval);
          done();
        }
      }, 20);
      el.style.opacity = 0;
    },
    afterEnter() {
      console.log(' affter enter');
    },

    beforeLeave() {
      console.log('before leave');
    },
    leave() {
      console.log('leave');
    },
    afterLeave() {
      console.log('after leave');
    },
    showUsers() {
      this.usersAreVisible = !this.usersAreVisible;
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    animateParagraph() {
      this.togglePara = !this.togglePara;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
  transition: all 0.3s ease;
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
/* .para {
  height: 40px;
} */

.animate {
  background-color: red;
  transform: translate(-50px, 50px);
}

.fade-button-enter-from,
.fade-button-enter-to {
  opacity: 0;
}
.fade-button-enter-active,
.fade-button-enter-active {
  transition: opacity 0.3s ease-in-out;
}
.fade-button-enter-to,
.fade-button-leave-from {
  opacity: 1;
}
.v-enter-from {
  transform: translateY(-20px);
}
.v-enter-active {
  transition: all 0.5s ease-in-out;
}
.v-enter-to {
  transform: translateY(0px);
}
</style>
