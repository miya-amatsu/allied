<template>
  <div>
    <h1>YoutubeList</h1>
    <div class="flex browse"  >
      <div class="video-renderer" v-for="(item) in thislistFun(stayPage)" :key="item.etag">
        <a v-bind:href="'https://www.youtube.com/watch?v='+item.id">
       
          <img v-bind:src="item.snippet.thumbnails.medium.url" alt="">
          <div class="video-text">
            <h3 >{{item.snippet.title}}</h3>
             <!-- <p class="">{{item.contentDetails.duration}}</p> -->
            <p class="">{{item.snippet.description}}</p>
          </div>
        </a>
      </div>
    </div>
    
    <ul class="flex page">
      <li v-for="item in pageAmount(videoAmount)" :key="item" v-on:click=" thislistFun (item)" >
        {{item}}
      </li>
    </ul>
  </div>
</template>

<script>
  import key from '@/assets/key.json'
  export default {
    name: 'YoutubeList',
    props: {},
    data() {
      return {
        ListObj: "",
        thislist:[],
        maxResults: 50,
        stayPage:1,
        videoAmount:12,
        nextPageToken: "",
        prevPageToken: "",
      }
    },

    created() {
      console.log("YoutubeListcreated");
      this.getYoutubeObj();
      
    },
    mounted() {
      console.log("YoutubeListmounted");

    },updated(){
      const vm = this;
     // vm.getYoutubeObj(vm._data.nextPageToken)
          if(vm._data.ListObj.length<5){
            vm.getYoutubeObj(vm._data.nextPageToken )
     }
      if(vm.thislist.length<2){
         vm.thislistFun()
     }

     
    },
    computed:{

    }
    ,

    methods: {
      getYoutubeObj(pageToken = "") {
        const vm = this;
        // `part=snippet,contentDetails`
        let url = `https://www.googleapis.com/youtube/v3/videos?` +
          `part=snippet,contentDetails` +
          `&chart=mostPopular` +
          `&maxResults=${vm.maxResults}` +
          `&pageToken=${pageToken}` +
          `&key=${key.key2}`;
        vm.$http.get(url).then((res) => {
           let  strVM="";
           let  strRes=JSON.stringify(res.data.items);
            if (vm.ListObj=="") {
               vm.ListObj   =res.data.items;
               vm.ListObj
            }else {
              strVM=JSON.stringify(vm.ListObj);
              strVM=strVM+strRes;
              strVM=strVM.replace('][', ',');
              vm.ListObj=JSON.parse(strVM);
            }
          vm.nextPageToken = res.data.nextPageToken;
          vm.prevPageToken = res.data.prevPageToken;
          // vm.thislistFun()
        }).catch(function (error) {
          console.log(error);
        })
      },
      thislistFun (page=1){
        const vm = this;
        this.stayPage=page;
        if(vm.ListObj==""){
          
          this.thislist=[  {
        "id": "00000000000",
        "snippet": {
         "title": "",
         "description": "",
         "thumbnails": {
          "medium": {
           "url": "https://i.imgur.com/OnRq3PI.jpg"
          }
         }
        },
        "contentDetails": {
         "duration": "PT7M16S"
        }
       }
       ]
          
          return this.thislist;
        }
        for(let i=0;i<this.videoAmount;i++){
            vm.thislist[i]=vm.ListObj[(page-1)* 2 +i]
        }
        return this.thislist;
       }
      ,
      
      getUrl(id) {
        console.log(id);
      },
      pageAmount(videoAmount){
         return Math.ceil(this._data.ListObj.length/videoAmount)
      },
      localSetItem(e,v,){
        if(e==="id"){
          v=JSON.stringify(v);
          // isE=JSON.parse(localStorage.getItem(e))

        }
          v=JSON.stringify(v);
          e=JSON.stringify(v);
        localStorage.setItem(e, v)

      },
      localGetItem(e){
        return JSON.parse(localStorage.getItem(e))
      },
      localRemoveItem(e){
        localStorage.removeItem(e);
        return JSON.parse(localStorage.getItem(e))
      }
    },

  }

</script>