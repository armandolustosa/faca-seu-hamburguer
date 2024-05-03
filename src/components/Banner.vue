<template>
  <div id="main-banner">
    <h1>Monte seu Hambúrguer</h1>
  </div>
</template>

<script>
export default {
  name: "Banner",
};
</script>

<style scoped>
#main-banner {
  background-image: url("/public/img/burger.jpg");
  background-position: 0px ;
  background-size: 120%;
  height: 400px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}

#main-banner > h1 {
  color: #F5DEB3;
  text-align: left;
  font-size: 2em;
  background-color: #D2691E;
  padding: 20px 30px;
  border-bottom: 4px solid #F5DEB3;
  border-bottom-right-radius: 60px;
  border-top-right-radius: 60px;
}
</style>
