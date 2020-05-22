<template>
  <div class="container">
    <h1 class="main-color text-center">Latest News</h1>
    <div class="properties">
      <form v-on:submit.prevent>
        <div class="form-item">
          <label for="country">Choose country</label>
          <select name="country" v-model="selectedCountry">
            <option v-for="country in countries" :key="country.id" :value="country.code">{{country.name}}</option>
          </select>
        </div>
        <div class="form-item">
          <label for="category">Choose category</label>
          <select name="country" v-model="selectedCategory">
            <option v-for="category in categories" :key="category.id" :value="category.code">{{category.name}}</option>
          </select>
        </div>
        <div class="form-item">
          <label for="category">Enter keyword</label>
          <input type="text" name="keyword" v-model="keyword">
        </div>
        <div class="form-item">
          <button class="btn btn-block" @click="getNews">Show News</button>
        </div>
      </form>
    </div>
    <div class="news" v-if="news">
      <div class="news-item" v-for="item in news" :key="item.id">
        <a :href="item.url" target="_blank">
          <div class="image" :style="{ backgroundImage: 'url(' + item.urlToImage + ')' }"></div>
          <div class="content">
            <h4>{{ item.title }}</h4>
            <p>{{ item.description }}</p>
            <div class="main-color source">
              <div class="date">
                <span class="news-date"><i class="far fa-calendar-plus"></i> {{ item.publishedAt.substr(0, 10) }}</span>
                <span class="news-time"><i class="far fa-clock"></i> {{ item.publishedAt.substr(11, 5)}}</span>
              </div>
              <div class="author">
                <span><i class="fas fa-angle-right"></i>{{ item.source.name }}</span>
              </div>
            </div>
          </div>
        </a>
      </div>
    </div>
    <p class="text-center powered-by">Powered by <a href="https://newsapi.org/">NewsAPI.org</a></p>
  </div>
</template>

<script>

import axios from 'axios'
import { newsApi, countriesList, categoryList } from '../settings.js'

export default {
  name: 'News',
  data: function(){
    return {
      countries: countriesList,
      selectedCountry: 'ru',
      categories: categoryList,
      selectedCategory: 'general',
      keyword: '',
      news: null,
    }
  },
  methods: {
    getNews(){
      let url = 'http://localhost:8080/v2/top-headlines?q='+ this.keyword +'&country='+ this.selectedCountry +'&category='+ this.selectedCategory +'&apiKey=' + newsApi;

      axios
        .get(url).then(response => (this.news = response.data.articles));
    }
  },
  mounted() {
      this.getNews()
  }
}
</script>