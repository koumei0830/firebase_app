<template>
  <div class="mb-3">
    <form>
      <div class="form-group">
        <label>ユーザ名</label>
        <input v-model="form.name" />
      </div>
      <div class="form-group">
        <textarea v-model="form.message" rows="3" class="form-control" />
      </div>
      <a class="btn btn-light" @click="submit"> 投稿する </a>
      <router-link class="btn btn-light" to="/"> 戻る </router-link>
    </form>
  </div>
</template>

<script>
import { getAuth, onAuthStateChanged } from "firebase/auth";
import { getFirestore, collection, addDoc } from "firebase/firestore";

export default {
  data() {
    return {
      form: {
        name: null,
        message: "",
      },
    };
  },
  mounted() {
    const auth = getAuth();
    onAuthStateChanged(auth, (user) => {
      if (user) {
        this.form.name = user.displayName;
      } else {
        console.log("ログインしてください");
      }
    });
  },
  methods: {
    async submit() {
      const db = getFirestore();
      let now = new Date().getTime();
      const docMessege = addDoc(collection(db, "messages"), {
        name: this.form.name,
        message: this.form.message,
        timestamp: now,
      });
      console.log(docMessege);
      console.log("メッセージを追加しました");
      this.$router.push("/");
    },
  },
};
</script>

<style>
</style>