<template>
  <div class="view login">
    <h3 class="tripplanner-header">TripPlanner</h3>
    <div id="firebaseui-auth-container"></div>
  </div>
</template>

<script>
import firebase from 'firebase'
import firebaseui from 'firebaseui'
// import { config } from '@/firebase.config'

export default {
  name: 'login',
  components: {
  },
  mounted () {
    const uiConfig = {
      signInSuccessUrl: '/',
      signInOptions: [
        firebase.auth.FacebookAuthProvider.PROVIDER_ID
      ]
    }
    const ui = new firebaseui.auth.AuthUI(firebase.auth())
    ui.start('#firebaseui-auth-container', uiConfig)
  }
}
</script>

<style>
.login, .firebaseui-card-content,
.firebaseui-container, #firebaseui-auth-container {
  text-align: center;
  height: 100%;
}
.firebaseui-container {
  background-color: transparent;
  margin: 0 -15px;
  width: 110%;
}
.firebaseui-idp-facebook {
  position: absolute !important;
  left: 50%;
  top: 50%;
  margin-left: -110px !important;
  margin-top: -50px !important;
}
.tripplanner-header {
  color: #fff;
  position: absolute !important;
  left: 60%;
  top: 42%;
  margin-left: -110px !important;
  margin-top: -50px !important;
}
</style>
