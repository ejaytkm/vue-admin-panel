# vue-simple-table

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Features
- Pagination
- CSV Download
- Searchable by Keyword

### Configurations
Configuration object. More configurations will be add based on demand.
```
config: {
  filename: 'testingconfig' // csv fileaname | default : 'data'
}
```

Example of component declaration
```
<simple-table
  :inputdata="testingData"
  :configdata="config"
/>
```

<!-- ### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/). -->
