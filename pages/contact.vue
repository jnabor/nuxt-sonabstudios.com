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
              <v-alert
                v-if="showsuccess"
                :value="true"
                outline
                type="success"
                class="mb-3"
                >Email sent to SonabStudios&trade;</v-alert
              >
            </transition>
            <transition appear name="fadeout">
              <v-alert
                v-if="showerror"
                :value="true"
                type="error"
                outline
                class="mb-3"
                >Email not sent to SonabStudios&trade;</v-alert
              >
            </transition>
            <v-form ref="form" v-model="valid">
              <v-text-field
                v-model="model.fromName"
                :rules="nameRules"
                label="Name"
                clearable
                required
              />
              <v-text-field
                v-model="model.fromEmail"
                :rules="[emailrules.required, emailrules.email]"
                label="E-mail"
                hint="We will reply to this email address"
                clearable
                required
              />
              <v-text-field
                v-model="model.subject"
                :rules="subjectRules"
                label="Subject"
                clearable
                required
              />
              <v-textarea
                v-model="model.body"
                :rules="messageRules"
                label="Message"
                clearable
                required
              />
              <vue-recaptcha sitekey="config.captchaKey">
                <v-btn
                  :loading="loading"
                  @click.native="onPost()"
                  :disabled="!submit"
                  block
                  color="primary"
                  class="mt-2 mb-2"
                >
                  SEND
                  <span slot="loader">Sending...</span>
                </v-btn>
              </vue-recaptcha>
            </v-form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import * as config from '../static/config.js'
import VueRecaptcha from 'vue-recaptcha'

export default {
  head() {
    return {
      script: [
        {
          src:
            'https://www.google.com/recaptcha/api.js?onload=vueRecaptchaApiLoaded&render=explicit',
          defer: true,
          async: true,
        },
      ],
    }
  },
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
        fromEmail: '',
      },
      nameRules: [(v) => !!v || 'Your name is required'],
      emailrules: {
        required: (v) => !!v || 'E-mail is required',
        email: (v) => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(v) || 'E-mail must be valid'
        },
      },
      messageRules: [(v) => !!v || 'Message is required'],
      subjectRules: [(v) => !!v || 'Subject is required'],
    }
  },
  watch: {
    valid() {
      this.submit = this.valid
    },
  },
  components: { VueRecaptcha },
  methods: {
    onPost: function () {
      console.log('sending post request to ' + config.apiEndpoint)
      this.submit = this.loading
      this.loader = 'loading'
      const l = this.loader
      this[l] = !this[l]
      const xhr = new XMLHttpRequest()
      xhr.open('POST', config.apiEndpoint)
      xhr.onreadystatechange = (event) => {
        const responseUrl = event.target.responseURL
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
      xhr.setRequestHeader('x-api-key', config.apiKey)
      let message = this.model.body
      // eslint-disable-next-line
      message = message
        .replace(/\n/g, '\\\\n')
        .replace(/\r/g, '\\\\r')
        .replace(/\t/g, '\\\\t')
        .replace(`"`, `\\"`)
      const msg = JSON.stringify({
        to: this.model.to,
        body: message,
        subject: this.model.subject,
        fromname: this.model.fromName,
        fromemail: this.model.fromEmail,
      })
      xhr.send(msg)
    },
  },
}
</script>

<style scoped>
.fadeout-enter-active,
.fadeout-leave-active {
  transition: opacity 1s;
}
.fadeout-enter,
.fadeout-leave-to {
  opacity: 0;
}
</style>
