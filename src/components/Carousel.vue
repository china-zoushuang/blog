<template>
  <div class="carousel">
    <div class="carousel__list-wrapper">
      <div
        :style="{ transform: `translateX(-${activeIndex * 100}%)` }"
        class="carousel__list">
        <slot></slot>
      </div>
    </div>
    <div
      v-if="slick === 'arrow' && list.length > range"
      class="carousel__arrow">
      <span
        class="carousel_arrow__prev"
        @click="handleTab(activeIndex - 1)">
      </span>
      <span
        class="carousel_arrow__next"
        @click="handleTab(activeIndex + 1)">
      </span>
    </div>
    <div
      v-if="slick === 'buttons' && list.length > 1"
      class="carousel__buttons">
      <span
        :class="{ disabled: activeIndex === 0}"
        class="carousel_buttons__prev"
        @click="handleTab(activeIndex - 1)">
      </span>
      <span
        :class="{ disabled: activeIndex === list.length - 1}"
        class="carousel_buttons__next"
        @click="handleTab(activeIndex + 1)">
      </span>
    </div>
    <div
      v-if="slick === 'dots'"
      class="carousel__dots">
      <span
        v-for="(item, index) in list"
        :key="index"
        :class="{ 'carousel_dots--active': activeIndex === index }"
        @click="handleTab(index)">
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    list: {
      type: Array,
      required: true,
    },
    slick: {
      type: String,
      required: true,
    },
    range: {
      type: Number,
    },
    activeIndex: {
      type: Number,
    },
  },
  methods: {
    handleTab(index) {
      const listLen = this.list.length / (this.range || 1);
      if (index > -1 && index < listLen) {
        this.$emit('change', index);
      }
    },
  },
};
</script>

<style lang="less" scpoed>
  .carousel {
    position: relative;

    .carousel__list-wrapper {

      .carousel__list {
        display: flex;
        transition: all .3s;

        & > *{
          flex-shrink: 0;
          transition: all .3s;
        }
      }
    }

    .carousel__arrow {
      position: absolute;
      top: 50%;
      width: 100%;
      transform: translateY(-50%);

      .carousel_arrow__prev,
      .carousel_arrow__next {
        position: absolute;
        top: 50%;
        display: block;
        width: 40px;
        height: 40px;
        cursor: pointer;
        transform: translateY(-50%) rotate(45deg);
        transition: all .3s;

        &:hover {
          transform: translateY(-50%) rotate(45deg) scale(1.2);
        }
      }

      .carousel_arrow__prev {
        left: -80px;
        border-left: 2px solid #000;
        border-bottom: 2px solid #000;
      }

      .carousel_arrow__next {
        right: -80px;
        border-top: 2px solid #000;
        border-right: 2px solid #000;
      }
    }

    .carousel__buttons {
      display: flex;
      justify-content: flex-end;

      .carousel_buttons__prev,
      .carousel_buttons__next {
        display: block;
        width: 40px;
        height: 40px;
        background: #fb3856 url('~@/assets/icons/arrowOutline.svg') center center/ 50% auto no-repeat;
        cursor: pointer;

        &:hover {
          background-color: #f76077;
        }
      }

      .carousel_buttons__prev {
        margin-right: 1px;
      }
      .carousel_buttons__next {
        transform: rotate(180deg);
      }

      .disabled {
        cursor: not-allowed;
        opacity: .5;
      }
    }

    .carousel__dots {
      display: flex;
      justify-content: center;
      padding-top: 50px;

      span {
        position: relative;
        display: block;
        width: 12px;
        height: 20px;
        margin-right: 5px;
        cursor: pointer;

        &::after {
          content: '';
          position: absolute;
          top: 50%;
          left: 0;
          right: 0;
          display: block;
          width: 100%;
          height: 2px;
          background-color: #797979;
          transform: translateY(-50%);
        }

        &:last-child {
          margin-right: 0;
        }
      }

      .carousel_dots--active {

        &::after {
          background-color: #fff;
        }
      }
    }
  }
</style>
