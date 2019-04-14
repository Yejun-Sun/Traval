<template>
    <ul class="list">
      <li class="item"
          v-for="item of letters"
          :key="item"
          :ref="item"
          @touchstart="handleTouchStart"
          @touchmove="handleTouchMove"
          @touchend="handleTouchEnd"
          @click="handleLetterClick"
      >
        {{ item }}
      </li>
    </ul>
</template>

<script>
    export default {
      name: "CityAlphabet",
      props:{
          cities: Object
      },
      data() {
        return{
          touchStatus: false,
          startY: 0,
          timer: null
        }
      },
      /*offsetTop 到上层控件的位置  此处为74*/
      updated() {
        this.startY = this.$refs['A'][0].offsetTop;
      },
      computed: {
        letters() {
          const letters = [];
          for (let i in this.cities) {
            letters.push(i)
          }
          return letters
        }
      },
      methods: {
        handleLetterClick(e) {
          this.$emit('change', e.target.innerText)
        },
        /*手指触摸事件触发开始*/
        handleTouchStart() {
          this.touchStatus = true;
        },
        /*手指触摸事件过程中*/
        handleTouchMove(e) {
          if (this.touchStatus) {
            /*防抖，减少 handleTouchMove 事件的触发频率
            * 如果触摸事件正被触发，就执行 setTimeout 中的函数，并延时16ms
            * 在此期间如果事件又被触发，则由if函数清除事件*/
            if (this.timer) {
              clearTimeout(this.timer)
            }
            this.timer = setTimeout(()=>{
              /*clientY 用户触摸为重到可视区顶部*/
              const touchY = e.touches[0].clientY - 79;
              const index = Math.floor((touchY - this.startY) / 20);
              if (index >= 0 && index < this.letters.length) {
                this.$emit('change',this.letters[index])
              }
            },16);
          }
        },
        /*手指触摸事件结束*/
        handleTouchEnd() {
          this.touchStatus = false
        }
      }
    }
</script>

<style lang="stylus" scoped>
  @import "~styles/varibles.styl"
.list
  display flex
  flex-direction column
  justify-content center
  position absolute
  top 1.58rem
  right 0
  bottom 0
  width .4rem
  .item
    text-align center
    line-height .4rem
    color $bgColor
</style>
