<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-text-field
            label="Email*"
            v-model="email"
        ></v-text-field>
        <v-text-field
            label="Password*"
            type="password"
            v-model="pw"
        ></v-text-field>
        <v-btn
          color="primary"
          @click="createUser()"
        >
            作成
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import firebaseApp from '../plugins/firebaseConfig'
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";

const auth = getAuth(firebaseApp)

export default {
  name: 'Users',
  data: () => ({
    email: '',
    pw: ''
  }),
  methods: {
    createUser: function () {
      createUserWithEmailAndPassword(auth, this.email, this.pw)
        .then((userCredential) => {
          const user = userCredential.user
          console.log('create user success.' + user)
          alert('作成成功')
        })
        .catch((error) => {
          const errorCode = error.code
          const errorMessage = error.message
          console.log('errorCode: ' + errorCode)
          console.log('errorMessage: ' + errorMessage)
          alert('作成失敗')
        })
    }
  }
}
</script>
