<template>
  <div class="products-list">
    <h1>Products list</h1>
    <p class="lead">This project is a Vue.js implementation of Snacks API</p>

    <form>
      <div class="form-group">
        <div class="input-group">
          <input class="form-control" name="q" placeholder="Search for products by name" v-model="search">
          <span class="input-group-append">
            <button type="submit" class="btn btn-outline-secondary">
              Search
            </button>
          </span>
        </div>
      </div>
    </form>

    <div class="row" v-if="products.length > 0">
      <div class="col-md-3 col-sm-4 col-xs-6" v-for="product in products" :key="product.uuid">
        <div class="card">
          <img class="card-img-top" src="http://via.placeholder.com/480x240" :alt="product.name">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">Product description goes here.</p>
            <button type="button" class="btn btn-primary">
              Likes <span class="badge badge-light">{{ product.likes }}</span>
            </button>
          </div>
          <div class="card-footer">
            <h4 class="text-right">{{ product.price | currency }}</h4>
          </div>
        </div>
      </div>
    </div>
    <div class="alert alert-primary" role="alert" v-else>
      No products matching current search criteria.
    </div>
    <div class="alert alert-warning" role="alert" v-if="error.length > 0">
      {{ error }}
    </div>
    <loading :active.sync="loading" :can-cancel="false" :is-full-page="true"></loading>
  </div>
</template>

<script>
  import numeral from 'numeral';
  import Loading from 'vue-loading-overlay';

  export default {
    name: 'ProductsList',
    data: function () {
      return {
        products: [],
        loading: false,
        search: '',
        error: ''
      }
    },
    created: function () {
      this.getProducts();
    },
    watch: {
      search: function (value) {
        this.getProducts(value);
      }
    },
    filters: {
      currency: function (value) {
        return numeral(value).format('$0,0.00');
      }
    },
    methods: {
      getProducts: function (search = '') {
        var self = this;

        self.loading = true;

        this.$http.get('/products?q[name_cont]=' + search).then(function (response) {
          self.loading = false;
          self.products = response.data;
          self.error = '';
        }).catch(function (error) {
          self.error = error;
          self.loading = false;
          self.products = [];
        });
      }
    },
    components: {
      Loading
    }
  }
</script>
