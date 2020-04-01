<template>
  <div class="nav">
    <h1 class="nav__title">ZouShuang</h1>
    <ul class="nav__list">
      <li
        v-for="(value, key) in source"
        :key="key"
        :class="{ 'nav_list__item--active': key === activeNav }"
        class="nav_list__item"
        @click="handleJump(key)">
        <a href="javascript:;">{{ value.title }}</a>
      </li>
    </ul>
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
  data() {
    return {
      activeNav: 'about',
      lastScrollTop: 0,
    };
  },
  mounted() {
    document.addEventListener('scroll', this.toggle, true);
  },
  beforeDestroy() {
    document.removeEventListener('scroll', this.toggle, true);
  },
  methods: {
    handleJump(key) {
      this.activeNav = key;
      const dom = document.getElementById(key);
      document.documentElement.scrollTop = dom.offsetTop;
    },
    toggle() {
      const scrollTop = window.pageYOffset || document.body.scrollTop || document.documentElement.scrollTop;
      if (scrollTop > window.innerHeight && scrollTop > this.lastScrollTop) {
        this.$el.style.top = '-60px';
        this.$el.style.opacity = '.5';
      } else {
        this.$el.style.top = '0px';
        if (scrollTop) {
          this.$el.style.opacity = '.5';
        } else {
          this.$el.style.opacity = '1';
        }
      }
      this.lastScrollTop = scrollTop;
    },
  },
};
</script>

<style lang="less" scoped>
  .nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
    display: flex;
    justify-content: space-between;
    width: 100%;
    padding: 0 80px;
    box-sizing: border-box;
    background-color: rgba(35, 35, 37, .5);
    transition: all .4s;

    .nav__title {
      line-height: 50px;
      font-size: 28px;
      color: #fff;
    }

    .nav__list {
      display: flex;

      .nav_list__item {
        margin-right: 20px;
        line-height: 50px;

        &:last-child {
          margin-right: 0;
        }

        a {
          color: #797979;
          text-decoration: none;
          transition: all .3s;

          &:hover {
            color: #95a9ad;
          }
        }
      }

      .nav_list__item--active {

        a {
          color: #fff;
          text-decoration: line-through;

          &:hover {
            color: #fff;
          }
        }
      }
    }
  }
</style>
