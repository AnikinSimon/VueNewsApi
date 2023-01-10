<template>
  <v-app>
    <v-app-bar
      color="deep-purple accent-4"
      dark
      prominent
    > 
    <v-container class="align-content-center">
      <v-row>
        <v-col cols="5">
          <v-toolbar-title class="mt-9">VueNewsApi</v-toolbar-title>
        </v-col>
        <v-col class="mt-5">
          <v-text-field
            v-model="q"
            label="Тэги"
          ></v-text-field>
        </v-col>
        <v-col>
          <v-select
          v-model="country"
          :items="items"
          width="50px"
          class="mt-5"
          ></v-select>
        </v-col>
        <v-col>
          <v-btn
            elevation="2"
            @click="getNewsData(country, q)"
            class="mt-8"
          >Искать новости</v-btn>
        </v-col>
      </v-row>
    </v-container>
    </v-app-bar>
    <v-main>
      <v-container fluid class="mr-10 ml-7" >
        <v-row dense>
          <v-card max-width="350" v-for="(article, index) in arcticles" :key="index" class="mr-7 ml-7 mb-4 pt-4" outlined color="grey lighten-3">
            <v-card-title>{{article.title}}</v-card-title>
            <v-img height="250px"
            :src="article.urlToImage" alt="">
            </v-img>
            <v-card-text>
              {{article.content}}
              <div>
                <v-btn
                outlined
                rounded
                text
                class="mt-2 mb-2"
                >
                  <a :href="article.url">Читать полностью</a>
                </v-btn>
              </div>
              <div>
                Автор: {{article.author}}
              </div>
            </v-card-text>
          </v-card>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios"

export default {
  name: 'App',

  components: {
  },

  data: () => ({
    arcticles: [],
    items: [
        'ru',
        'us',
        'fr',
        'gr',
      ],
      country: "us",
      q: ""
  }),
  methods: {
    getNewsData(country, q) {
      axios
      .get(`https://newsapi.org/v2/top-headlines?country=${country}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766&q=${q}`)
      .then(response => {
        this.arcticles = response.data.articles
        console.clear()
        console.log(response.data.articles)
      });
    }
  },
  mounted() {
    this.getNewsData(this.country, this.q)
  }
}
</script>
