<template>
  <div class="clip">
    <div class="spinner" v-if="loading"></div>
    <span class="plays">{{ clip.plays }} views</span>
    <div class="embed" v-html="embed"></div>
    <div class="info">
      <div class="title">
        <a :href="clip.url">{{ clip.title }}</a>
      </div>
      <div class="date">{{ formatDate(clip.date_added) }}</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'Clip',
  props: ['clip'],
  created: function () {
    this.downloadEmbed()
  },
  data () {
    return {
      embed: null,
      loading: true
    }
  },
  methods: {
    downloadEmbed: function () {
      this.loading = true
      axios.get('https://api.streamable.com/oembed.json?url=https://streamable.com/' + this.clip.shortcode).then(response => {
        this.embed = response.data.html
        this.loading = false
      })
    },
    formatDate: function (unixTimestamp) {
      return moment.unix(unixTimestamp).format('D MMMM YYYY')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
div.clip {
  border-radius: 2px;
  box-shadow: 0 1px 5px rgba(16, 22, 26, 0.2);
  margin: 10px;
  position: relative;
  width: 400px;
  background-color: white;
}

div.embed {
  height: 225px;
}

@media screen and (max-width: 420px) {
  div.embed {
    height: auto;
  }
}

span.plays {
  color: white;
  font-size: 12px;
  position: absolute;
  right: 5px;
  text-shadow: 0 0 2px black;
  top: 3px;
  z-index: 1;
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
