# my-project

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

# test_vue
Vue  typescript

to run :
```
git clone ...
cd ...

yarn install

yarn serve
```

# structure

```
App.vue
  |
  components/Table.vue
  |   |
  |   ButtonPage.vue
  |   Modal.vue
  |   Loading.vue
```

# features :

1. search
  flat JSON data for better search and dislay of the table
  display data arrocdin to the match search term

2. reverse column
  reverse might be confusing because pagination was added at the end to solve lag

3. pagination
  page to solve speed/slow
  pagination recalculate according to the result of the search

4. modal
  display info clicking on ℹ️


# to improve

1. pagination on a separated component
  this.$emit / $on

2. display data on cell
  could use a separated file and add other conditions for better design

