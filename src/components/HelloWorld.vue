<template>
  <div id="app">
    <h1>RakutenAPI練習</h1>
    <div>
      <button v-on:click="getLarge">料理別で調べる</button>|
      <button v-on:click="getMedium"></button>|
      <button v-on:click="getSmall">小さいレシピ</button>
      <div>
        {{ comment }}
      </div>
      <!-- <div v-for="info in infos" :key="info.id">
        {{ info.snippet.description }}
        <div>------</div>
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      infos: [],
      comment: "",
    }
  },
  methods: {
    getLarge() {
      this.getRakutenApi("large")
    },
    getMedium() {
      this.getRakutenApi("medium")
    },
    getSmall() {
      this.getRakutenApi("small")
    },
    async getRakutenApi(recipeType) {
      const res = await fetch(
        // "https://app.rakuten.co.jp/services/api/Recipe/CategoryRanking/20170426?applicationId=1007014016464114143&categoryId=31"
        "https://app.rakuten.co.jp/services/api/Recipe/CategoryList/20170426?applicationId=1007014016464114143&categoryType=" +
          recipeType
      )
      try {
        const value = await res.json()
        console.log("value:" + JSON.stringify(value))
        this.comment = value
      } catch (e) {
        console.log("error:" + e)
      }
    },
  },
}
</script>

<style>
#app {
  text-align: center;
}
</style>
