<template>
  <div class="home">
    <YoutubeList v-bind:root="post.root" />
  </div>
</template>

<script>
// @ is an alias to /src
import YoutubeList from '@/components/YoutubeList.vue'

export default {
  name: 'Home',

  data() {
    return {
      post:{root:window.location.href}
    }
  },
  components: {
    YoutubeList
  },
   created(){
   },
   mounted(){
     
   }
}
</script>
