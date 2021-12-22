<template>
  <div id="app">
    <h1>レシピを探そう！</h1>
    <div>
      <div>
        <div>
          <input v-model="foodName" type="text" />

          <button v-on:click="getSmall" id="button1">入力して検索</button>
          <div id="botan">
            <button v-on:click="getLarge" id="button1">
              料理カテゴリ一覧を表示（入力不必要）
            </button>
          </div>
        </div>
      </div>
      <div v-for="bsubject in Bsubjects" :key="bsubject.categoryId" id="bsub">
        <a v-bind:href="bsubject.categoryUrl" target="”_blank”">{{
          bsubject.categoryName
        }}</a>
        |
        <button v-on:click="getRecipe(bsubject.categoryId)" id="button2">
          人気レシピを探す
        </button>
      </div>
      <div v-for="subject in subjects" :key="subject.categoryId" id="sub">
        <a v-bind:href="subject.categoryUrl" target="”_blank”">{{
          subject.categoryName
        }}</a>
      </div>
      <div v-for="recipe in infoRecipe" :key="recipe.recipeId" id="recipe">
        <h2>{{ recipe.recipeTitle }}</h2>
        <img
          v-bind:src="recipe.foodImageUrl"
          alt="I can not get this image "
          width="300"
          height="200"
        />
        <h3>材料</h3>
        <div>
          {{ recipe.recipeMaterial }}
        </div>
        <h3>コスト</h3>
        <div>
          {{ recipe.recipeCost }}
        </div>
        <h3>料理説明</h3>
        <div>
          {{ recipe.recipeDescription }}
        </div>
        <a v-bind:href="recipe.recipeUrl" target="”_blank”">URLはこちら！</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      infos: [],
      subjects: [],
      Bsubjects: [],
      foodName: "",
      infoRecipe: [],
    }
  },
  methods: {
    getLarge() {
      this.subjects.length = 0
      this.Bsubjects.length = 0
      this.infoRecipe.length = 0
      this.getId("large")
    },
    getMedium() {
      this.subjects.length = 0
      this.Bsubjects.length = 0
      this.infoRecipe.length = 0
      this.getId("medium")
    },
    getSmall() {
      this.subjects.length = 0
      this.Bsubjects.length = 0
      this.infoRecipe.length = 0
      this.getId("small")
    },
    async getId(type) {
      const res = await fetch(
        "https://app.rakuten.co.jp/services/api/Recipe/CategoryList/20170426?applicationId=1007014016464114143&categoryType=" +
          type
      )
      try {
        const value = await res.json()
        if (type === "large") {
          this.infos = value.result.large
          console.log(this.infos)
          this.Bsubjects = this.infos.filter(({ categoryName }) =>
            categoryName.match(this.foodName)
          )
        }
        if (type === "medium") {
          this.infos = value.result.medium
          console.log(this.infos)
          this.subjects = this.infos.filter(({ categoryName }) =>
            categoryName.match(this.foodName)
          )
        }
        if (type === "small") {
          this.infos = value.result.small
          console.log(this.infos)
          this.subjects = this.infos.filter(({ categoryName }) =>
            categoryName.match(this.foodName)
          )
        }
        console.log(this.subject)
      } catch (e) {
        console.log("error:" + e)
      }
    },
    async getRecipe(id) {
      console.log("id" + id)
      const res = await fetch(
        "https://app.rakuten.co.jp/services/api/Recipe/CategoryRanking/20170426?applicationId=1007014016464114143&categoryId=" +
          id
      )
      try {
        const value = await res.json()
        console.log("value:" + JSON.stringify(value))
        this.infos.length = 0
        this.subjects.length = 0
        this.Bsubjects.length = 0
        this.infoRecipe = value.result
        console.log(this.infoRecipe)
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
  font-family: cursive;
  background-color: rgb(252, 245, 253);
}
#botan {
  padding: 1rem;
  margin: 1rem;
}
#recipe {
  background-color: rgb(226, 255, 224);
  padding: 3rem;
  margin: 3rem;
}
#sub {
  padding: 5px;
}
#bsub {
  padding: 7px;
}
#button1 {
  background: #cbe8fa;
}
#button2 {
  background-color: rgb(245, 238, 202);
}
</style>
