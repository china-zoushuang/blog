<template>
  <div class="main-header">
    <h2 class="main-header__title">{{ title }}</h2>
    <div class="main-header__tags">
      <ul class="main-header_tags__list">
        <li
          v-for="(item, index) in tags"
          :key="index"
          class="main-header_tags_list__item">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    source: {
      type: Object,
      required: true,
    },
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
  },
};
</script>

<style lang="less" scoped>
  .main-header {
    margin-top: 100px;
    padding: 50px 0 50px 0;

    .main-header__title {
      margin: 0;
      line-height: 100px;
      font-size: 72px;
      font-weight: normal;
    }

    .main-header__tags {

      .main-header_tags__list {
        display: flex;
        justify-content: center;

        .main-header_tags_list__item {
          margin-right: 10px;
          font-size: 12px;
          color: #d4d4d4;
          cursor: pointer;
          transition: all .3s;

          &:last-child {
            margin-right: 0;
          }

          &:hover {
            color: #95a9ad;
          }
        }
      }
    }
  }
</style>
