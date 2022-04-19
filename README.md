# Vue animations tutorial

[LINK](https://www.youtube.com/watch?v=RIApQjn9fvw&list=PL4cUxeGkcC9ghm7-iTfS9n468Kp7l9Ipu&ab_channel=TheNetNinja)

[starter project](https://github.com/iamshaunjp/vue-animations/tree/starter) -> It's self explaind if you know Vue3.

---
## **Transitions**
Vue has built-in transitions components.
It wraps around componenets, and applies CSS to the element it wraps.

```
<transition>
    <h1>Hello World!</h1>
</transition>
```
**There are total of 6 classes that can be used.**

- **On entering page:**

    - `.enter-from`  
        Applied to the element before it enters the browser, the initial CSS state.

    - `.enter-active`  
        Applied while the element is transition from `.enter-from` to `.enter-to`

    - `.enter-to`  
        Applied to the component as it enters the browser.

**_Example:_**
```
.enter-from{
    opacity:0;
}
.enter-active{
    transition:opacity 2s ease;
}
.enter-to{
    opacity:1;
}
```

-   **On leaving page:**

    - `.leave-from`  
        Applied to the element before it leaves the browser, the initial CSS state.

    - `.leave-active`  
        Applied while the element is transition from `.leave-from` to `.leave-to`

    - `.leave-to`  
        Applied to the component as it leaves the browser.

    **_Example:_**
    ```
    .leave-from{
        opacity:0;
    }
    .leave-active{
        transition:opacity 2s ease;
    }
    .leave-to{
        opacity:1;
    }
    ```

- **Additional:**  
You can also add a name prop to the transition and then Vue will append it to the transition class, making you able to have multiple transitions for a single page.

**_Example: this code will give you some classes, which you ca see under._**

```
<transition name="fade">
    <h1>Hello World!</h1>
</transition>
```
```
.fade-enter-from
.face-enter-active
.face-enter-to
.fade-leave-from
.face-leave-active
.face-leave-to
```
----
## Really doing something!

Working on the `Home.vue` component.  
We added a Div with an if statment that shows and hides on click of a button, and it has the effect of fade in and fade out, meaning the opacity changes before the component shows, and after.
```
<template>
    ....
    <transition name="fade">
      <div v-if="showHello">
        Hello World!
      </div>
    </transition>
    <button @click="toggleHello">toggle Hello!</button>
  </div>
</template>

<script>
....
  ...
  setup() {
    ...
    const showHello = ref(true)
    const toggleHello = () => {
      showHello.value = !showHello.value
    }
    ...
    return {
        ...
      showHello,
      toggleHello }
  }
}
</script>

<style>
  .fade-enter-from{
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
  }
</style>
```
----
**_Note:_** We can remove 
```
.fade-enter-to{
    opacity: 1;
}
```
and
```
.fade-leave-to{
    opacity: 0;
}
```

because its their default value, so in the specific case,they are not required.

Also, to save some code, we can just write the `leave-active & enter-active` in this scenerio like this:
```
.fade-enter-active,
.fade-leave-active{
    transition:all 2s ease;
  }
```
because the do the same thing

----
## The Toast (`Home.vue` again)  

---
**_Note:_** `<transition>` takes only one child component inside of it.

---

**component:**
```
<transition name="toast">
    <Toast v-if="showToast" />
</transition>
```

**animation:**

This is a dropdown animation from fade using Vue built in functions.

```
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
```

Now we can add an `keyframe` to it, like so:
```
...
.toast-enter-active {
  /* transition: all 0.3s ease; */
  animation: wobble 0.5s ease;
}
...
@keyframes wobble {
  0% {transform:translateY(-60px); opacity: 0}
  50% {transform:translateY(0px); opacity: 1}
  60% {transform:translateX(8px)}
  70% {transform:translateX(-8px)}
  80% {transform:translateX(4px)}
  90% {transform:translateX(-4px)}
  100% {transform:translateX(0px)}
}
```

This animation will make the toast to shake when it drops down to 0px on Y axis.

----
## Transition Group
In this part we see how to make transitions for a group of items, in this speific example, we will see the Todo list items.

inside `Todos.vue` replace:
```
<div v-if="todos.length">
  <ul>
    <li v-for="todo in todos" :key="todo.id" @click="deleteTodo(todo. id)">
      {{ todo.text }}
    </li>
  </ul>
</div>
```
with 
```
<transition-group tag="ul" name="list">
  <li v-for="todo in todos" :key="todo.id" @click="deleteTodo(todo.id)">
    {{ todo.text }}
  </li>
</transition-group>
```
----
*(Note that we set the `tag="ul"`, that's because Vue does not know what tag is initially set for the element, so we need to specify to it that we are using a `ul` here for the list), andw e called it `list`*

----
and lets add some animations to it:
```
/* list transitions */
  /* Enter transition */
  .list-enter-from {
    opacity: 0;
    transform: scale(0.6)
  }
  .list-enter-to {
    opacity: 1;
    transform: scale(1)
  }
  .list-enter-active {
    transition: all 0.4s ease;
  }
  /* Leave transition */
  .list-leave-from {
    opacity: 1;
    transform: scale(1)
  }
  .list-leave-to {
    opacity: 0;
    transform: scale(0.6)
  }
  .list-leave-active {
    transition: all 0.4s ease;
  }
```
this will make the todo items to appear and dissapear by scaling.

- Now, you can notice that initialy, the items just render without animation, to fix that, we just add `appear` to the transition group.
  ```
  <transition-group tag="ul" name="list" appear>
  ```
  Now they will also render with animation.

- Another thing, you may notice that when items are added to deleted, they are just jumping from top to buttom, without animation to it, they just appear under / above the other todo item, to fix that, `transition-group` provides us with another animation class, called `move`
  ```
  .list-move{
    transition: all 0.3s ease;
  }
  ```
  Now when you add items, you will see that all of the items below, are sliding down, instead of just jumping down.
- To fix the animation of delete, and make the items not to jump, we just add absolute position.
```
.list-leave-active {
  transition: all 0.4s ease;
  position: absolute;
}
```

----
## Transition between elemebets  
(In this specific example, how to show that there are no more todos left after there aren't any `if / else`).

first we wrap our `if / else` elements, in this case the list `todo items / message` with a transition, and we chose to name it `switch`
```
<transition name="switch">
  <div v-if="todos.length">
    <transition-group tag="ul" name="list" appear>
      <li v-for="todo in todos" :key="todo.id" @click="deleteTodo(todo.id)">
        {{ todo.text }}
      </li>
    </transition-group>
  </div>
  <div v-else>
    Woohoo, nothing left todo!
  </div>
</transition>
```
and add this animation
```
 /* switch transitions */ 
  .switch-enter-from,
  .switch-leave-to {
    opacity: 0;
    transform: translateY(20px)
  }
  .switch-enter-to,
  .switch-leave-from{
    opacity: 1;
    transform: translateY(0px)
  }
  .switch-enter-active,
  .switch-leave-active{
    transition: all 0.5s ease;
  }
```

It already looks better, but now the transition jumps, so for that we have `mode` attribute.
```
<transition name="switch" mode="out-in">
```

Now everything will work smoothly.

---
## Transition JavaScript Hooks

This hooks can be fired at different stages of a transition.

- Enter Hooks:
  - `before-enter`
  - `enter`
  - `after-enter`
- Leave Hooks:  
  - `before-leave`
  - `leave`
  - `after-leave`

You can call them like this:
```
<transition
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
>
  <h1>Hello World!</h1>
</transition>
```
The function that is fired generally should be called as the hook names, but it's not a must.

We will demonstrate it in the `About.vue`

let's wrap the text in the about page first:

```
<transition name="fade" appear>
  <div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Cum aperiam officia possimus delectus inventore quod quisquam culpa voluptas iusto, quae maiores quo dolorum, corporis laboriosam a dolore consequatur assumenda nam!</p>
  </div>
</transition>
```
and add animation to it.
```
  .fade-enter-from,
  .fade-leave-to{
    opacity: 0;
  }
  .fade-enter-active,
  .fade-leave-active{
    transition: opacity 3s ease;
  }
```

**Now lets use the `hooks`:**  
let's first add 3 hooks on the transition
```
<transition
  name="fade"
  appear
  @before-enter="beforeEnter"
  @enter="enter"
  @after-enter="afterEnter"
  >
```

then add the 3 functions inside the script.
```
<script>
export default {
  setup() {
    const beforeEnter = () => {
      console.log('beforeEnter')
    }

    const enter = () => {
      console.log('enter')
    }

    const afterEnter = () => {
      console.log('afterEnter')
    }

    return {beforeEnter,enter,afterEnter}
  }
}
</script>
```

Now you will see the logs in the console, the leave hooks work vice-versa.

----
## Using [GSAP](https://greensock.com/gsap/). (Professional-grade JavaScript animation for the modern web)

- GSAP is an animation library which works perfectly with the Vue transitions.

1. first of all, we install GSAP  
 `npm install gsap`  
2. import gsap (in this case to the `About.vue` page)
```
...
</template>

<script>
import gsap from 'gsap'

export default {
  setup() {
...
```
3. remove all of the previous animations, we don't need them anymore, and remove some other code we dont need:
```
<template>
  <div class="about">
    <h1>About</h1>
    <transition
      appear
      @before-enter="beforeEnter"
      @enter="enter"
      >
      <div>
        <p>Lorem ipsum...</p>
        <p>Lorem ipsum...</p>
        <p>Lorem ipsum...</p>
      </div>
      </transition>
  </div>
</template>

<script>
import gsap from 'gsap'

export default {
  setup() {
    const beforeEnter = () => {
      console.log('beforeEnter')
    }

    const enter = () => {
      console.log('enter')
    }

    return {beforeEnter,enter}
  }
}
</script>

<style>
  .about {
    max-width: 600px;
    margin: 20px auto;
  }
</style>
```
now, let's edit the functions,and add GSAP to it.
```
export default {
  setup() {
    const beforeEnter = (element) => {
      console.log('beforeEnter - Set initial style', element)
      element.style.transform = 'translateY(-60px)'
      element.style.opacity = 0
    }

    const enter = (element) => {
      console.log('Starting to enter', element)
      gsap.to(element,{
        duration: 1,
        y:0,
        opacity:1,
        ease:'bounce.out'
      })
    }
    return {beforeEnter,enter}
  }
}
```
the `element` is the element that the functions wraps around, meaning the element in the dom, which.
- on `beforeEnter` you can see that we set initial parameters of style for the element.
- on `enter` we use GSAP to transfare from one style to another, and GSAP gives up awesome animations built in like `bounce.out` which bounces the element in the end.

so now we see that the element is fading in, and jumping in the end, with MUCH less code and work.

but what if we want the `afterEnter`?
We can invoke it normally, but the problem with it, is that transition functions don't know by default when the animation is done, it just fires GSAP, and lets the animation run, so if we set the duration for example 5 seconds, we will see that the `afterEnter` will fire before the animation is done, to fix that we use `done`.

- add after enter:
  ```
  <transition
        appear
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter"
        >
  ```
- update functions:
  ```
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
          duration: 3,
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
  ```

---
## Stagger animations with GSAP

Let's animate the `Contact.vue` page, using GSAP, and make the boxes there appear one by one.

first change the `ul` to `transition-group`
```
<transition-group
  tag="ul"
  appear
  @before-enter="beforeEnter"
  @enter="enter"
  >
  <li v-for="icon in icons" :key="icon.name">
    <span class="material-icons">{{ icon.name }}</span>
    <div>{{ icon.text }}</div>
  </li>
</transition-group>
```

- add default animation:
```
<script>
import { ref } from 'vue'
import gsap from 'gsap'
export default {
  setup() {
    const icons = ref([
      { name: 'alternate_email', text: 'by email'},
      { name: 'local_phone', text: 'by phone'},
      { name: 'local_post_office', text: 'by post'},
      { name: 'local_fire_department', text: 'by smoke signal'},
    ])

    const beforeEnter = (element) => {
      element.style.opacity = 0
      element.style.transform = 'translateY(-50px)'
    }
    const enter = (element, done) => {
      gsap.to(element,{
        opacity: 1,
        y:0,
        duration:0.8,
        onComplete:done
      })
    }

    return { icons , beforeEnter, enter}
  }
}
</script>
```
after we add this, we can see that all of the cards are appearing in the same time.

we can add `delay`:
```
const enter = (element, done) => {
  gsap.to(element,{
    opacity: 1,
    y:0,
    duration:0.8,
    onComplete:done,
    delay:0.2
  })
}
```
but even now, they all get the delay, so we need to get the index of each item, and make each index to get i*0.2 delay seconds.
```
-> index * 0.2 = delay

1*0.2 = 0.2
2*0.2 = 0.4
and so on..
```
so each of them gets a different delay.

so first of all we need to get the index.
- Lets add indexing, and bind it as a data item.
```
<li v-for="(icon,index) in icons" :key="icon.name" :data-index="index">
        <span class="material-icons">{{ icon.name }}</span>
        <div>{{ icon.text }}</div>
      </li>
```
and then update the function to use the index from the dataset.
```
const enter = (element, done) => {
  gsap.to(element,{
    opacity: 1,
    y:0,
    duration:0.8,
    onComplete:done,
    delay:element.dataset.index *0.2
  })
}
```
----
## Route transition.

Right now there is no transition between routes / pages, and we want to add some, instead of the pages just showing poping up.

let's open `App.vue`

first we need to change the `router` to look like this:
```
<template>
  ....
  <router-view v-slot="{Component}">
    <transition name="route" mode="out-in">
      <component :is="Component"></component>
    </transition>
  </router-view>
</template>
```
by doing this, we are taking the `Component` that the router provides, and putting it into a `component`, which then renders it.
and then we are able to animate it.

now just add some styles:
```
/* route transitions */
.route-enter-from {
  opacity: 0;
  transform: translateX(100px);
}
.route-enter-active {
  transition: all 0.3s ease-out; 
}
.route-leave-to {
  opacity: 0;
  transform: translateX(-100px);
}
.route-leave-active {
  transition: all 0.3s ease-in; 
}
```

Now we have a transition between routs!
Wohooo!
Done with the course.