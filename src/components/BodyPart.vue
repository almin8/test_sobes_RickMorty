<template>
  <div style="min-height: 540px">
    <div class="searchFail" v-if="!dataComp.length">
      Не удалось найти персонажей<br />с таким именем
    </div>
    <div class="arrBody" v-for="d in dataComp" :key="d.id">
      <div class="fontBody">
        <img
          style="border-radius: 50%; width: 50px; margin-right: 10px"
          :src="d.image"
          alt="noneIMG"
        />
        {{ d.name }}
      </div>
      <div @click="alertMes(d.species)" style="margin-top: 16px">
        <img :src="logoSVG" width="21" height="22" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return { logoSVG: require("../assets/logo.svg") };
  },
  props: { dataComp: { type: Array } },
  methods: {
    alertMes(event) {
      alert(event);
    },
  },
};
</script>

<style scoped>
.searchFail {
  text-align: center;
  color: #c1c2c7;
  font-family: "Lato", sans-serif;
  font-size: 15px;
  margin-top: 50px;
  font-style: normal;
  font-weight: 100;
  line-height: normal;
  letter-spacing: -0.327px;
}
.arrBody {
  margin-left: 17px;
  margin-right: 17px;
  margin-top: 11px;
  padding: 8px;
  display: flex;
  background-color: #ffffff;
  border-radius: 15px;
  justify-content: space-between;
}
.fontBody {
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 700;
  font-family: "Lato", sans-serif;
}
</style>
