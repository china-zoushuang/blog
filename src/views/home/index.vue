<template>
  <div class="home">
    <Nav :source="rawData" />
    <Banner
      id="about"
      :source="rawData.about" />
    <Gallery
      id="gallery"
      :source="rawData.gallery" />
    <Article
      id="movie"
      :source="rawData.movie" />
    <Article
      id="read"
      :source="rawData.read" />
    <Tag
      id="tag"
      :source="rawData" />
    <div class="home__bottom">
      <strong class="home_bottom__copyright">copyright© zoushuang</strong>
    </div>
  </div>
</template>

<script>
import Nav from '@/components/Nav.vue';
import Banner from './Banner.vue';
import Gallery from './Gallery.vue';
import Article from './Article.vue';
import Tag from './Tag.vue';
import data from './data.json';

export default {
  components: {
    Nav,
    Banner,
    Gallery,
    Article,
    Tag,
  },
  data() {
    return {
    };
  },
  computed: {
    rawData() {
      return data;
    },
  },
};
</script>

<style lang="less" scoped>
  @deep: ~'>>>';
  .home {
    width: 100%;

    .home__bottom {

      .home_bottom__copyright {
        line-height: 60px;
        font-weight: normal;
        font-size: 12px;
        color: #fff;
        text-shadow: 2px 2px 2px rgb(75, 74, 74);
      }
    }
  }
</style>
