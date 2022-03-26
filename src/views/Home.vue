<template>
  <div class="home">
    <transition name="toast">
      <Toast v-if="showToast" />
    </transition>
    <Todos @badValue="triggerToast" />
    <transition name="fade">
      <div v-if="showHello">
        Hello World!
      </div>
    </transition>
    <button @click="toggleHello">toggle Hello!</button>
  </div>
</template>

<script>
import { ref } from 'vue'
import Toast from '../components/Toast'
import Todos from '../components/Todos'

export default {
  components: { Toast, Todos },
  setup() {
    const showToast = ref(false)
    const showHello = ref(true)

    const toggleHello = () => {
      showHello.value = !showHello.value
    }

    const triggerToast = () => {
      showToast.value = true;
      setTimeout(() => showToast.value = false, 3000)
    }

    return {
      showToast,
      triggerToast,
      showHello,
      toggleHello }
  }
}
</script>

<style>
  /* .fade-enter-from{
    opacity: 0;
  }
  .fade-enter-to{
    opacity: 1;
  }
  .fade-enter-active{
    transition:all 2s ease;
  }
  .fade-leave-from{
    opacity: 1;
  }
  .fade-leave-to{
    opacity: 0;
    
  }
  .fade-leave-active{
    transition:all 2s ease;
  } */

  .toast-enter-from {
    opacity: 0;
    transform: translateY(-60px);
  }
  .toast-enter-to {
    opacity: 1;
    transform: translateY(0px);
  }
  .toast-enter-active {
    transition: all 0.3s ease;
  }
  .toast-leave-from {
    transform: translateY(0px);
  }
  .toast-leave-to {
    transform: translateY(-60px);
  }
  .toast-leave-active {
    transition: all 0.3s ease;
  }
   
</style>