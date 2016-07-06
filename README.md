# vue-bs-pagination

> A Vue.js pagination component with bootstrap3 pagination project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

```

## Get started

``` javascript
# import pagination.vue

    import pagination from './components/pagination'

    export default {
      data(){
        return {
          page:1,
          size:20,
          total:100,
          maxLink:5,
          eventName:'go'
        }
      },
      components: {
        pagination
      },
      computed:{
        items(){
          let arrays = [];
          for (let i = (this.page-1) * this.size;i<(this.page) * this.size;i++){
            arrays.push(i);
          }
          return arrays
        }
      },
      events: {
        'go': function (page) {
            console.log("page:"+page)
        }
      }
    }

# import <pagination></pagination>
  <pagination :page.sync="page" :page-size.sync="size" :total.sync="total" :max-link.sync="maxLink" :event-name="eventName"></pagination>


```
