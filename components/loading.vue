<template>
  <div class="loading">
    <span></span>
  </div>
</template>

<script>
export default {
  name: 'Loading',
}
</script>

<style lang="scss">
.loading {
  height: 60vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  @keyframes circle-loading {
    to {
      transform: rotateZ(360deg);
    }
  }
  > span {
    display: block;
    width: 5rem;
    height: 5rem;
    border-radius: 50%;
    background-color: transparent;
    border: 5px solid rgb(0,122,255);
    border-top-color: #fff;
    animation: circle-loading 1.2s linear infinite;
  }
}
</style>
