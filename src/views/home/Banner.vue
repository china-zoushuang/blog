<template>
  <div class="banner">
    <div class="banner__article">
      <strong class="banner_article__title">{{ currentCarousel.title }}</strong>
      <div class="banner_article__subtitle">
        <span>{{ currentCarousel.date }}</span>
        <span
          v-for="(item, index) in currentCarousel.tags"
          :key="index">
          {{ `#${item}` }}
        </span>
      </div>
      <p
        v-html="currentCarousel.description"
        class="banner_article__content">
      </p>
    </div>
    <div class="banner__carousel">
      <Carousel
        slick="dots"
        :list="rawData"
        :activeIndex="activeCarousel"
        @change="activeCarousel = $event">
        <div
          v-for="(item, index) in rawData"
          :key="index"
          :class="{ 'banner_carousel__item--active': activeCarousel === index }"
          class="banner_carousel__item">
          <img
            :src="require(`@/assets/images/${item.coverImage}`)"
            :alt="item.coverImage">
        </div>
      </Carousel>
    </div>
  </div>
</template>

<script>
import Carousel from '@/components/Carousel.vue';

export default {
  components: {
    Carousel,
  },
  props: {
    source: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      activeCarousel: 0,
    };
  },
  computed: {
    rawData() {
      return this.source.data;
    },
    currentCarousel() {
      return this.rawData[this.activeCarousel];
    },
  },
};
</script>

<style lang="less" scoped>
  @deep: ~'>>>';
  .banner {
    position: relative;
    height: 100vh;
    overflow: hidden;
    background: black url('~@/assets/images/wolf-girl.jpg') top left/auto 100% no-repeat;

    .banner__article {
      position: absolute;
      top: 50%;
      right: calc(10vw + 400px);
      z-index: 1;
      width: 500px;
      box-sizing: content-box;
      text-align: left;
      color: #fff;
      transform: translateY(-50%);

      .banner_article__title {
        display: block;
        font-size: 60px ;
      }

      .banner_article__content {
        padding-top: 40px;
        margin: 0;
        line-height: 40px;
        color: rgba(255, 255, 255, .8);

        @{deep}em {
          display: inline-block;
          margin: 0 5px;
          font-size: 22px;
          color: rgba(255, 255, 255, 1);
        }
      }

      .banner_article__subtitle {
        font-size: 12px;
        font-style: oblique;
        opacity: .6;

        span {
          display: inline-block;
          margin-right: 10px;
        }
      }
    }

    .banner__carousel {
      position: absolute;
      top: 50%;
      right: 10vw;
      width: 300px;
      height: 400px;
      transform: translateY(-50%);

      &::before,
      &::after {
        content: '';
        position: absolute;
        z-index: 1;
        display: block;
        width: 100%;
        height: 100%;
        border: 2px solid #fff;
      }

      &::before {
        top: -20px;
        left: -30px;
      }

      &::after {
        right: -20px;
        bottom: -20px;
      }

      .banner_carousel__item {
        display: flex;
        align-items: center;
        width: 300px;
        height: 400px;
        border: 8px solid #fff;
        box-sizing: border-box;
        opacity: .3;
        transform: scale(.8);

        img {
          width: 100%;
        }
      }

      .banner_carousel__item--active {
        opacity: 1;
        transform: scale(1);
      }
    }
  }
</style>
