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


```
   	<header>
  <!-- Navbar -->
  <nav class="navbar navbar-expand-lg shadow-md py-2 bg-white relative flex items-center w-full justify-between">
    <div class="px-6 w-full flex flex-wrap items-center justify-between">
      <div class="flex items-center">
        <button
          class="navbar-toggler border-0 py-3 lg:hidden leading-none text-xl bg-transparent text-gray-600 hover:text-gray-700 focus:text-gray-700 transition-shadow duration-150 ease-in-out mr-2"
          type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContentY"
          aria-controls="navbarSupportedContentY" aria-expanded="false" aria-label="Toggle navigation">
          <svg aria-hidden="true" focusable="false" data-prefix="fas" class="w-5" role="img"
            xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">
            <path fill="currentColor"
              d="M16 132h416c8.837 0 16-7.163 16-16V76c0-8.837-7.163-16-16-16H16C7.163 60 0 67.163 0 76v40c0 8.837 7.163 16 16 16zm0 160h416c8.837 0 16-7.163 16-16v-40c0-8.837-7.163-16-16-16H16c-8.837 0-16 7.163-16 16v40c0 8.837 7.163 16 16 16zm0 160h416c8.837 0 16-7.163 16-16v-40c0-8.837-7.163-16-16-16H16c-8.837 0-16 7.163-16 16v40c0 8.837 7.163 16 16 16z">
            </path>
          </svg>
        </button>
      </div>
      <div class="navbar-collapse collapse grow items-center" id="navbarSupportedContentY">
        <ul class="navbar-nav mr-auto lg:flex lg:flex-row">

        <div v-if="$page.props.user">
          <li class="nav-item">
            <a class="nav-link block pr-2 lg:px-2 py-2 text-black-600 hover:text-black-700 focus:text-black-700 transition duration-150 ease-in-out"
              data-mdb-ripple="true" data-mdb-ripple-color="light">Sneak Peek</a>
          </li>
          <li class="nav-item">
            <a class="nav-link block pr-2 lg:px-2 py-2 text-black-600 hover:text-black-700 focus:text-black-700 transition duration-150 ease-in-out"
               data-mdb-ripple="true" data-mdb-ripple-color="light">
              <Link class="ml-4"
        		:href="route('tests')" >Tests</Link>
   			</a>
          </li>
          <li class="nav-item">
          	 <div>{{ $page.props.user.name }}</div>
          </li>
         </div>

        <div v-else>
          <li class="nav-item">
            <a class="nav-link block pr-2 lg:px-2 py-2 text-gray-600 hover:text-gray-700 focus:text-gray-700 transition duration-150 ease-in-out"
              data-mdb-ripple="true" data-mdb-ripple-color="light">
              	<Link :href="route('login')">
	            	Log in
	        	</Link>
	    	</a>
          </li>
          <li class="nav-item mb-2 lg:mb-0">
            <a class="nav-link block pr-2 lg:px-2 py-2 text-gray-600 hover:text-gray-700 focus:text-gray-700 transition duration-150 ease-in-out"
              data-mdb-ripple="true" data-mdb-ripple-color="light">
              	<Link :href="route('register')" class="ml-4">
            		Register
        		</Link>
    		</a>
          </li>
      </div>

        </ul>
      </div>
    </div>
  </nav>
  <!-- Navbar -->
</header>
```
