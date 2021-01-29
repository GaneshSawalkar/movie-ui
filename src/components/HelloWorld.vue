<template>
  <div class="hello">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <input name="search" v-model="searchMovie" placeholder="Search" /><button
      medium
      color="green"
      @click="getMovieListSearch(searchMovie)"
    >
      Search
    </button>
    <div></div>
    <input
      type="radio"
      name="tipoProyecto"
      value="Movies"
      :checked="isChecked"
      @click="selectedType('movie')"
      required
    />Movies
    <input
      type="radio"
      name="tipoProyecto"
      value="Series"
      :checked="isChecked"
      @click="selectedType('series')"
      required
    />Series
    <div v-for="order in orders" v-bind:key="order.key" class="product">
      <div :class="'product-inner ' + order.Poster">
        <div class="product-text-wrap">
          <!-- <h2 class="bg-text">{{ order.Poster }}</h2> -->
          <!-- <v-img :src="order.Poster"></v-img> -->
        </div>
        <div class="product-image-wrap">
          <!-- <img :src="product.src" class="image" /> -->
        </div>
        <div class="product-detail">
          <h2>{{ order.Title }}</h2>
        </div>
        <div class="product-detail">
          Type:
          {{ order.Type }}
          <h6>
            {{ order.Year }}
          </h6>
        </div>
      </div>
    </div>
    <div v-if="orders.length == 0 && !noDataFound">No Data Found</div>
    <div v-if="noDataFound">
      Please Search Movie Name(Eg. Avenger, Captain America)
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },

  data: () => ({
    orders: [
      {
        orderId: "aa",
        key: "s"
      },
      {
        orderId: "aasss",
        key: "s"
      },
      {
        orderId: "aa",
        key: "s"
      },
      {
        orderId: "aasss",
        key: "s"
      },
      {
        orderId: "aa",
        key: "s"
      },
      {
        orderId: "aasss",
        key: "s"
      }
    ],
    abc: {},
    noDataFound: false,
    searchMovie: "",
    series: "",
    movies: "",
    ordersList: "",
    isChecked: ""
  }),
  created() {
    console.log(this.isChecked);
    this.getMovieList();
  },
  methods: {
    getMovieListSearch(datas) {
      console.log(datas, ";;;;;;;;;;;;;;");
      var search = {};
      search.s = datas;
      return axios
        .post(`http://localhost:4000/movies/search/movie`, search)
        .then(result => {
          if (result.data.Response == "True") {
            this.orders = result.data.Search;
            this.ordersList = result.data.Search;

            this.noDataFound = false;
          } else {
            this.orders = [];
            this.noDataFound = true;
          }
        });
    },
    getMovieList(data) {
      return axios
        .post(`http://localhost:4000/movies/search/movie`, data)
        .then(result => {
          if (result.data == "Incorrect IMDb ID.") {
            this.noDataFound = true;
            this.orders = [];
          } else if (result.data.Response == "True") {
            this.orders = result.data.Search;
            this.ordersList = result.data.Search;
            this.noDataFound = false;
          }
        });
  
    },
    selectedType(data) {
      console.log(data, "aaaaaaaaaaaaaaa");
      this.orders = [];
      this.ordersList.forEach(o => {
        if (o.Type == data) {
          this.orders.push(o);
        }
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.product {
  flex: 1 1 33.333%;
  width: 100%;
  padding: 25px;
}
.product-inner {
  position: relative;
  padding: 25px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  perspective: 1000px;
}
.product-inner.green {
  background-image: linear-gradient(to bottom right, #24d484, #116432);
}
.product-inner.blue {
  background-image: linear-gradient(to bottom left, #24d484, #2474c4 70%);
}
.product-inner.pink {
  background-image: linear-gradient(to bottom right, #f444a4, #1168d4);
}
.product-text-wrap {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 0;
  overflow: hidden;
  perspective: 1000px;
}

.product-text-wrap h2 {
  color: #313131;
  font-size: 128px;
  font-weight: 900;
  opacity: 0.2;
  transform-origin: center;
}
.product-image-wrap {
  position: relative;
  z-index: 1;
  transform-origin: center;
}
.product-image-wrap .image {
  width: 100%;
  filter: drop-shadow(0px 0px 12px rgba(0, 0, 0, 0.25));
}
.product-detail {
  background-color: #fff;
  padding: 25px;
  margin: 0px -25px -25px;
}
.product-detail h2 {
  font-size: 24px;
  font-weight: 700;
  color: #676767;
  margin-bottom: 15px;
}
.product-detail p {
  font-size: 14px;
  line-height: 1.5;
  font-weight: 300;
  color: #676767;
}
.v-btn {
  border-radius: 15px;
}
</style>
