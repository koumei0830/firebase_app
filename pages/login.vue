<template>
  <div>
    <a class="btn btn-light" @click="login"> ログイン </a>
  </div>
</template>

<script>
import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
export default {
  data() {
    return {};
  },
  methods: {
    login() {
      const auth = getAuth();
      const provider = new GoogleAuthProvider();
      signInWithPopup(auth, provider)
        .then((result) => {
          console.log("ログインに成功しました");
          this.$router.push("/");
        })
        .catch((error) => {
          console.log("ログインに失敗しました");
        });
    },
  },
};
</script>

<style>
</style>