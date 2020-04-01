<template>
  <section class="section">
    <main-header :source="source"></main-header>
    <div class="section__cover">
      <Carousel
        slick="arrow"
        :range="5"
        :list="cover"
        :activeIndex="activeCover"
        @change="activeCover = $event">
        <div
          v-for="(item, index) in cover"
          :key="index"
          class="section_cover__item"
          @click="selectedCover = index">
          <em class="section_cover_item__subtitle">{{ item.date }}</em>
          <strong class="section_cover_item__title">{{ item.title }}</strong>
          <div class="section_cover_item__img">
            <img
              :src="require(`@/assets/images/${item.coverImage}`)"
              :alt="item.coverImage">
          </div>
        </div>
      </Carousel>
    </div>
    <div class="section__article">
      <div
        :style="{
          backgroundImage: `url(${require(`@/assets/images/${currentCover.coverImage}`)})`
        }"
        class="section_article__background">
      </div>
      <div class="section_article__content">
        <div class="section_article_content__carousel">
          <Carousel
            slick="buttons"
            :list="currentCover.children"
            :activeIndex="activeArticle"
            @change="activeArticle = $event">
            <div
              v-for="(item, index) in currentCover.children"
              :key="index"
              class="section_article_content_carousel__item">
              <div class="section_article_content_carousel_item__img">
                <img
                  :src="require(`@/assets/images/${currentArticle.coverImage}`)"
                  :alt="currentArticle.coverImage">
              </div>
            </div>
          </Carousel>
        </div>
        <div class="section_article_content__text">
          <strong>{{ currentArticle.title || currentCover.title }}</strong>
          <p v-html="currentArticle.description || currentCover.description"></p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Carousel from '@/components/Carousel.vue';
import MainHeader from '@/components/MainHeader.vue';

export default {
  components: {
    Carousel,
    MainHeader,
  },
  props: {
    source: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      activeCover: 0,
      selectedCover: 0,
      activeArticle: 0,
    };
  },
  computed: {
    rawData() {
      return this.source;
    },
    title() {
      return this.rawData.title;
    },
    tags() {
      const newData = [];
      const rawData = this.rawData.data;
      for (const item of rawData) {
        for (const childItem of item.tags) {
          newData.push(`#${childItem}`);
        }
      }
      return [...new Set(newData)];
    },
    cover() {
      return this.rawData.data;
    },
    currentCover() {
      return this.cover[this.selectedCover];
    },
    currentArticle() {
      return this.currentCover.children[this.activeArticle];
    },
  },
};
</script>

<style lang="less" scoped>
@deep: ~'>>>';
  .section {

    .section__cover {
      width: calc(80% + 60px);
      margin: 0 auto;

      @{deep}.carousel__list-wrapper {
        overflow: hidden;
      }

      .section_cover__item {
        width: 20%;
        padding: 0 30px 100px 30px;
        border-right: 1px solid #f4f4f4;
        box-sizing: border-box;
        text-align: left;
        cursor: pointer;

        .section_cover_item__subtitle {
          border-bottom: 1px solid  #d4d4d4;
          padding-bottom: 5px;
          line-height: 20px;
          font-size: 12px;
          color: #d4d4d4;
        }

        .section_cover_item__title {
          display: block;
          margin: 15px 0;
          font-size: 16px;
        }

        .section_cover_item__img {
          position: relative;
          width: 100%;

          img {
            width: 100%;
          }
        }
      }
    }

    .section__article {
      position: relative;
      width: 100%;
      min-height: 50vh;

      .section_article__background {
        position: absolute;
        top: 0;
        left: 0;
        z-index: -1;
        width: 100%;
        height: 100%;
        background-position: center center;
        background-size: 100% auto;
        background-repeat: no-repeat;

        &::after {
          content: '';
          position: absolute;
          top: 0;
          left: 0;
          display: block;
          width: 100%;
          height: 100%;
          background-color: rgba(44, 61, 70, .7);
        }
      }

      .section_article__content {
        display: flex;
        width: 80%;
        padding: 100px 0;
        margin: 0 auto;

        .section_article_content__carousel {
          width: 300px;
          margin-right: 100px;

          @{deep}.carousel__list-wrapper {
            overflow: hidden;
          }

          .section_article_content_carousel__item {
            width: 100%;

            .section_article_content_carousel_item__img {
              display: flex;
              align-items: center;
              width: 100%;
              min-height: 400px;
              margin-right: 100px;
              background-color: black;

              img {
                width: 100%;
              }
            }
          }
        }

        .section_article_content__text {
          flex: 1;
          line-height: 32px;
          text-align: left;
          color: #fff;

          strong {
            padding-bottom: 40px;
          }
        }
      }
    }
  }
</style>
