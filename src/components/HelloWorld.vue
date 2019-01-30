<template>
  <div id="app">

    <input type="text" v-model="queryURL">

    <button v-on:click="searchNaverAPI">글 불러오기</button>

    <div v-for="post in posts" v-bind:key="post.id">
      <a :href="post.link">
        <h1>{{post.title}}</h1>
      </a>
    </div>

  </div>
</template>

<script>
  import axios from 'axios'
  import cleanStr from '../add_js/cleanString'
  let clientId = "fk2b6H7p_xkrQtrtoKQP";
  let clientSecret = "GijlgkzUzM";

  export default {
    name: 'app',
    data: function () {
      return {
        queryURL: "nexon",
        posts: []
      }
    },
    mounted() {
      this.searchNaverAPI();
    },
    methods: {
      searchNaverAPI: function () {
        axios({
          method: "get",
          url: "/naversearch",
          params: {
            query: this.queryURL,
            display: "100"

          },
          crossDomain: true,
          headers: {
            "X-Naver-Client-Id": clientId,
            "X-Naver-Client-Secret": clientSecret
          }
        }).then((result) => {
          console.log(result);
          let news = result.data.items;

          this.posts = [];
          for (let i in news) {
            var tmp = {
              title: cleanStr(news[i].title, "title"),
              link: news[i].originallink
            };
            this.posts.push(tmp)
          }

        }).catch((ex) => {
          console.log("ERROR")
        })
      }
    }
  }

</script>
