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