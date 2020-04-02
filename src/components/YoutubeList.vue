<template>
  <div >
    <h1>YoutubeList</h1>
    
    <div class="flex browse">
      <div class="col-4" v-for="(item) in ListObj.items" :key="item.id">
       <a v-bind:href="'https://www.youtube.com/watch?v='+item.id">
       <img v-bind:src="item.snippet.thumbnails.medium.url" alt="">
       <p class="">{{item.snippet.title}}</p>
       <p class="">{{item.contentDetails.duration}}</p>
       <p class="">{{item.snippet.description}}</p>
       </a>
      </div>
    </div>

    
  </div>
</template>

<script>
import key from '@/assets/key.json'

export default {
  name: 'YoutubeList',
  props: {
  },
  data(){
    return{
      ListObj:{
      },
      maxResults:3,
      nextPageToken:"",
      prevPageToken:"",
      url:"",
    }
  },

  created(){
    console.log("YoutubeListcreated");
    this.getYoutubeObj();
  },
  mounted(){
     console.log("YoutubeListmounted");
   },
  methods:{
    getYoutubeObj(pageToken=""){
      const vm = this;
      let url=`https://www.googleapis.com/youtube/v3/videos?`+
            `part=snippet,contentDetails`+
            `&chart=mostPopular`+
            `&maxResults=${vm.maxResults}`+
            `&pageToken=${pageToken}`+
            `&key=${key.key}`
    vm.$http.get(url).then((res)=>{
      vm.ListObj=res.data;
      console.log(vm.ListObj.items[0]);
      vm.nextPageToken = vm.ListObj.nextPageToken;
      vm.prevPageToken = vm.ListObj.prevPageToken;
    }).catch(function(error){
      console.log(error);
    })
    },
    getUrl(id){
      console.log(id);
    }
   }
}
</script>
