# Compositions API

## Why we have to use this?

Options API is just fine, but we might have two main limitations/issues when build bigger Vue apps.

- Code that **belongs together logically** is **split up** across multiple options (data, methods, computed)
    - data(), computed, methods, watchers -> reflected to the same data are split across these different options
    - A bit annoying to manage (change something in data -> change computed -> change methods -> change watcher 😢) ->
      Scroll a lots.
- Re-using logic across components can be tricky or cumbersome

## Compositions API

We will merge: data, computed, watcher, methods into **setup()**

### ref

`ref` create a value that we can **reference**.

**value** -> `reactive` value that **Vue** can find out when we change it, be able to `watch` it and is able to `update`
the template at the DOM when that value `changed`.

### reactive

as the same as `ref`, but doesn't have the `value` wrapper in Proxy Object.

### isRef, isReactive -> boolean

### toRefs

give it an object => automatically turn all property values into refs (`reactive` object is able to work)

```javascript
const user = reactive({
    name: 'VuPham',
    age: 20
})

const userRefs = toRefs(user)  // return an object
// any values in object is REF => reactivity

setTimeout(() => {
    user.name = ' ANOTHER'; // template will re-render
    user.age = 22222; // template will re-render
})

// so that we can
return {
    userName: userRefs.name, // reactivity
    userAge: userRefs.age // reactivity
}

```

## Computed

A function was created by VueJS engine. Computed under the hood is just a `ref` (but `read` only).

```javascript
    const fullName = computed(() => {
    return firstName + ' ' + lastName;
})
```

## Watcher

A function was created by VueJS engine.

```javascript
    const state = ref('');

watch(state, (newValue, oldValue) => {
    // function when state change
})

```

With watch function, we can gain more dependency (by passing an array)

```javascript
    const state = ref('');
const another = ref('');

watch([state, another], (newValue, oldValue) => {
    // newValue[0], newValue[1]
    // oldValue[0], oldValue[1] 
})

```

## Ref (this.$refs)

use like `ref`

```javascript

<template>
    <input ref={inputRef}/>
</template>

const inputRef = ref(null); // access inputRef.value.value
return {inputRef}
```

## Props

```javascript
export default {
    setup(props){
        // access props
    }
}
```

## Context

```javascript
<template>
    <slot v-slot="header"></slot>
    // access here
    <slot v-slot="footer"></slot>
</template>

export default {
    setup(props, context){
        // context
        // emit
        // attrs
        // slots -> access slots above
    }
}
```

## Provide / Inject

```javascript
import {provide, inject} from 'vue';

export default {
    setup(){
        const user = ref('asdasd');

        provide('userName', user)

        // other component
        const name = inject('userName')
        name.value = 'sdad'; // can be but not do it
        // should change the value PROVIDE from it PROVIDE
    }
}
```

## Components Lifecycle

```md
beforeCreate, created --> Not Needed (`setup` replaces this)

beforeMount, mounted --> onBeforeMount, onMounted

beforeUpdate, updated --> onBeforeUpdate, onUpdated

beforeUnmount, unmounted --> onBeforeUnmount, onUnmounted
```

```javascript

import {onBeforeUnmount} from 'vue';

export default {
    setUp(){
        onBeforeUnmount(() => {

        })
    }
}

```

## Routing

use Hooks (Composable)

```javascript
import {useRoute, useRouter} from 'vue-router';

const route = useRoute();
// fullPath, hash, matched, meta, name, path, params, query

const router = useRouter();
// 
```

## Vuex

```javascript
import {useStore} from 'vuex'

const store = useStore();
// dispatch, commit, getters
```

## Mixins

We have `AddUser` and `DeleteUser` use the same logic => use Mixins to reuse that code.

> ⚠ : Components can not be shared through Mixins

We can merge mixins with original data

```javascript
import alertMixin from "@/mixins/alert";

export default {
    components: {
        UserAlert,
    },
    data(){
        title: 'Work fine'
    },
    mixins: [alertMixin] // automatically merge with the options in component
    // the option in component will override mixin
}
```

### Global Mixins

Logging for analytis functionality, to know when a component was mounted.

```javascript
// globalMixins.js
export default {
    mounted(){
        // some logic
    }
}


// Vue.app
import globalMixin from './mixins/globalMixins.js';

app.mixin(globalMixinl)

```

### Mixins Disadvantages

Bigger Apps -> Many `mixins` in 1 component -> `showAlert` -> belong to what mixins ?????????

> Harder to manage

## Custom Hooks
