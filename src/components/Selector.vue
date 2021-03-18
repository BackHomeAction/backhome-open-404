<template>
  <div class="selector">
    <div
      v-for="item in list"
      :key="item.id"
      class="item"
    >
      <img
        class="item__image"
        :src="item.oldMan.identificationPhoto"
      >
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    list: {
      type: Array,
      default: () => { return [] }
    },
    modelValue: {
      type: Number,
      default: 0
    }
  },
  setup () {
    return {}
  }
})
</script>

<style lang="scss" scoped>
.selector {
  width: 900px;
  display: flex;
  justify-content: space-between;
}

.item {
  &__image {
    width: 100px;
    height: 100px;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.5);
    border-radius: 100%;
    object-fit: cover;
  }
}
</style>
