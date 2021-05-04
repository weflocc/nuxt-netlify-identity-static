<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        nuxt-identity
      </h1>
      <div class="links">
        <button
          v-show="!$auth.loggedIn"
          @click="triggerNetlifyIdentityAction('login')"
          class="button--green"
        >
          Log In
        </button>
        <button
          v-show="$auth.loggedIn"
          @click="triggerNetlifyIdentityAction('logout')"
          class="button--green"
        >
          Log Out
        </button>
        <nuxt-link
          v-show="$auth.loggedIn"
          to="/protected"
          class="button--grey"
        >
          Protected Page
        </nuxt-link>
      </div>
    </div>
  </div>
</template>

<script>
import netlifyIdentity from "netlify-identity-widget"

if (process.browser) {
  netlifyIdentity.init({
    APIUrl: "https://relaxed-lamport-ca12e6.netlify.app/.netlify/identity"
  })
  // const user = netlifyIdentity.currentUser()
  // if (user) {
  //   console.log(`user detected:`)
  //   // this.$auth.setUser(user)
  // }
}

export default {
  methods: {
    async triggerNetlifyIdentityAction(action) {
      if (action == "login" || action == "signup") {
        netlifyIdentity.open(action)
        netlifyIdentity.on(action, user => {
          console.log('action below:')
          console.log(action)
          this.$auth.setUserToken(user.token.access_token, user.token.refresh_token)
            .then(() => netlifyIdentity.close())
        })
      } else if (action == "logout") {
        this.$auth.logout()
          .then(() => netlifyIdentity.logout())
      }
    }
  },
  mounted() {
    const user = netlifyIdentity.currentUser()
    console.log('mounted user below:')
    console.log(this.$auth.user)
    if (user) {
      console.log('user detected on currentUser')
      if (!this.$auth.user) {
        this.$auth.setUser(user)
        console.log(this.$auth.user)
      } else {
        console.log(`this.$auth.user already found:`)
        console.log(this.$auth.user)
      }
    } else {
      console.log('user not detected on currentUser')
    }
    netlifyIdentity.on('init', user => {
      console.log('init:')
      if (user) {
        console.log('user detected on init')
        if (!this.auth.user) {
          this.$auth.setUser(user)
        } else {
          console.log(`this.$auth.user already found:`)
          console.log(this.$auth.user)
        }
        this.$auth.setUser(user)
      } else {
        console.log('no user detected on init')
      }
    })
    netlifyIdentity.on('login', user => {
      console.log('login:')
      if (user) {
        console.log('user detected on login')
        if (!this.auth.user) {
          this.$auth.setUser(user)
        } else {
          console.log(`this.$auth.user already found:`)
          console.log(this.$auth.user)
        }
        this.$auth.setUser(user)
      } else {
        console.log('no user detected on login')
      }
    })
    // console.log('mounted!')
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
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
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
