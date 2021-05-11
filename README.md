# Other

> <a href="https://github.com/CB279/vue-3-cb-validate">validate</a>

> <a href="https://github.com/CB279/vue-3-cb-alert">alert</a>

> <a href="https://github.com/CB279/vue-3-cb-modal">modal</a>

> <a href="https://github.com/CB279/vue-3-cb-datepicker">datepicker</a>

> <a href="https://github.com/CB279/vue-3-cb-select">select</a>

> <a href="https://github.com/CB279/vue-3-cb-grid">grid</a>

> <a href="https://github.com/CB279/vue-3-cb-sidenav">sidenav</a>

## Development

npm install @vue-cb/paginate

## Config

```js
import paginate from "@vue-cb/paginate";

createApp(app).use(paginate);
```

## Usage

```html
<v-paginate v-model="state.paginate.page" :total="state.paginate.total" />
```

```js
const state = reactive({
    paginate: {
        page: 1,
        total: 15,
    },
});
```

## Props

| name  | value  | default |
| ----- | ------ | ------- |
| total | number | -       |
| map   | number | 5       |
| skip  | number | 1       |

## Slots

| name  |
| ----- |
| first |
| last  |

## 📑 License

[MIT License](./LICENSE)
