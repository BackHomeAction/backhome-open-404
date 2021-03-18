<template>
  <div class="body">
    <div class="background">
      <div class="top" />
    </div>
    <div class="header">
      <div
        class="text"
      >
        <div
          class="text__row"
          style="font-size: 68px; text-align: right; font-weight: bold"
        >
          Oops! 404
        </div>
        <div
          class="text__row"
          style="font-size: 40px; text-align: right;"
        >
          Not Found…
        </div>
      </div>
      <div class="text">
        <div class="text__row">
          您访问的页面找不回来了，
        </div>
        <div class="text__row">
          但我们可以一起帮他们回家！
        </div>
      </div>
    </div>
    <div
      v-if="selectedOldMan"
      class="content"
    >
      <Content :data="selectedOldMan" />
    </div>
    <div
      v-if="oldManList && oldManList.length"
      class="selector"
    >
      <Selector
        v-model="selectedIndex"
        :list="oldManList"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, computed } from 'vue'
import Content from '@/components/Content.vue'
import Selector from '@/components/Selector.vue'
import request from '@/utils/request'

const oldManList = ref([])
const selectedIndex = ref(0)

export default defineComponent({
  components: { Content, Selector },
  setup () {
    onMounted(async () => {
      try {
        // eslint-disable-next-line @typescript-eslint/no-explicit-any
        const res: any = await request.get('https://fwwb2020-app-volunteer.tgucsdn.com/thirdParty/notFound', {
          searchParams: {
            count: 5
          }
        }).json()
        oldManList.value = res.data
        selectedIndex.value = Math.floor(res.data.length / 2)
        console.log(res)
      } catch (e) {
        console.log(e)
      }
    })

    const selectedOldMan = computed(() => {
      return oldManList.value[selectedIndex.value]
    })

    return { oldManList, selectedIndex, selectedOldMan }
  }
})
</script>

<style lang="scss" scoped>
@import url("./assets/fonts/DFWaWaW5_1616057382456/DFWaWaW5.css");

.body {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  height: 70vh;
  clip-path: circle(250vw at 50vw -220vw);
  z-index: -1;
  background: #30475D;

  .top {
    background-image: url("./assets/images/top.png");
    background-position: center center;
    background-repeat: repeat-x;
    background-size: auto 12vh;
    width: 100vw;
    height: calc(12vh - 1px);
  }
}

.header {
  margin-top: 16vh;
  display: flex;
  justify-content: space-between;
  width: 900px;
  user-select: none;

  .text {
    vertical-align: top;
    color: #ffffff;
    font-family: "DFWaWaW5";
    font-size: 40px;
    line-height: 60px;
    font-weight: normal;
  }
}

.content {
  margin-top: 10px;
}

.selector {
  margin-top: 15px;
}
</style>
