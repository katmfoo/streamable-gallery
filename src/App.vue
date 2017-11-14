<template>
  <div class="app">
    <div class="clips">
      <Clip v-for="(value, index) in clips" :key="index" :clip="value" :number="(total - index) - ((page - 1) * count)"/>
    </div>
    <div class="footer">
      <span class="page-number" v-for="n in totalPages" :key="n" v-on:click="goToPage(n)" v-bind:class="{active: n == page}">{{ n }}</span>
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
      let params = {
        sort: this.sort,
        sortd: this.sortd,
        count: this.count,
        page: this.page
      }
      axios.get('http://localhost:3000', {params: params})
      .then(response => {
        this.clips = response.data.videos
        this.total = response.data.total
        this.totalPages = Math.ceil(this.total / this.count)
        console.log(this.clips)
      }).catch(error => {
        alert(error)
      })
    },
    goToPage: function (page) {
      this.page = page
      this.downloadClips()
    },
    nextPage: function () {
      this.page++
      this.downloadClips()
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
div.clips {
  display: flex;
  flex-wrap: wrap;
  font-size: 12px;
  justify-content: center;
  margin: 40px 0 120px 0;
  overflow: auto;
}

div.footer {
  background-color: #f1f1f1;
  border-top: 1px solid #dedede;
  bottom: 0;
  padding: 30px 0;
  position: fixed;
  text-align: center;
  width: 100%;
  z-index: 2;
}

span.page-number {
  cursor: pointer;
  margin: 0 4px;
  padding: 3px;
}

span.page-number:hover {
  color: gray;
}

span.page-number.active {
  color: gray;
  font-weight: 700;
}

</style>
