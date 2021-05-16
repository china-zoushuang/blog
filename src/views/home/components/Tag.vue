<template>
  <section class="container">
    <div class="sidebar">
      <h2 class="sidebar__title">tags</h2>
      <div class="sidebar__content">
        <ul class="sidebar_content__list">
          <li
            v-for="(item, index) in tags"
            :key="index"
            :class="{'sidebar_content_list__item--active': activeTag === item}"
            class="sidebar_content_list__item"
            @click="activeTag = item">
            {{ item }}
          </li>
        </ul>
      </div>
    </div>
    <div class="main">
      <div class="main__content">
        <ul class="main_content__list">
          <li
            v-for="(item, index) in filteredList"
            :key="index"
            class="main_content_list__item">
            <div class="main_content_list_item__cover">
              <img
                :src="require(`@/assets/images/${item.coverImage}`)"
                :alt="item.coverImage">
            </div>
            <div class="main_content_list_item__desc">
              <strong class="main_content_list_item_desc__title">
                {{ item.title }}
              </strong>
              <p
                v-html="item.description"
                class="main_content_list_item_desc__text">
              </p>
              <div class="main_content_list_item_desc__extra">
                <span>{{ item.date }}</span>
                <span
                  v-for="(childItem, childIndex) in item.tags"
                  :key="childIndex">
                  {{ `#${childItem}` }}
                </span>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    source: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      activeTag: '#',
    };
  },
  computed: {
    rawData() {
      return Object.values(this.source);
    },
    tags() {
      const newData = ['#'];
      for (const item of this.rawData) {
        for (const childItem of item.data) {
          newData.push(...childItem.tags);
        }
      }
      return [...new Set(newData)];
    },
    filteredList() {
      const newData = [];
      for (const item of this.rawData) {
        for (const childItem of item.data) {
          if (this.activeTag === '#') {
            newData.push(childItem);
          } else if (childItem.tags.indexOf(this.activeTag) !== -1) {
            newData.push(childItem);
          }
        }
      }
      return newData;
    },
  },
  mounted() {
    window.onscroll = () => {
      const scrollTop = window.pageYOffset || document.body.scrollTop || document.documentElement.scrollTop;
      const { offsetTop } = document.querySelector('#tag');
      if (scrollTop > offsetTop) {
        document.querySelector('.sidebar').style.position = 'fixed';
      } else {
        document.querySelector('.sidebar').style.position = 'absolute';
      }
    };
  },
};
</script>

<style lang="less" scoped>
  .container {
    position: relative;
    padding: 50px;
    text-align: left;

    .sidebar {
      position: absolute;
      top: 50px;
      left: 50px;
      bottom: 50px;
      width: 200px;

      .sidebar__title {
        padding-bottom: 20px;
        margin: 0;
        line-height: 32px;
        font-size: 32px;
        font-weight: normal;
      }

      .sidebar__content {
        height: calc(100% - 50px);

        .sidebar_content__list {
          width: 150px;
          height: 100%;
          overflow: auto;

          .sidebar_content_list__item {
            line-height: 28px;
            font-size: 14px;
            color: rgb(97, 97, 97);
            cursor: pointer;

            &:hover {
            color: #cccccc;
            }
          }

          .sidebar_content_list__item--active {
            color: #fb3856;

            &:hover {
              color: #fb3856;
            }
          }
        }
      }
    }

    .main {
      padding-left: 200px;

      .main__content {

        .main_content__list {

          .main_content_list__item {
            display: flex;
            padding-bottom: 20px;
            border-bottom: 1px solid #e1e1e1;
            margin-bottom: 20px;

            .main_content_list_item__cover {
              flex-shrink: 0;
              width: 100px;
              margin-right: 40px;

              img {
                width: 100%;
              }
            }

            .main_content_list_item__desc {

              .main_content_list_item_desc__title {

              }

              .main_content_list_item_desc__text {
                font-size: 14px;
                line-height: 24px;
              }

              .main_content_list_item_desc__extra {
                display: flex;
                font-size: 12px;
                color: #b3b3b3;

                span {
                  display: block;
                  padding-right: 12px;
                }
              }
            }
          }
        }
      }
    }
  }
</style>
