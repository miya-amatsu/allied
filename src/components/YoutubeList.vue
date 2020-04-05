<template>
  <div>
    <h1>YoutubeList</h1>

    <div class="flex browse">
      <div id="video-renderer" class="video-renderer" v-for="(item) in thislistFun(stayPage)" :key="item.id">
        <a v-bind:href="root+'video?v='+item.id">
        <img v-bind:src="item.snippet.thumbnails.medium.url" alt="">
        </a>
        <div class="video-text flex">
          <div class="collect-renderer" v-on:click="thislistFun (stayPage),collectList(item.id)">
            <img v-if="item.collect" src="@/assets/liked-icon.svg">
            <img v-else src="@/assets/like-icon.svg">
          </div>
          <div class="video-text-content">
            <h3>{{item.snippet.title}}</h3>
            <!-- <p class="">{{item.contentDetails.duration}}</p> -->
            <p class="">{{item.snippet.description}}</p>
          </div>
        </div>
        
      </div>
    </div>
    <ul class="flex page">
      <li v-for="item in pageAmount(videoAmount)" :key="item" v-on:click=" thislistFun (item)">
        {{item}}
      </li>
    </ul>
  </div>
</template>

<script>
  import key from '@/assets/key.json'
  import testDataJson from '@/assets/test.json'
  export default {
    name: 'YoutubeList',
    props: {
      root: String
    },
    data() {
      return {
        ListObj: "",
        thislist: [],
        maxResults: 50,
        maxListResults: 100,
        stayPage: 1,
        videoAmount: 12,
        nextPageToken: "",
        prevPageToken: "",
        testDataJson: testDataJson,
      }
    },

    created() {
        this.getYoutubeObj();
        console.log('YOUroot');
         console.log(this.root);
        
        
    },
    mounted() {

    },
    updated() {

    },
    computed: {},

    methods: {
      getYoutubeObj(pageToken = "") {
        console.log('get');

        const vm = this;
        // `part=snippet,contentDetails`
        //`part=id,player`
        let url = `https://www.googleapis.com/youtube/v3/videos?` +
          `part=snippet,contentDetails` +
          `&chart=mostPopular` +
          `&maxResults=${vm.maxResults}` +
          `&pageToken=${pageToken}` +
          `&key=${key.key}`;
        vm.$http.get(url).then((res) => {
          let strVM = "";
          let strRes = JSON.stringify(res.data.items);
          // let strRes=vm.testDataJson.items;   
          if (vm.ListObj == "") {
            vm.ListObj = res.data.items;
            //vm.ListObj   =strRes;
          } else {
            strVM = JSON.stringify(vm.ListObj);
            strVM = strVM + strRes;
            strVM = strVM.replace('][', ',');
            vm.ListObj = JSON.parse(strVM);
          }
          let tag = localStorage.getItem("id")
          if (tag) {
            vm.ListObj.map(x => {
              let id = x.id;
              let str = tag.indexOf(id);
              if (str == -1) {
                x.collect = false;
              } else if (str != -1) {
                x.collect = true;
              }
            })
          }

          vm.nextPageToken = res.data.nextPageToken;
          vm.prevPageToken = res.data.prevPageToken;
          if (vm.ListObj.length < vm.maxListResults) {
            vm.getYoutubeObj(vm.nextPageToken)
          }
        }).catch(function (error) {
          console.log(error);
        })
      },
      thislistFun(page = 1) {
        const vm = this;
        this.stayPage = page;
        if (vm.ListObj == "") {
          this.thislist = [{
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
          }]
          return this.thislist;
        }
        let Amount = this.videoAmount;
        if (vm.ListObj.length < Amount) {
          Amount = vm.ListObj.length;
        } else if (vm.ListObj.length < Amount * page) {
          Amount = vm.ListObj.length % Amount;

        }
        let setThislist = [];
        for (let i = 0; i < Amount; i++) {
          setThislist[i] = vm.ListObj[(page - 1) * this.videoAmount + i]
        }
        this.thislist = setThislist;
        return setThislist;
      },
      pageAmount(videoAmount) {
        return Math.ceil(this._data.ListObj.length / videoAmount)
      },
      collectList(id = "") {
        console.log('hit');
        
        let vm = this;
        let tag = localStorage.getItem("id")
        if (id != "") {
          let str=-1;
          if(tag){
             str = tag.indexOf(id);
          }
          
          if (str == -1) {
            tag = tag + `,${id}`
            localStorage.setItem("id", tag)
          } else if (str != -1) {
            tag = tag.replace(`,${id}`, "")
            localStorage.setItem("id", tag)
          }
          vm.ListObj.map(x => {
            let id = x.id;
            let str = tag.indexOf(id);
            if (str == -1) {
              x.collect = false;
            } else if (str != -1) {
              x.collect = true;
            }
          })
          this.$forceUpdate()
        }
      }
    }

  }
</script>