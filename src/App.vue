<template>
  <v-app>
    <v-navigation-drawer fixed :clipped="clipped" v-model="drawer" app>
      <v-list>
        <v-list-tile value="true" v-for="(source, i) in sources" :key="i">
          <v-btn color="primary" @click="changeSource(source)" block>
            <v-list-tile-content>
              <v-list-tile-title class="white--text">{{ source.name }}</v-list-tile-title>
            </v-list-tile-content>
          </v-btn>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar fixed dense app :clipped-left="clipped">
      <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
      <v-toolbar-title>Vue News</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <v-container fluid>
        <v-layout column align-center>
          <h2 class="text-xs-center">News Articles from ( {{ selectedSource }} )</h2>
          <p class="text-xs-center subheading">Select the souce from the menu</p>
        </v-layout>

        <v-layout row wrap>
          <v-flex class="mb-4" v-for="(article, i) in articles" :key="i">
            <v-card color="blue darken-4" class="white--text">
              <v-layout row>
                <v-flex xs12 sm7>
                  <v-card-title primary-title>
                    <div>
                      <div class="headline mb-2">{{ article.title }}</div>
                      <p>{{ article.content }}</p>
                    </div>
                  </v-card-title>
                  <v-card-actions class="pa-3">

                    <v-btn outline color="white" target="_blank" :href="article.url">Read More</v-btn>
                  </v-card-actions>
                </v-flex>
                <v-flex xs12 sm5>
                  <v-img :src="article.urlToImage ? article.urlToImage : 'public/v.png' " height="100%" cover></v-img>
                </v-flex>
              </v-layout>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
      </v-card>
      </v-flex>
      </v-layout>

      </v-container>
    </v-content>

    <v-footer app>
      <v-layout justify-center row wrap>
        <v-flex grey darken-4 py-3 text-xs-center white--text xs12>
          &copy; 2018 — NKWEBDEV | <strong>Powered by NewsAPI.org</strong>
        </v-flex>
      </v-layout>
    </v-footer>

  </v-app>
</template>

<script>
  export default {
    data() {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        sources: [],
        right: true,
        rightDrawer: false,
        articles: [],
        selectedSource: 'No source Selected'
      }
    },
    created() {
      this.$http.get('https://newsapi.org/v2/sources?language=en&apiKey=e1c48bc7c61c43b19d186af24a50ecbc')
        .then(response => {
          this.sources = response.data.sources
        })
    },
    methods: {
      changeSource(source) {
        console.log(source.id)
        this.selectedSource = source.name
        this.$http.get('https://newsapi.org/v2/everything?sources=' + source.id +
            '&apiKey=e1c48bc7c61c43b19d186af24a50ecbc')
          .then(response => {
            this.articles = response.data.articles
          })
      }
    }
  }
</script>