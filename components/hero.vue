<template>
  <div class="hero">
    <div class="container hero_content">
      <div class="text">
        <h1><span>Now</span> Streaming</h1>
        <h2 class="mini-heading">it's chilling time</h2>
        <a href="#movie-grid" class="btn">View Movies</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',

}
</script>

<style lang="scss" scoped>
.hero {
  height: 400px;
  position: relative;
  background-image: url('../assets/imgs/movieHero.jpg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  @media (min-width: 750px) {
    height: 500px;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }
  &_content {
    position: relative;
    z-index: 10;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    .mini-heading {
      font-weight: 600;
      font-size: 1.1rem;
      text-transform: uppercase;
      color: #ff0000;
      margin-bottom: 1rem;
      width: fit-content;
      @media (min-width: 750px) {
        font-size: 1.25rem;
      }
    }
    h1 {
      color: #fff;
      font-size: 4rem;
      font-weight: 200;
      @media (min-width: 750px) {
        font-size: 5rem;
      }
      span {
        font-weight: 500;
      }
    }
  }
}
</style>
