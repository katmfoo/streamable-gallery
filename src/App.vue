<template>
  <div class="app">
    <Clip shortCode="vd4go"/>
  </div>
</template>

<script>
import Clip from './components/Clip'
import axios from 'axios'

export default {
  name: 'App',
  created: function () {
    axios.get('https://ajax.streamable.com/videos?sort=date_added&sortd=DESC&count=12&page=1', {headers: {'set-cookie': 'test=test'}}).then(response => {
      console.log(response)
      this.clips = response.data.html
    })
  },
  data () {
    return {
      clips: null
    }
  },
  components: {
    Clip
  }
}
</script>

<style>
div.app {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
