<template>
  <div class="container">
    <div class="taskBar transparent"></div>
  </div>
</template>
<style scoped>
.container{
    width: 250px;
    height: 40px;
    position: absolute;
    bottom: 0;
    left:50%;
    transform: translate(-50%, 0%);
}
.container:hover > .taskBar{
    transform: translate(0%, 0);
}
</style>
