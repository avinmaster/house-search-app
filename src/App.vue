<template>
  <div id="app">
    <div id="nav">
      <search-component
        :loading="loading"
        v-model="name"
        @toggleFilters="filtersVisible = !filtersVisible"
        @submit="submit()"
      ></search-component>
      <transition name="fade">
        <div v-show="filtersVisible" class="filters">
          <input-component
            label="Minimal price"
            v-model="minPrice"
          ></input-component>
          <input-component
            label="Maximal price"
            v-model="maxPrice"
          ></input-component>
          <input-component
            label="bedrooms"
            v-model="bedrooms"
          ></input-component>
          <input-component
            label="bathrooms"
            v-model="bathrooms"
          ></input-component>
          <input-component label="storeys" v-model="storeys"></input-component>
          <input-component label="garages" v-model="garages"></input-component>
        </div>
      </transition>
    </div>
    <p v-show="loading" class="loading">Loading...</p>
    <results-component v-show="!loading" :results="results"></results-component>
    <p v-show="!nextPage" class="loading">
      No more houses matching your search
    </p>
  </div>
</template>

<script>
import axios from "axios";
import SearchComponent from "@/components/SearchComponent.vue";
import ResultsComponent from "@/components/ResultsComponent.vue";
import InputComponent from "@/components/InputComponent.vue";

export default {
  name: "App",

  components: {
    SearchComponent,
    ResultsComponent,
    InputComponent,
  },

  data() {
    return {
      name: "",
      minPrice: null,
      maxPrice: null,
      bedrooms: null,
      bathrooms: null,
      storeys: null,
      garages: null,
      results: [],
      filtersVisible: true,
      loading: false,
      nextPage: true,
    };
  },
  computed: {
    filtersData() {
      return {
        name: this.name,
        minPrice: this.minPrice,
        maxPrice: this.maxPrice,
        bedrooms: this.bedrooms,
        bathrooms: this.bathrooms,
        storeys: this.storeys,
        garages: this.garages,
      };
    },
  },
  methods: {
    submit() {
      this.loading = true;
      axios
        .post(
          `http://${process.env.VUE_APP_API_URL}/v1.0.0/search`,
          this.filtersData
        )
        .then((data) => {
          this.nextPage = data.data.houses.next_page_url;
          this.results = data.data.houses.data;
          this.loading = false;
        });
    },
    getNextHouses() {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;

        if (bottomOfWindow && this.nextPage) {
          axios.post(this.nextPage, this.filtersData).then((data) => {
            this.nextPage = data.data.houses.next_page_url;
            this.results = [...this.results, ...data.data.houses.data];
          });
        }
      };
    },
  },
  beforeMount() {
    this.submit();
  },
  mounted() {
    this.getNextHouses();
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  text-decoration: none;
  box-sizing: border-box;
}

body {
  background: #fefefe;
  overflow-y: scroll;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#nav {
  padding: 13px 0;
  // position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: #feebef;
}

.filters {
  padding: 0 4%;
  display: flex;
  flex-wrap: wrap;

  .group {
    width: 230px;
    margin: 0 auto;
    margin-top: 20px;
    margin-bottom: 20px;
    padding-left: 15px;

    input {
      border: 0;
      background: #fefefe;
      border-radius: 5px;
      height: 30px;
      font-size: 18px;
      padding: 0 6px;
      display: block;
      border: 1px solid #eeeeee;
      width: 200px;
    }

    & label {
      font-family: "Segoe UI Light", "Segoe UI", Tahoma, Geneva, Verdana,
        sans-serif;
      font-size: 18px;
      font-weight: lighter;
    }
  }
}
.loading {
  text-align: center;
  font-size: 20px;
  margin: 30px 0;
}
.fade-enter-active {
  animation: finished 0.3s reverse;
}
.fade-leave-active {
  animation: finished 0.3s;
}
@keyframes finished {
  0% {
    opacity: 1;
    height: 0%;
  }
  100% {
    opacity: 0;
    height: 100%;
  }
}
@media screen and (max-width: 700px) {
  .group {
    flex-direction: column;
  }
}
</style>
