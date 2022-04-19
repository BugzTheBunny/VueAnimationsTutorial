<template>
  <div class="about">
    <h1>About</h1>
    <transition
      appear
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      >
      <div>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
      </div>
      </transition>
  </div>
</template>

<script>
import gsap from 'gsap'

export default {
  setup() {
    const beforeEnter = (element) => {
      console.log('beforeEnter - Set initial style', element)
      element.style.transform = 'translateY(-60px)'
      element.style.opacity = 0
    }

    const enter = (element,done) => {
      console.log('Starting to enter', element)
      gsap.to(element,{
        duration: 2,
        y:0,
        opacity:1,
        ease:'bounce.out',
        onComplete:done
      })
    }
    const afterEnter = () => {
      console.log('Waited for ending to start afterEnter...')
    }
    return {beforeEnter,enter,afterEnter}
  }
}
</script>

<style>
  .about {
    max-width: 600px;
    margin: 20px auto;
  }
</style>