<template>
  <div class="selector">
    <div
      v-for="(item, index) in list"
      :key="item.id"
      class="item"
      :class="{['item--selected']: modelValue === index}"
      @click="handleClickItem(index)"
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
  emits: ['update:modelValue'],
  setup (props, { emit }) {
    function handleClickItem (id: number) {
      emit('update:modelValue', id)
    }
    return { handleClickItem }
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
  border-radius: 100%;
  width: 104px;
  height: 104px;
  user-select: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;

  &__image {
    width: 100px;
    height: 100px;
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.5);
    border-radius: 100%;
    object-fit: cover;
  }

  &--selected &__image {
    border: 4px solid #799351;
  }
}

@media screen and (max-width: 1600px) {
  .selector {
    width: 700px;
  }

  .item {
    width: 84px;
    height: 84px;

    &__image {
      width: 80px;
      height: 80px;
    }
  }
}

@media screen and (max-width: 767.99px) {
  .selector {
    width: 85vw;
  }

  .item {
    width: 64px;
    height: 64px;

    &__image {
      width: 60px;
      height: 60px;
    }
  }
}
</style>
