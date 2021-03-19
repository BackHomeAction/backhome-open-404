<template>
  <div class="body">
    <div class="background">
      <div class="top" />
    </div>
    <div class="header">
      <div
        class="text text---right"
      >
        <div class="text__row text__title">
          Oops! 404
        </div>
        <div class="text__row">
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

    <Footer />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, computed } from 'vue'
import Content from '@/components/Content.vue'
import Selector from '@/components/Selector.vue'
import Footer from '@/components/Footer.vue'
import request from '@/utils/request'
import { enquireScreen } from 'enquire-js'

const oldManList = ref([])
const selectedIndex = ref(0)

export default defineComponent({
  components: { Content, Selector, Footer },
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

      // check if mobile
      enquireScreen((bool: boolean) => {
        if (bool) {
          if (oldManList.value.length === 5) {
            oldManList.value = oldManList.value.slice(1, 4)
            selectedIndex.value = Math.floor(oldManList.value.length / 2)
          }
        }
      }, 'only screen and (max-width: 767.99px)')
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

    &--right {
      text-align: right;
    }

    &__title {
      font-size: 68px;
      font-weight: bold;
    }
  }
}

.content {
  margin-top: 10px;
}

.selector {
  margin-top: 15px;
}

@media screen and (max-width: 1600px) {
  .background {
    .top {
      background-size: auto 10vh;
      height: calc(10vh - 1px);
    }
  }

  .header {
    margin-top: 14vh;
  }

  .content {
    margin-top: 8px;
  }

  .selector {
    margin-top: 10px;
  }
}

@media screen and (max-width: 767.99px) {
  .background {
    height: 50vh;
    clip-path: circle(250vw at 50vw -180vw);

    .top {
      background-size: auto 6vh;
      height: calc(6vh - 1px);
    }
  }

  .header {
    width: 90vw;
    margin-top: 10vh;

    .text {
      font-size: 4.5vw;
      line-height: 27px;

      &__title {
        font-size: 6vw;
      }
    }
  }

  .content {
    margin-top: 8px;
  }

  .selector {
    margin-top: 10px;
  }
}
</style>
