<template>
  <span class="dice" :class="diceClass"></span>
</template>

<script>
export default {
  name: 'Dice',
  props: ['value'],
  computed: {
    diceClass() {
      return `dice-${this.value}`
    }
  }
}
</script>

<style>

.dice {
  display: inline-block;
  min-height: 1em;
  padding-left: 1em;
  background-size: 1em;
  background-repeat: no-repeat;
}

.dice-1 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/1_nac7yd.png");
}

.dice-2 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/2_wbp6m7.png");
}

.dice-3 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/3_j5xrd6.png");
}

.dice-4 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/4_qqljux.png");
}

.dice-5 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/5_xb0qaj.png");
}

.dice-6 {
  background-image: url("https://res.cloudinary.com/dyj4k9tr0/image/upload/v1635872875/6_ctovod.png");
}
</style>