# Vue Bulma Pagination

[![npm version](https://badge.fury.io/js/vue-2-bulma-pagination.svg)](https://badge.fury.io/js/vue-2-bulma-pagination)

> A Vue.js pagination component for the Bulma CSS framework

View the [demo](https://rosendin.github.io/vue-bulma-pagination/).

## Installation

Install via NPM:

``` bash
npm install vue-2-bulma-pagination --save
```

## Usage

**Props**

|Name|Type|Required|Default|Description|
|----|----|--------|-------|-----------|
|current|Number|True|N/A|Current page|
|total|Number|False|0|Total number of items|
|itemsPerPage|Number|True|N/A|Items per page|
|step|Number|False|3|Number of pages to display (besides first and last)|
|onChange|Function|True|N/A|Page changed event callback. Parameters: page|

**Example**

Use like below. See the [example code](https://github.com/roseware/vue-bulma-pagination/blob/master/src/Demo.vue) in the demo.

``` html
<template>

  <table class="table">
    <thead>
      <tr>
        <th v-for="(key, value) in countries[0]">{{ key }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="country in countries">
        <td v-for="key in keys">{{ country[key] }}</td>
      </tr>
    </tbody>
  </table>

  <pagination
    :current="current"
    :total="total"
    :itemsPerPage="itemsPerPage"
    :onChange="onChange">
  </pagination>

</template>

<script>
import Pagination from 'vue-2-bulma-pagination'
import axios from 'axios';

let pagination = {
  current: 1,       // Current page
  total: 0,         // Items total count
  itemsPerPage: 5   // Items per page
}

export default {
  name: 'demo',
  components: { Pagination },
  data () {
    return {
      countries: [],
      pagination: pagination
    }
  },
  methods: {
    onChange (page) {
      console.log(`Getting page ${page}`)
      axios.get(`https://api.openaq.org/v1/countries?limit=5&page=${page}`)
      .then(response => {
        this.countries = response.data.results
        this.pagination.current  = page
      })
    }
  },
  created () {
    axios.get('https://api.openaq.org/v1/countries?limit=5')
    .then(response => {
      this.keys = Object.keys(response.data.results[0])
      this.countries = response.data.results

      // Set pagination config based on response
      this.pagination.total = response.data.meta.found
    })
  }
}
</script>
```

## Development

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
