```
<template>

  <div class="topping">
  <!-- <div class="topping sticky top-0"> -->

    <div class="container-fluid">
      <div class="header flex pt-3 pb-6 items-center justify-between">
        <!-- logo -->

        <!-- Mobile menu -->
        <slot name="mobile_menu"></slot>

        <a href="/" class="logo-wrapper hidden md:visible">
          <span class="logo">{{ logoText }}</span>
        </a>
        <!-- search -->
        <div class="search-block relative" @click="changewidth()">

          <input v-model="query" id="search-change" v-if="$root.sharedState.lang == 'kaz'" type="text"
          placeholder="Оқулық бойынша іздеу" class="search-input sm:w-full transition-all duration-1000" @blur="blur()"
          @keyup="search()"/>

          <div class="search-result absolute z-10 bg-white p-4 w-full rounded-b-lg mt-1" v-show="showme" id="showme">

            <main v-if="loading" class="flex flex-col align-center justify-center text-center">
              <loader></loader>
            </main>


            <ul v-else>
              <!-- for loop shows all the articles' titles as links-->
              <li v-for = "article in queryResults" class="bg-slate-400">
                <inertia-link :href="getArticleRoute(article)" >{{ article.title }}</inertia-link>
              </li>
            </ul>

          </div>

        </div>

        <!-- switch -->
        <Switch id="switch"></Switch>
        <!-- <Switch @switch="switchGraphics"></Switch> -->
        <div class="account-block mx-4" id="myprofile">

          <jet-dropdown v-if="$page.props.user" align="right" width="48">
            <template #trigger>
              <button v-if="$page.props.jetstream.managesProfilePhotos" class="flex text-sm border-2 border-transparent rounded-full focus:outline-none focus:border-gray-300 transition">
                <img class="h-8 w-8 rounded-full object-cover" :src="$page.props.user.profile_photo_url" :alt="$page.props.user.name" />
              </button>

              <span v-else class="inline-flex rounded-md">
                <button type="button" class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-gray-500 bg-white hover:text-gray-700 focus:outline-none transition">
                    {{ $page.props.user.name }}
                  <svg class="ml-2 -mr-0.5 h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                      <path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd" />
                  </svg>
                </button>
              </span>
            </template>

            <template #content>
              <!-- Account Management -->
              <div class="block px-4 py-2 text-xs text-gray-400">
                  Manage Account
              </div>

              <jet-dropdown-link :href="route('profile.show')">
                  {{ convert("Профиль") }}
              </jet-dropdown-link>

              <jet-dropdown-link :href="route('api-tokens.index')" v-if="$page.props.jetstream.hasApiFeatures">
                  API Tokens
              </jet-dropdown-link>

              <div class="border-t border-gray-100"></div>

              <!-- Authentication -->
              <form @submit.prevent="logout">
                  <jet-dropdown-link as="button">
                      {{ convert("Шыгу") }}
                  </jet-dropdown-link>
              </form>
            </template>
          </jet-dropdown>
          <template v-else>
            <jet-dropdown-link class="sm:" :href="route('profile.show')">
              {{ convert("Кіру") }}
            </jet-dropdown-link>
          </template>

        </div>
      </div>
    </div>
  </div>

  <main>
    <div class="container-fluid">
      <div class="subhead flex flex-wrap mr-0 justify-between mb-4">
        <div class="link-to-main">
          <inertia-link :href="route('main')" class="hover:text-gray-500 hover:underline"
          >{{ convert("Басты бет") }}</inertia-link>
        </div>
        <div class="link-to-additionals">
          <inertia-link :href="route('converter')" class="mx-2 nav-menu"
          >{{ convert("Конвертер") }}</inertia-link>
          <inertia-link :href="route('dictionary')" class="mx-2 nav-menu pr-5"
          >{{ convert("Сөздік") }}</inertia-link>
        </div>
      </div>
    </div>
    <slot></slot>
  </main>



</template>

<style lang="sass">
.nav-menu:hover
  color: #9bc4e2
.topping
  .header
    .logo-wrapper
      @apply mr-12
      @media (max-width: 768px)
        @apply mr-4
    .logo
      min-width: 160px
      display: inline-block
      @apply text-2xl font-bold tracking-wider
      @media (max-width: 768px)
        min-width: 80px
        font-size: 14px
        @apply text-center
    .search-block
      position: relative
      &:before
        position: absolute
        width: 18px
        height: 18px
        background-image: url('/icons/search.svg')
        content: ''
        display: block
        z-index: 1
        left: 16px
        top: 10px
      @apply mr-6 flex-1
      .search-input
        @apply w-full rounded-3xl bg-gray-200 opacity-75 text-gray-500 border-0 font-sans pl-10
        padding-top: 6px
        padding-bottom: 6px

    @media (max-width: 768px)
      .account-block
        //display: block
.container-fluid
  padding: 0 24px
  max-width: 100%

@media only screen and (min-width: 1200px)
  .container-fluid
    padding: 0 20px
@media only screen and (min-width: 1440px)
  .container-fluid
    padding: 0 160px
  h1
    font-size: 156px
  h2
    font-size: 24px

  h3
    font-size: 20px
    margin-bottom: 20px

  .meta-info-block
    h4
      font-size: 20px
    .source-info
      width: 500px
      font-size: 16px


  // For fututre use, when we'll have a dark theme
  // @media (prefers-color-scheme: dark)
</style>

<script>
  // import Chapter from '@/Pages/Chapter/Page';
  import Switch from '@/Form/Switch';
  import app from '@/app';
  import JetDropdown from '@/Jetstream/Dropdown';
  import JetDropdownLink from '@/Jetstream/DropdownLink';
  import JetNavLink from '@/Jetstream/NavLink';
  import JetResponsiveNavLink from '@/Jetstream/ResponsiveNavLink';
  import ConvertMixin from '@/Snippets/ConvertMixin';
  import Loader from '@/components/Loader.vue';

  export default {
    mixins: [ConvertMixin],
    components: {
      // Chapter,
      // JetNavLink,
      Switch,
      JetDropdown,
      JetDropdownLink,
      JetNavLink,
      JetResponsiveNavLink,
      Loader,
    },
    props: {
     // props: ['titles'],
      canLogin: Boolean,
      canRegister: Boolean,
      laravelVersion: String,
      phpVersion: String,
      articles: Array,
      semester: String,
      level: String,
    },
    data: () => ({
      loading: true,
      query: "",
      showme: false,
      title: {},
      queryResults: [],
      querysbTree: [],
      url: [],
    }),
    methods: {
      logout() {
          this.$inertia.post(route('logout'));
      },

      blur(){
        if(screen.width<=500){
          document.getElementById("search-change").style.width = "100px";
          document.getElementById("switch").style.display = "block";
          document.getElementById("myprofile").style.display = "block";
          this.showme = false;

        }
        this.loading = false;
        this.showme = false;

      },

      changewidth(){
        if(screen.width<=500){
          document.getElementById("search-change").style.width = "300px";
          document.getElementById("switch").style.display = "none";
          document.getElementById("myprofile").style.display = "none";
        }
      },

      getArticleRoute(article) {
        let level_id = article.level_id;
        let semester = 0;
        let level = 0;

        // Complicated not to forget to simplify and explain!
        for(let i = 1; i <= 2; i++){
          for(let j = 1; j <= 4; j++){
              if(this.querysbTree[i][j]==level_id){
                semester = i;
                level = j;
                //console.log(semester, " ", level);
              }
            }
        }
        return route('s.b.articleBySlug', [semester, level, article.slug]);
      },


      async search() {
        this.loading = true;
        this.showme = true;

        if(this.query.length>=3){
          await axios.get('/search?search='+this.query)
          .then((response)=>{
            this.queryResults = response.data.articles;

            this.querysbTree = response.data.sbTree;

            console.log(this.queryResults);

            this.loading = false;

            if(this.queryResults.length==0){
              this.loading = true;
            }
          })

          .catch((error)=>{
            console.log(error)
          })

        }
      }

    },
    computed: {
      logoText: function() {
        if (this.$root.sharedState.lang == 'kaz') {
          return 'ҚАЗАҚ ТІЛІ';
        } else {
          var placeholder = document.getElementById("search-change").getAttribute("placeholder");
          document.getElementById("search-change").placeholder = "Oqulyq boiynşa ızdeu";
          console.log(document.getElementById("search-change").getAttribute("placeholder"));

          return 'QAZAQ TİLİ';
        }
      }
    },
  }
</script>

```
