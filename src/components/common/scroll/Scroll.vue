<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  
  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      },
			data: {
				type: Array,
				default() {
					return []
				}
			}
    },
    
    data() {
      return {
        scroll: null
      }
    },
    methods: {
      scrollTo(x, y, time=1000) {
        this.scroll && this.scroll.scrollTo(x, y, time)
      },
      finishPullUp() {
        this.scroll && this.scroll.finishPullUp()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      }
    },
    mounted() {
      //1.创建BScroll对象
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      })
      
      //2.监听滚动的位置
      this.scroll.on('scroll', position => {
        this.$emit('scroll', position)
      })  
        
      //3.监听scroll滚动到底部
      this.scroll.on('pullingUp', () => {
        this.$emit('pullingUp')
      })
    },
		watch: {
			data() {
				setTimeout(this.refresh, 20)
			}
		}
  }
</script>

<style scoped>
  
   
</style>
