<template>
  <!--  -->
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    @click="onToggleFlipCard"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div class="card__inner" :class="{ 'is-flipped': isFlipped }">
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardFlip",
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    };
  },
  props: {
    imgBackFaceUrl: {
      type: String,
      require: true,
    },
    card: {
      type: [Number, String, Array, Object],
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
    rules: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  methods: {
    onToggleFlipCard() {
      /* 
        - khi 2 lá bài giống nhau không cho bấm nữa
        - khi 1 quân bài đã được lật, không cho bấm tiếp
        - khi 2 quân bài đã lật và không giống nhau, trong thời gian timeout 
        thì sẽ không bấm được quân bài khác
      */
      if (this.isDisabled || this.isFlipped || this.rules.length === 2) {
        return false;
      }

      this.isFlipped = !this.isFlipped;

      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onEnabledDisableMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .card__inner {
  cursor: default;
  pointer-events: none;
}

.card__inner.is-flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.card__face--front .card__content {
  background: url("./../assets/images/icon_back.png") no-repeat center center;
  width: 100%;
  height: 100%;
}

.card__face--back {
  transform: rotateY(-180deg);
  background-color: var(--light);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}
</style>
