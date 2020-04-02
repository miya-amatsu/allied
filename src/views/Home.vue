<template>
  <div class="home">
    <YoutubeList />
  </div>
</template>

<script>
// @ is an alias to /src
import YoutubeList from '@/components/YoutubeList.vue'

export default {
  name: 'Home',
  components: {
    YoutubeList
  },
   created(){
     console.log("Homecreated");
   },
   mounted(){
      console.log("Homemounted");
     console.log(YoutubeList);
     console.log(YoutubeList.data);
   }
}
</script>
