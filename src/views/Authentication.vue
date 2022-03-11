<template>
  <v-container>
    <v-row
        justify="center" align-content="center"
    >
      <v-col cols="md">
        <v-card>
          <v-card-title>ログインチェック用</v-card-title>
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
    <v-row>
      <v-col>
          <v-btn
            style="margin-right:5px;"
            depressed
            color="grey"
            @click="signOut()"
          >
            ログアウト
          </v-btn>
          <v-btn
            style="margin-right:5px;"
            depressed
            color="teal"
            @click="onAuthStateChanged()"
          >
            ログインチェック①
          </v-btn>
          <v-btn
            style="margin-right:5px;"
            depressed
            color="teal"
            @click="isSignIn()"
          >
            ログインチェック②
          </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import firebaseApp from '../plugins/firebaseConfig'
import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "firebase/auth";

const auth = getAuth(firebaseApp)

export default {
  name: 'Authentication',
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
          alert('ログイン成功')
        })
        .catch((error) => {
          const errorCode = error.code
          const errorMessage = error.message
          console.log('errorCode: ' + errorCode)
          console.log('errorMessage: ' + errorMessage)
          alert('ログイン失敗')
        })
    },
    signOut: function () {
      signOut(auth).then(() => {
        alert('ログアウト成功')
      }).catch((error) => {
        console.log(error)
        alert('ログアウト失敗')
      })
    },
    onAuthStateChanged: function () {
      onAuthStateChanged(auth, (user) => {
        if (user) {
          const uid = user.uid;
          alert('ログイン中(onAuthStateChanged), uid: ' + uid)
        } else {
          alert('未ログイン(onAuthStateChanged)')
        }
      })
    },
    isSignIn: function () {
      const user = auth.currentUser
      if (user) {
        const uid = user.uid;
        alert('ログイン中(isSignIn), uid: ' + uid)
      } else {
        alert('未ログイン(isSignIn)')
      }
    }
  }
}
</script>
