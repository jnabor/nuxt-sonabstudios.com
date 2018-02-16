<template>
  <v-content>
    <v-container>
      <v-layout row wrap justify-center>

        <v-flex xs12 md5>
          <div class="text-xs-center">
            <img class="elevation-0 mt-2 mb-3 logo" src="sonabstudios.png">
              <div class="headline"><span style="font-weight:bold">Sonab</span>Studios&trade;</div>
              <div class="subheading text-xs-center grey--text pt-2 pb-4">Progressive Real-Time Web and Mobile Applications</div>
              <v-layout row justify-center>
                <v-btn flat class="mt-2" color="blue" dark>Portfolio</v-btn>
                <v-btn flat class="mt-2" color="green" dark>About</v-btn>
                <v-btn flat class="mt-2" color="indigo" dark>Contact</v-btn>
              </v-layout>
          </div>
        </v-flex>

        <v-flex xs12 md5 offset-md1>
          <div v-for="post in posts" :key="post.title">
            <v-card class="my-3" hover >
              <v-card-media
                class="white--text"
                height="170px"
                :src="post.imgUrl">
                <v-container fill-height fluid>
                  <v-layout>
                    <v-flex xs12 align-end d-flex>
                      <span class="headline">{{ post.title }}</span>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-card-media>
              <v-card-text>
                  {{ post.content }}
              </v-card-text>
              <v-card-actions>
                <v-btn icon class="red--text">
                  <v-icon medium>fa-reddit</v-icon>
                </v-btn>
                <v-btn icon class="light-blue--text">
                  <v-icon medium>fa-twitter</v-icon>
                </v-btn>
                <v-btn icon class="blue--text text--darken-4">
                  <v-icon medium>fa-facebook</v-icon>
                </v-btn>
                <v-spacer></v-spacer>
                <v-btn flat class="blue--text">Read More</v-btn>
              </v-card-actions>
            </v-card>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
export default {
  data () {
    return {
      title: 'Your Logo',
      posts: [
        {
          title: 'Progressive Web Applications',
          content: 'Loads instantly and never shows the downasaur, even in uncertain network conditions. Responds quickly to user interactions with silky smooth animations and no janky scrolling. Feels like a natural app on the device, with an immersive user experience. ',
          imgUrl: '/img/progressive.jpeg'
        },
        {
          title: 'Internet of Things',
          content: 'A system of interrelated computing devices, mechanical and digital machines, objects, animals or people that are provided with unique identifiers and the ability to transfer data over a network without requiring human-to-human or human-to-computer interaction.',
          imgUrl: '/img/raspberry.jpeg'
        },
        {
          title: 'Technology Stack',
          content: 'Fast scalable and flexible technology stack and array of services for all applications that need consistent, single-digit millisecond latency. Cloud services and APIs that are fully managed for your backend. Reactive user interfaces and single-page applications pre-rendered on the server side.',
          imgUrl: '/img/techstack.jpeg'
        }
      ]
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.logo {
  max-height: 150px;
}

</style>
