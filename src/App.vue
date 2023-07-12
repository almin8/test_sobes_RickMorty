<template>
  <div id="app">
    <div class="workSpace">
      <head-part
        @inputValueCompF="inputValue = $event"
        @dataCompF="data = $event"
        @countDataCompF="countData = $event"
        @pageCompF="page = $event"
        @showButtonCompF="showButton = $event"
        :clearInput="inputValueClear"
      ></head-part>
      <body-part :dataComp="data"></body-part>
      <div class="footer" v-if="showButton" @click="getNextPage">
        <div>Показать еще</div>
        <div>↓</div>
      </div>
      <div v-if="inputValue" class="footerArea">
        <div
          @click="(inputValueClear = Math.random()), getSearch()"
          class="footerBut"
        >
          Сбросить Поиск
        </div>
        <div class="footerMenu">
          <img :src="firstMenuSVG" width="34" height="45" />
          <img :src="secondMenuSVG" width="34" height="45" />
          <img :src="thirdMenuSVG" width="34" height="45" />
          <img :src="fourthMenuSVG" width="33" height="45" />
        </div>
      </div>
      <div v-if="!inputValue" class="footerMenuStart">
        <img :src="firstMenuSVG" width="34" height="45" />
        <img :src="secondMenuSVG" width="34" height="45" />
        <img :src="thirdMenuSVG" width="34" height="45" />
        <img :src="fourthMenuSVG" width="33" height="45" />
      </div>
    </div>
  </div>
</template>

<script>
import HeadPart from "./components/HeadPart.vue";
import BodyPart from "./components/BodyPart.vue";

import axios from "axios";

export default {
  name: "App",
  components: {
    HeadPart,
    BodyPart,
  },
  data() {
    return {
      data: [],
      page: 1,
      showButton: true,
      inputValue: "",
      countData: 0,
      inputValueClear: 0,
      logoSVG: require("./assets/logo.svg"),
      firstMenuSVG: require("./assets/firstMenu.svg"),
      secondMenuSVG: require("./assets/secondMenu.svg"),
      thirdMenuSVG: require("./assets/thirdMenu.svg"),
      fourthMenuSVG: require("./assets/fourthMenu.svg"),
    };
  },
  methods: {
    async getSearch() {
      if (this.page > 1) {
        this.page = 1;
      }
      this.data = [];
      try {
        const resp = await axios.get(
          `https://rickandmortyapi.com/api/character/`,
          {
            params: { page: this.page, name: this.inputValue },
          }
        );
        console.log(resp.data.results);
        this.data.push(...resp.data.results);

        if (resp.data.info.pages == this.page) {
          this.showButton = false;
        } else {
          this.showButton = true;
        }
        this.countData = resp.data.info.count;
      } catch (e) {
        console.log(e);
        this.showButton = false;
        this.countData = 0;
      }
    },
    getNextPage() {
      this.page += 1;
      this.getData();
    },
    async getData() {
      try {
        const resp = await axios.get(
          `https://rickandmortyapi.com/api/character/`,
          {
            params: { page: this.page, name: this.inputValue },
          }
        );
        console.log(resp.data.info);
        this.data.push(...resp.data.results);

        if (resp.data.info.pages == this.page) {
          this.showButton = false;
        } else {
          this.showButton = true;
        }
        this.countData = resp.data.info.count;
      } catch (e) {
        console.log(e);
        this.showButton = false;
        this.countData = 0;
      }
    },
  },
};
</script>

<style scoped>
.workSpace {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 400px;
  background-color: #f4f5f5;
  border-radius: 20px;
}
#app {
  display: flex;
  justify-content: center;
}
.footer {
  margin-left: 17px;
  margin-right: 17px;
  margin-top: 11px;
  margin-bottom: 10px;
  padding: 8px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  background-color: #ffffff;
  border-radius: 15px;
  font-weight: 400;
  font-family: "Lato", sans-serif;
}
.footerBut {
  width: 325px;
  height: 56px;
  background-color: #cb99fa;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 15px;
  color: #ffffff;
  font-family: "Lato", sans-serif;
}
.footerArea {
  display: flex;
  justify-content: space-evenly;
  background-color: #ffffff;
  box-shadow: 0px -6px 12px 2px rgba(34, 60, 80, 0.21);
  margin-top: 10px;
  border-radius: 15px;
  flex-direction: column;
  align-items: center;
  padding: 5px;
  height: 130px;
}
.footerMenu {
  border-radius: 40px 40px 30px 30px;
  background-color: #ffffff;
  display: flex;
  justify-content: space-around;
  box-shadow: 0px -6px 12px 2px rgba(34, 60, 80, 0.21);
  height: 70px;
  align-items: center;
  width: 103%;
  margin-bottom: -30px;
}
.footerMenuStart {
  border-radius: 40px 40px 30px 30px;
  background-color: #ffffff;
  display: flex;
  justify-content: space-around;
  box-shadow: 0px -6px 12px 2px rgba(34, 60, 80, 0.21);
  height: 70px;
  align-items: center;
  width: 100%;
  margin-bottom: -30px;
  margin-top: 12px;
}
</style>
