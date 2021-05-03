<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        nuxt-identity
      </h1>
      <div class="links">
        <!-- <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a> -->
        <button
          v-if="!$auth.loggedIn"
          @click="triggerNetlifyIdentityAction('login')"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Log In
        </button>
        <nuxt-link
          v-if="$auth.loggedIn"
          to="/protected"
          target="_blank"
          rel="noopener noreferrer"
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
}

export default {
  data: () => ({

  }),
  methods: {
    triggerNetlifyIdentityAction(action) {
      if (action == "login" || action == "signup") {
        netlifyIdentity.open(action);
        netlifyIdentity.on(action, user => {
          // this.currentUser = {
          //   username: user.user_metadata.full_name,
          //   email: user.email,
          //   access_token: user.token.access_token,
          //   expires_at: user.token.expires_at,
          //   refresh_token: user.token.refresh_token,
          //   token_type: user.token.token_type
          // };
          // this.updateUser({
          //   currentUser: this.currentUser
          // });
          netlifyIdentity.close();
        });
      } else if (action == "logout") {
        // this.currentUser = null;
        // this.updateUser({
        //   currentUser: this.currentUser
        // });
        // netlifyIdentity.logout();
        // this.$router.push({ name: "Home" });
      }
    }
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
