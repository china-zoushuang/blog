<template>
  <section class="gallery">
    <main-header :source="source"></main-header>
    <div class="gallery__list-wrapper">
      <ul
        v-for="(item, index) in list"
        :key="index"
        :class="index === 0 ? 'gallery__list--left' : 'gallery__list--right'"
        class="gallery__list">
        <li
          v-for="(childItem, childIndex) in item"
          :key="childIndex"
          :style="{
            width: index === 1 ? ((childIndex + 1) % 4 === 0 ? '50%' : '100%') : '100%'
          }"
          class="gallery_list__item">
          <img
            :src="require(`@/assets/images/${childItem.coverImage}`)"
            :alt="childItem.coverImage">
          <div class="gallery_list_item__desc">
            <h6 class="gallery_list_item_desc__title">{{ childItem.title }}</h6>
            <ul class="gallery_list_item_desc__tags">
              <li
                v-for="(grandItem, grandIndex) in childItem.tags"
                :key="grandIndex"
                class="gallery_list_item_desc_tags__item">
                {{ `#${grandItem}` }}
              </li>
            </ul>
            <p
              v-html="childItem.description"
              class="gallery_list_item_desc__text">
            </p>
          </div>
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
import MainHeader from '@/components/MainHeader.vue';

export default {
  components: {
    MainHeader,
  },
  props: {
    source: {
      type: Object,
      required: true,
    },
  },
  computed: {
    list() {
      const rawData = this.source.data;
      const leftList = rawData.slice(0, 4);
      const rightList = rawData.slice(4, rawData.length);
      return [leftList, rightList];
    },
  },
};
</script>

<style lang="less" scoped>
  .gallery {
    width: 70%;
    margin: 0 auto;

    .gallery__list-wrapper {
      padding: 50px;
      overflow: hidden;

      .gallery__list {
        float: left;
        display: flex;
        flex-wrap: wrap;

        .gallery_list__item {
          position: relative;
          cursor: pointer;

          &:hover {

            .gallery_list_item__desc {
              opacity: 1;
            }
          }

          img {
            width: 100%;
          }

          .gallery_list_item__desc {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            width: 100%;
            height: 100%;
            padding: 40px;
            box-sizing: border-box;
            background-color: rgba(0, 0, 0, .6);
            text-align: left;
            color: #fff;
            opacity: 0;
            transition: all .3s;

            .gallery_list_item_desc__title {
              margin: 0;
              font-size: 48px;
            }

            .gallery_list_item_desc__tags {
              display: flex;
              flex-wrap: wrap;
              padding: 5px 0;

              .gallery_list_item_desc_tags__item {
                margin-right: 5px;
                font-size: 12px;
                color: #d4d4d4;
              }
            }

            .gallery_list_item_desc__text {
              margin: 40px 0;
              line-height: 28px;
            }
          }
        }
      }

      .gallery__list--left {
        width: 30%;
        justify-content: flex-end;

        .gallery_list__item {
          width: 100%;
        }
      }

      .gallery__list--right {
        width: 70%;
        justify-content: flex-start;
      }
    }
  }
</style>
