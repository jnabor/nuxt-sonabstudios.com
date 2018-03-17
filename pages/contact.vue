<template>
  <v-container grid-list-md class="mt-4">
    <v-layout row wrap justify-center>
      <v-flex xs12>
        <v-card>
          <v-toolbar color="primary" dark dense>
            <v-toolbar-title>Send Email</v-toolbar-title>
          </v-toolbar>
          <v-card-text class="pa-4">
          <v-alert outline color="success" dismissible icon="check_circle" :value="true">
            Email sent to SonabStudios&trade;
          </v-alert>
          <v-alert outline color="error" dismissible icon="check_circle" :value="true">
            Email not sent to SonabStudios&trade;
          </v-alert>
          <v-form v-model="valid">
            <v-text-field
              label="Name"
              v-model="model.fromName"
              :rules="nameRules"
              required>
            </v-text-field>
            <v-text-field
              label="E-mail"
              v-model="model.fromEmail"
              :rules="emailRules"
              hint="We will reply to this email address"
              required>
            </v-text-field>
            <v-text-field
              label="Subject"
              v-model="model.subject"
              :rules="subjectRules"
              required>
            </v-text-field>
            <v-text-field
              label="Message"
              v-model="model.body"
              :rules="messageRules"
              multi-line
              required>
            </v-text-field>
            <v-btn
              block
              :loading="loading"
              color="primary"
              @click.native="onPost()"
              :disabled="loading"
              class="mt-2 mb-2">
              SEND
              <span slot="loader">Sending...</span>
            </v-btn>
          </v-form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      apiUrl: 'https://i6ycatp01h.execute-api.us-east-1.amazonaws.com/prod/ses-sendmail',
      valid: false,
      loader: null,
      loading: false,
      submit: false,
      model: {
        to: 'sonabstudios@gmail.com',
        body: '',
        subject: '',
        fromName: '',
        fromEmail: ''
      },
      nameRules: [
        (v) => !!v || 'Your name is required'
      ],
      emailRules: [
        (v) => !!v || 'Your email is required',
        (v) => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
      ],
      messageRules: [
        (v) => !!v || 'Message is required',
        (v) => v.length >= 15 || 'At least 15 characters'
      ],
      subjectRules: [
        (v) => !!v || 'Subject is required',
        (v) => v.length >= 5 || 'At least 5 characters'
      ]
    }
  },
  methods: {
    onPost: function () {
      console.log('sending post request to ' + this.apiUrl)
      this.loader = 'loading'
      const l = this.loader
      this[l] = !this[l]
      let xhr = new XMLHttpRequest()
      xhr.open('POST', this.apiUrl)
      xhr.onreadystatechange = (event) => {
        console.log(event.target.response)
        this[l] = false
        this.loader = null
      }
      xhr.setRequestHeader('Content-Type', 'application/json')
      let msg = JSON.stringify({
        "to": "sonabstudios@gmail.com",
        "body": this.model.body,
        "subject": this.model.subject,
        "fromname": this.model.fromName,
        "fromemail": this.model.fromEmail
      })
      xhr.send(msg)
    }
  },
  watcher: {
    valid() {
      this.submit = !this.valid
    }
  }

}

</script>

<style scoped>
</style>
