# Compositions API

## Why we have use this?

Options API is just fine, but we might have two main limitations/issues when build bigger Vue apps.
- Code that **belongs together logically** is **split up** across multiple options (data, methods, computed)
    - data(), computed, methods, watchers -> reflected to the same data are split across these different options
    - A bit annoying to manage (change something in data -> change computed -> change methods -> change wathcer 😢) -> Scroll alots
- Re-using logic across components can be tricky or cumbersome

## Compositions API

We will merge: data, computed, watcher, methods into **setup()**

### ref

`ref` create a value that we can **reference**.

**value** -> `reactive` value that **Vue** can find out when we change it, be able to `watch` it and is able to `update` the template at the DOM when that value `changed`.

`ref` -> an object -> can access **value** through `.value`, but we just access variable into the template (no need to `.value`)


