<template>
  <div class="container">
    <div>
      <logo />
      <h1 class="title">
        sample-axios-app
      </h1>
      <h2 class="subtitle">
        My sublime Nuxt.js project
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey"
        >
          GitHub
        </a>
      </div>
      <div class="links">
        <button @click="weather" class="button is-primary">{{buttonTilte}}</button>
      </div>
      <div class="section">
        <div><strong>{{tilte}}</strong></div>
        <div class="columns">
          <div class="column" v-for="w in weatherInfo">
            <div>{{w.dateLabel}}</div>
            <div>（{{w.date}}）</div>
            <div>{{w.telop}}</div>
            <div>{{(w.temperature.max != null) ? w.temperature.max.celsius + '℃' : '-'}}</div>
            <div>{{(w.temperature.max != null) ? w.temperature.max.fahrenheit + '%' : '-'}}</div>
            <div>
              <div :style="{'margin':'auto', 'height':'31px','width':'50px','background-image': 'url(' + w.image.url + ')' }"></div>
            </div>
          </div>
        </div>
        <div><p>{{text}}</p></div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

const CITY_CODE_OSAKA = '270000'

export default {
  asyncData (context) {
    return {buttonTilte: '天気予報取得'}
  },
  components: {
    Logo
  },
  data() {
    return {weatherInfo:[], tilte:'', text: ''}
  },
  methods: {
    weather: function(e) {

      let self = this

      axios({
        method:"GET",
         url: "/api/forecast/webservice/json/v1",
         params:{"city" : CITY_CODE_OSAKA},
         headers: {},
      }).then(function(responce) {
        console.log(responce)
       self.weatherInfo = responce.data.forecasts
       self.tilte = responce.data.title
       self.text = responce.data.description.text
      }).catch(function(error) {
        console.log(error)
      })
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
