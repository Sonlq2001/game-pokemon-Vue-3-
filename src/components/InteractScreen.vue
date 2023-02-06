<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{
          index,
          value: card,
        }"
        @onFlip="checkRule($event)"
        :cardsContext="cardsContext"
        :rules="rules"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
  name: "InteractScreen",
  components: { CardFlip },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      // trường hợp mảng chứa hơn 2 phần tử thì loại bỏ, chỉ cần 2 quân bài để kiểm tra so sánh với nhau
      if (this.rules.length === 2) {
        return false;
      }
      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        // add class 'disabled' to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();

        // reset rules to []
        this.rules = [];

        // check completed game
        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );

        // querySelectAll đang lấy được các bản ghi vì đang có 1 khoảng thời gian settimeout, chưa kịp set thì đã gọi rồi
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          // close two card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

          // reset rules to []
          this.rules = [];
        }, 1000);
      } else {
        return false;
      }
    },
  },
};
</script>

<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  background-color: var(--dark);
}

.screen__inner {
  margin: 2rem auto;
  display: flex;
  flex-wrap: wrap;
}
</style>
