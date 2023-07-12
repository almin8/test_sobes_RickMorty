<template>
  <div>
    <img :src="headSVG" width="375" height="44" />
    <div class="headFont">Персонажи</div>
    <img class="logo" :src="searchSVG" width="29" height="33" />
    <input
      class="inputSearch"
      @input="getSearch(), inputF()"
      v-model.trim="inputValueComp"
      type="text"
      placeholder="Поиск"
    />
    <div
      v-if="countDataComp == 0"
      style="text-align: center; color: #c1c2c7"
    ></div>
    <div v-if="countDataComp == 1" style="text-align: center; color: #c1c2c7">
      {{ countDataComp }} персонаж
    </div>
    <div
      v-if="countDataComp > 1 && countDataComp < 5"
      style="text-align: center; color: #c1c2c7"
    >
      {{ countDataComp }} персонажа
    </div>
    <div v-if="countDataComp > 5" style="text-align: center; color: #c1c2c7">
      {{ countDataComp }} персонажей
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      searchSVG: require("../assets/search.svg"),
      headSVG: require("../assets/head.svg"),
      inputValueComp: "",
      dataComp: [],
      countDataComp: 0,
      pageComp: 1,
      showButtonComp: false,
    };
  },
  watch: {
    clearInput() {
      this.inputValueComp = "";
      this.getSearch();
    },
  },
  props: { clearInput: { type: Number } },
  methods: {
    async getSearch() {
      if (this.pageComp > 1) {
        this.pageComp = 1;
      }
      this.dataComp = [];
      try {
        const resp = await axios.get(
          `https://rickandmortyapi.com/api/character/`,
          {
            params: { page: this.pageComp, name: this.inputValueComp },
          }
        );
        console.log(resp.data.results);
        this.dataComp.push(...resp.data.results);

        if (resp.data.info.pages == this.pageComp) {
          this.$emit("showButtonCompF", false);
        } else {
          this.$emit("showButtonCompF", true);
        }
        this.countDataComp = resp.data.info.count;
      } catch (e) {
        console.log(e);
        this.$emit("showButtonCompF", false);
        this.countDataComp = 0;
      }
      this.inputF();
    },
    inputF() {
      this.$emit("inputValueCompF", this.inputValueComp);
      this.$emit("dataCompF", this.dataComp);
    },
    async getData() {
      try {
        const resp = await axios.get(
          `https://rickandmortyapi.com/api/character/`,
          {
            params: { page: this.pageComp, name: this.inputValue },
          }
        );
        console.log(resp.data.info);
        this.dataComp.push(...resp.data.results);

        if (resp.data.info.pages == this.pageComp) {
          this.showButton = false;
        } else {
          this.showButton = true;
        }
        this.countDataComp = resp.data.info.count;

        this.$emit("dataCompF", this.dataComp);
      } catch (e) {
        console.log(e);
        this.showButton = false;
        this.countDataComp = 0;
      }
      this.inputF();
    },
  },
  mounted() {
    this.getData();
  },
};
</script>

<style scoped>
.logo {
  position: relative;
  top: 33px;
  left: 26px;
}
.headFont {
  text-align: center;
  font-size: 22px;
  margin: 18px;
  font-family: "Lato", sans-serif;
}
.inputSearch {
  margin: 18px;
  margin-top: -13px;
  padding: 8px 41px;
  border-radius: 15px;
  height: 33px;
  background-color: #ffffff;
  border: none;
  width: 71%;
  font-size: 15px;
}
</style>
