<template>
  <div class="clip">
    <div v-html="embed"></div>
    <div class="info">
      <div class="title">{{ title }}</div>
      <div class="date">{{ date }}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Clip',
  props: ['shortCode', 'title', 'plays', 'date'],
  created: function () {
    axios.get('https://api.streamable.com/oembed.json?url=https://streamable.com/' + this.shortCode).then(response => {
      this.embed = response.data.html
    })
  },
  data () {
    return {
      embed: null
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.clip {
  border-radius: 2px;
  box-shadow: 0 1px 5px rgba(16,22,26,.2);
  margin: 10px;
  width: 400px;
}

div.info {
  display: flex;
  padding: 10px;
}

div.title {
  flex-grow: 1;
}

div.date {
  min-width: 135px;
  text-align: right;
}
</style>
