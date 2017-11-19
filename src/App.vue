<template>
  <div class="app">
    <div class="header">
      <div class="square-button" v-on:click="changeSort('date_added')" v-bind:class="{active: sort == 'date_added'}">Newest</div>
      <div class="square-button" v-on:click="changeSort('plays')" v-bind:class="{active: sort == 'plays'}">Most Popular</div>
      <div class="square-button" v-on:click="changeSort('title')" v-bind:class="{active: sort == 'title'}">A-Z</div>
    </div>
    <div class="spinner" v-if="loading"></div>
    <div class="clips" v-if="!loading">
      <Clip v-for="(value, index) in clips" :key="index" :clip="value"/>
    </div>
    <div class="footer">
      <div class="square-button" v-for="n in totalPages" :key="n" v-on:click="goToPage(n)" v-bind:class="{active: n == page}">{{ n }}</div>
    </div>
  </div>
</template>

<script>
import Clip from './components/Clip'
import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      clips: [],
      sort: 'date_added',
      sortd: 'DESC',
      count: 12,
      page: 1,
      total: 0,
      totalPages: 0,
      loading: true
    }
  },
  methods: {
    downloadClips: function () {
      this.loading = true
      let params = {
        sort: this.sort,
        sortd: this.sortd,
        count: this.count,
        page: this.page
      }
      axios.get('http://ec2-34-203-215-132.compute-1.amazonaws.com', {params: params})
      .then(response => {
        this.clips = response.data.videos
        this.total = response.data.total
        this.totalPages = Math.ceil(this.total / this.count)
        this.loading = false
      }).catch(error => {
        alert(error)
      })
    },
    changeSort: function (sort) {
      if (!this.loading && this.sort !== sort) {
        this.sort = sort
        switch (this.sort) {
          case 'date_added':
            this.sortd = 'DESC'
            break
          case 'plays':
            this.sortd = 'DESC'
            break
          case 'title':
            this.sortd = 'ASC'
            break
        }
        this.downloadClips()
      }
    },
    goToPage: function (page) {
      if (!this.loading && this.page !== page) {
        this.page = page
        this.downloadClips()
      }
    }
  },
  created: function () {
    this.downloadClips()
  },
  components: {
    Clip
  }
}
</script>

<style>
div.header {
  text-align: center;
  height: 70px;
  padding-top: 23px;
}

div.clips {
  display: flex;
  flex-wrap: wrap;
  font-size: 12px;
  justify-content: center;
}

div.footer {
  position: absolute;
  right: 0;
  bottom: 0;
  left: 0;
  height: 75px;
  padding-top: 19px;
  text-align: center;
}

div.square-button {
  cursor: pointer;
  display: inline-block;
  height: 30px;
  padding: 5px 10px;
  margin: 0 3px;
}

div.square-button:hover {
  background-color: lightgray;
}

div.square-button.active {
  background-color: lightskyblue;
}

</style>
