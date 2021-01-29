<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <ChatBot msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import ChatBot from '@/components/ChatBot.vue'; // @ is an alias to /src

@Component({
  components: {
    ChatBot,
  },
})
export default class Home extends Vue {

}
</script>
