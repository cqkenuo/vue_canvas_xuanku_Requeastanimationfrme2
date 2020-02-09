<template>
  <div class="hello">
    <div class="circledemooutbox">
      <circledemo />
    </div>
  </div>
</template>

<script>
import circledemo from "./circledemo";
export default {
  name: "HelloWorld",
  components: {
    circledemo
  },
  data() {
    return {};
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "@/styles/global.scss";
.hello {
  display: flex;
  flex-flow: column;
  justify-content: flex-start;
  align-items: flex-start;
  width: vw(1920);
  height: vh(1080);
  overflow: hidden;
  .circledemooutbox {
    display: flex;
    flex-flow: column;
    justify-content: flex-start;
    align-items: flex-start;
    width: vw(1920);
    height: vh(1080);
    background-color: #42b983;
  }
}
a {
  color: #42b983;
  color: #2c3e50;
}
</style>
