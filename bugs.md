```
<script>
  import PaginationMixin from '@/Snippets/PaginationMixin';
  export default {
    mixins: [
      PaginationMixin
    ],
    props: ['results'],
    data(){
      return {
        users: [],
      };
    },

    methods: {
      dateFormat(dateStr) {
          let date = new Date(dateStr);
          return date.toLocaleDateString('ru-RU');
        },
    },

    async mounted() {
      this.setupPagination(this.results)    }
}
</script>


<template>

<div class="container">

  <h2>Results</h2>
  <ul class="responsive-table">
    <li class="table-header text-black bg-rose-300">
      <div class="col col-1">id</div>
      <div class="col col-2">test name</div>
      <div class="col col-2">user name</div>
      <div class="col col-3">score</div>
      <div class="col col-3">date</div>
    </li>


      <li class="table-row" v-for="result in items">
        <div class="col col-1" data-label="id">{{result.result.id}}.</div>
        <div class="col col-2" data-label="question name">{{result.result.test.name}}</div>

        <div class="col col-2" v-if="result.result.user" data-label="question name">
          {{result.result.user.name}}</div>


        <div class="col col-2" data-label="question name">{{ result.correctAnswers }} /
        {{ result.total }} — {{result.correctPercentage }}%</div>
        <div class="col col-2" data-label="question name">{{ dateFormat(result.result.updated_at) }}</div>
      </li>


       <!-- Pagination-->
        <div class="flex items-center space-x-1 mb-10">
          <button @click="pageChangeHandler(page - 1)" class="flex items-center px-4 py-2 text-gray-500 bg-gray-300 rounded-md">
              &lt;—
          </button>

          <div v-for="page in pageCount">
          <vue-ads-pagination
            :total-items="pageCount.length"
            :max-visible-pages="5"
            :page="page"
        >
          <button :class = "['pagination-button px-4 py-2 text-gray-700 bg-gray-200 rounded-md hover:bg-gray-200 hover:text-white', page == this.getPage() ? 'active' : '' ]" @click="pageChangeHandler(page)">
            {{page}}
          </button>

        </vue-ads-pagination>

          </div>

          <button @click="pageChangeHandler(page + 1)" class="px-4 py-2 font-bold text-gray-500 bg-gray-300 rounded-md hover:bg-gray-200 hover:text-white">
              —&gt;
          </button>

        </div>
      <!-- Pagination -->

  </ul>
</div>

</template>

<style lang="scss">
  body {
  font-family: "lato", sans-serif;
}
.container {
  max-width: 1000px;
  margin-left: auto;
  margin-right: auto;
  padding-left: 10px;
  padding-right: 10px;
}

h2 {
  font-size: 26px;
  margin: 20px 0;
  text-align: center;
  small {
    font-size: 0.5em;
  }
}

.responsive-table {
  li {
    border-radius: 3px;
    padding: 25px 30px;
    display: flex;
    justify-content: space-between;
    margin-bottom: 25px;
  }
  .table-header {
    font-size: 14px;
    text-transform: uppercase;
    letter-spacing: 0.03em;
  }
  .table-row {
    background-color: #ffffff;
    box-shadow: 0px 0px 9px 0px rgba(0, 0, 0, 0.1);
  }
  .col-1 {
    flex-basis: 10%;
  }
  .col-2 {
    flex-basis: 40%;
  }
  .col-3 {
    flex-basis: 25%;
  }
  .col-4 {
    flex-basis: 25%;
  }

  @media all and (max-width: 767px) {
    .table-header {
      display: none;
    }
    .table-row {
    }
    li {
      display: block;
    }
    .col {
      flex-basis: 100%;
    }
    .col {
      display: flex;
      padding: 10px 0;
      &:before {
        color: #6c7a89;
        padding-right: 10px;
        content: attr(data-label);
        flex-basis: 50%;
        text-align: right;
      }
    }
  }
}


  .pagination-button{
    box-shadow: 0px 0px 9px 0px rgba(0, 0, 0, 0.1);
    &.active{
      background-color: #c0c0c0;
      cursor: auto;
    }

}
</style>
```
