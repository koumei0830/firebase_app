<template>
  <div>
    <router-link class="btn btn-light" to="/form"> 投稿画面 </router-link>
    <br /><br />
    <ul class="list-group list-group-flush">
      <li class="list-group-item" v-for="message in docs">
        <div>
          {{ message.name }}
        </div>
        <div>
          {{ message.message }}
        </div>
      </li>
    </ul>
    <a class="btn btn-light" @click="logout"> ログアウト </a>
  </div>
</template>

<script>
import { getAuth, signOut } from "firebase/auth";
import {
  getFirestore,
  collection,
  doc,
  query,
  getDocs,
  orderBy,
  limit,
  where,
  onSnapshot,
} from "firebase/firestore";

export default {
  data() {
    return {
      docs: [],
    };
  },
  mounted() {
    this.getData();
  },
  computed: {
    messages() {
      return this.$store.state.chat.messages;
    },
  },
  methods: {
    logout() {
      const auth = getAuth();
      signOut(auth)
        .then(() => {
          console.log("ログアウトしました");
          this.$router.push("/login");
        })
        .catch((error) => {
          console.log("ログアウトに失敗しました");
        });
    },
    getData: async function () {
      const that = this;
      console.log("getData loaded");
      const db = getFirestore();
      const messageRef = collection(db, "messages");
      const result = query(messageRef, orderBy("timestamp"), limit(5));
      const querySnapshot = await getDocs(result);
      querySnapshot.forEach((doc) => {
        // doc.data() is never undefined for query doc snapshots
        // console.log(doc.id, " => ", doc.data());
        that.docs.push(doc.data());
      });
      console.log(this.docs);
    },
  },
};
</script>

<style>
</style>