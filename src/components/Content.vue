<template>
  <div class="content">
    <div class="left">
      <img
        :src="data.oldMan.identificationPhoto"
        class="photo"
      >
    </div>
    <div class="right">
      <div class="text text__name">
        {{ data.oldMan.name }}
        <span v-if="data.oldMan.sex">（{{ data.oldMan.sex === 1 ? '男' : '女' }}）</span>
      </div>
      <div class="text">
        出生日期：{{ data.oldMan.birthDate ? getFormattedDate(data.oldMan.birthDate) : '暂无' }}
      </div>
      <div class="text">
        走失日期：{{ getFormattedDate(data.oldMan.lostTime) }}
      </div>
      <div class="text">
        走失地点：{{ `${data.province}${data.city}${data.district} ${data.place}` }}
      </div>
      <div class="text">
        常去地点：{{ offenPlaceText ? offenPlaceText : '暂无' }}
      </div>
      <div class="text">
        走失人特征：{{ featureText ? featureText : '暂无' }}
      </div>
      <div class="text">
        家属联系方式：{{ data.oldMan.phone ? data.oldMan.phone : '暂无' }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue'
import dayjs from 'dayjs'

function getFormattedDate (value: string) {
  return dayjs(value).format('YYYY年MM月DD日')
}

export default defineComponent({
  props: {
    data: {
      type: Object,
      default: null
    }
  },
  setup (props) {
    const offenPlaceText = computed(() => {
      if (!props.data.oldMan.offerPlace) return ''
      let text = ''
      const list = JSON.parse(props.data.oldMan.offerPlace)
      // eslint-disable-next-line @typescript-eslint/no-explicit-any
      list.map((ele: any, index: number) => {
        if (index !== list.length - 1) {
          text += `${ele.name}、`
        } else {
          text += ele.name
        }
      })

      return text
    })

    const featureText = computed(() => {
      let text = ''

      if (props.data.oldMan.height) {
        text += `身高${props.data.oldMan.height}cm，`
      }
      if (props.data.oldMan.weight) {
        text += `体重${props.data.oldMan.weight}kg，`
      }
      if (props.data.oldMan.senileDementia === 2) {
        text += '患有老年痴呆，'
      }
      if (props.data.oldMan.others) {
        text += `${props.data.oldMan.others}，`
      }

      return text.substring(0, text.lastIndexOf('，'))
    })

    return { getFormattedDate, offenPlaceText, featureText }
  }
})
</script>

<style lang="scss" scoped>
.content {
  width: 700px;
  min-height: 250px;
  background: #FFFFFF;
  border-radius: 25px;
  box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.5);
  padding: 30px;
  box-sizing: border-box;
  display: flex;
}

.left {
  .photo {
    width: 180px;
    height: 240px;
    border-radius: 20px;
    object-fit: cover;
  }
}

.right {
  margin-left: 40px;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px 0;

  .text {
    font-size: 16px;
    font-weight: 400;
    color: #555555;
    line-height: 22px;

    &__name {
      font-size: 18px;
      font-weight: 500;
      color: #000000;
      line-height: 26px;
    }
  }
}
</style>
