<template>
  <v-container>
    <v-row
        justify="center" align-content="center"
    >
      <v-col cols="md">
        <v-card>
          <v-card-title>ログイン</v-card-title>
            <v-form
                ref="form"
            >
              <v-row
                  justify="center" align-content="center"
              >
                <v-col
                    cols="10"
                >
                  <v-text-field
                      label="ログインID"
                      v-model="email"
                  ></v-text-field>
                  <v-text-field
                      label="パスワード"
                      v-model="pw"
                  ></v-text-field>
                  <v-spacer></v-spacer>
                  <v-btn
                      depressed
                      color="primary"
                      @click="login()"
                  >
                      ログイン
                  </v-btn>
                </v-col>
              </v-row>
            </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import firebaseApp from '../plugins/firebaseConfig'
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

const auth = getAuth(firebaseApp)

export default {
  name: 'Login',
  data: () => ({
    email: '',
    pw: ''
  }),
  methods: {
    login: function () {
      signInWithEmailAndPassword(auth, this.email, this.pw)
        .then((userCredential) => {
          const user = userCredential.user
          console.log('create user success.' + user)
          this.$router.push('/top')
        })
        .catch((error) => {
          const errorCode = error.code
          const errorMessage = error.message
          console.log('errorCode: ' + errorCode)
          console.log('errorMessage: ' + errorMessage)
          alert('認証失敗')
        })
    }
  }
}
</script>
