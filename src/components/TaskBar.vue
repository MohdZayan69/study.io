<template>
  <div class="container">
    <div class="taskBar transparent"></div>
  </div>
</template>
<style scoped>
.container{
    width: 48px;
    height: 250px;
    position: absolute;
    top: 50%;
    left:0%;
    transform: translate(0%, -50%);
}
.container:hover > .taskBar{
    transform: translate(0%, 0);
}
</style>
