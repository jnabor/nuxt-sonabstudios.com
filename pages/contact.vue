<template>
  <v-container grid-list-md class="mt-3">
    <v-layout row wrap justify-center>
      <v-flex xl5 lg5 md5 sm5 xs5>
        <v-card class="pa-4 ml-4 mr-4">
          <v-form v-model="valid">
            <v-text-field
              label="Name"
              v-model="name"
              :rules="nameRules"
              required>
            </v-text-field>
            <v-text-field
              label="E-mail"
              v-model="email"
              :rules="emailRules"
              required>
            </v-text-field>
            <v-text-field
              label="Message"
              multi-line
              required>
            </v-text-field>
          </v-form>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {

}

</script>

<style scoped>
</style>
