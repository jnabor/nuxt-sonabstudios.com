<template>
  <v-container grid-list-md class="mt-4 ml-0 mr-0 pa-0">
    <v-layout row wrap justify-center>
      <v-flex xs12>
        <v-card>
          <v-toolbar class="elevation-0" color="grey darken-2" dark dense>
            <v-toolbar-title>Send Email</v-toolbar-title>
          </v-toolbar>
          <v-card-text class="pa-4">
          <transition appear name="fadeout">
            <v-alert v-if="showsuccess" outline type="success" class="mb-3" :value="true">
              Email sent to SonabStudios&trade;
            </v-alert>
          </transition>
          <transition appear name="fadeout">
            <v-alert v-if="showerror" outline type="error" class="mb-3" :value="true">
              Email not sent to SonabStudios&trade;
            </v-alert>
          </transition>
          <v-form v-model="valid" ref="form" lazy-validation>
            <v-text-field
              label="Name"
              clearable
              v-model="model.fromName"
              :rules="nameRules"
              required>
            </v-text-field>
            <v-text-field
              label="E-mail"
              clearable
              v-model="model.fromEmail"
              :rules="[emailrules.required, emailrules.email]"
              hint="We will reply to this email address"
              required>
            </v-text-field>
            <v-text-field
              label="Subject"
              clearable
              v-model="model.subject"
              :rules="subjectRules"
              required>
            </v-text-field>
            <v-text-field
              label="Message"
              clearable
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
              :disabled="!submit"
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
import * as config from '../static/config.js'

export default {
  data: function () {
    return {
      showerror: false,
      showsuccess: false,
      valid: false,
      loader: null,
      loading: false,
      submit: false,
      model: {
        to: config.emailReceiver,
        body: '',
        subject: '',
        fromName: '',
        fromEmail: ''
      },
      nameRules: [
        v => !!v || 'Your name is required'
      ],
      emailrules: {
        required: (v) => !!v || 'E-mail is required',
        email: (v) => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(v) || 'E-mail must be valid'
        }
      },
      messageRules: [
        v => !!v || 'Message is required'
      ],
      subjectRules: [
        v => !!v || 'Subject is required'
      ]
    }
  },
  methods: {
    onPost: function () {
      console.log('sending post request to ' + config.apiEndpoint)
      this.submit = this.loading
      this.loader = 'loading'
      const l = this.loader
      this[l] = !this[l]
      let xhr = new XMLHttpRequest()
      xhr.open('POST', config.apiEndpoint)
      xhr.onreadystatechange = (event) => {
        let responseUrl = event.target.responseURL
        console.log(event.target.response)
        if (responseUrl === config.apiEndpoint) {
          this.showsuccess = true
          this.showerror = false
          setTimeout(() => (this.showsuccess = false), 5000)
        } else {
          this.showsuccess = false
          this.showerror = true
          setTimeout(() => (this.showerror = false), 5000)
        }
        this[l] = false
        this.loader = null
        this.submit = this.valid
        this.model.body = ''
        this.model.subject = ''
        this.model.fromName = ''
        this.model.fromEmail = ''
        this.$refs.form.reset()
      }
      xhr.setRequestHeader('Content-Type', 'application/json')
	  let message = this.model.body
	  // eslint-disable-next-line
      message = message.replace(/\n/g, "\\\\n").replace(/\r/g, "\\\\r").replace(/\t/g, "\\\\t")
      let msg = JSON.stringify({
        "to": this.model.to,
        "body": message,
        "subject": this.model.subject,
        "fromname": this.model.fromName,
        "fromemail": this.model.fromEmail
      })
      xhr.send(msg)
    }
  },
  watch: {
    valid () {
      this.submit = this.valid
    }
  }
}

</script>

<style scoped>
.fadeout-enter-active, .fadeout-leave-active {
  transition: opacity 1s;
}
.fadeout-enter, .fadeout-leave-to {
  opacity: 0;
}
</style>
