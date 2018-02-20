<template>
<v-app>

  <v-toolbar
    color="yellow darken-1"
    light dense>
    <v-toolbar-side-icon></v-toolbar-side-icon>
    <v-spacer></v-spacer>
    <v-btn icon class="white--text" onclick="location.href='https://github.com/sonabstudios';">
      <img class="github" src="/github64.png">
    </v-btn>
  </v-toolbar>

  <nuxt />

  <v-footer class="pa-2 footer-fixed" dark color="grey lighten-1">
    <v-layout justify-center>
    <div>Jayson Nabor &copy; 2018</div>
    </v-layout>
  </v-footer>

</v-app>
</template>

<script>
export default {
    methods: {
    navHome: function () {
      router.push('/home')
    },
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

.github {
  max-height: 32px;
}

</style>
