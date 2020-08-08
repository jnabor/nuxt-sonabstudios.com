<template>
  <v-content id="gradient">
    <v-container>
      <v-layout row wrap justify-center>
        <v-flex xs12 md5>
          <div class="text-xs-center">
            <img class="elevation-0 mt-2 mb-3 logo" src="sonabstudios.png" />
            <div class="headline">
              <span style="font-weight: bold;">SonabStudios</span>&trade;
            </div>
            <div class="subheading text-xs-center grey--text pt-2 pb-4">
              Data Automation, Analytics and Visualization
            </div>
            <v-layout row justify-center>
              <!-- <v-btn flat class="mt-2" @click="aboutActivate()" :color="aboutColor" >About</v-btn> -->
              <v-btn @click="homeActivate()" icon class="mt-2">
                <v-icon :color="homeColor">
                  home
                </v-icon>
              </v-btn>
              <v-btn
                @click="contactActivate()"
                :color="contactColor"
                flat
                class="mt-2"
              >
                Contact
              </v-btn>
            </v-layout>
          </div>
          <div v-if="contact">
            <app-contact />
          </div>
        </v-flex>
        <v-flex xs12 md5 offset-md1>
          <div v-for="post in posts" :key="post.title">
            <v-card class="my-3" hover>
              <v-card-media
                :src="post.imgUrl"
                class="white--text"
                height="240px"
              >
                <v-container fill-height fluid>
                  <v-layout>
                    <v-flex xs12 align-end d-flex>
                      <span class="headline">{{ post.title }}</span>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-card-media>
              <v-card-text>{{ post.content }}</v-card-text>
            </v-card>
          </div>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
import contact from './contact'
export default {
  components: {
    'app-contact': contact,
  },
  data() {
    return {
      about: false,
      contact: false,
      homeColor: 'h_active',
      aboutColor: '',
      contactColor: '',
      title: 'Your Logo',
      posts: [
        {
          title: 'Progressive Web Applications',
          content:
            'Web Apps that load instantly and never show the downasaur, even in uncertain network conditions. Responds quickly to user interactions with silky smooth animations and no janky scrolling. Feels like a natural app on the device, with an immersive user experience. ',
          imgUrl: '/img/progressive.jpeg',
        },
        {
          title: 'Cross-Platform Mobile Applications',
          content: `Multiple platforms, one codebase. Truly native apps that don't compromise your users' experiences.  Layered architecture allows for full customization, which results in incredibly fast rendering, expressive and flexible designs`,
          imgUrl: '/img/mobile.jpeg',
        },
        {
          title: 'Serverless Architecture',
          content: `Focus on your application and business value, not your infrastructure. You don't have to provision infrastructure or worry about scale. Serverless backends expand and contract with demand. It cuts time to market in half and reduces IT operational costs.`,
          imgUrl: '/img/serverless.jpg',
        },

        {
          title: 'Technology Stack',
          content:
            'Fast scalable and flexible technology stack for all applications that need consistent, single-digit millisecond latency. Cloud services and APIs that are fully managed. Reactive user interfaces and single-page applications pre-rendered on the server or pre-built files served over a CDN.',
          imgUrl: '/img/techstack.jpeg',
        },
      ],
    }
  },
  methods: {
    aboutActivate: function () {
      this.contact = false
      this.about = true
      this.homeColor = ''
      this.aboutColor = 'h_active'
      this.contactColor = ''
    },
    contactActivate: function () {
      this.contact = true
      this.about = false
      this.homeColor = ''
      this.aboutColor = ''
      this.contactColor = 'h_active'
    },
    homeActivate: function () {
      this.contact = false
      this.about = false
      this.homeColor = 'h_active'
      this.aboutColor = ''
      this.contactColor = ''
    },
  },
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
#gradient {
  background: linear-gradient(135deg, #fafafa, #e0e0e0);
}
</style>
