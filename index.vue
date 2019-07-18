<style scope>
    #cityFullScreen{
        height: 100%;
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        overflow: auto;
    }
 
    .van-nav-bar .van-icon{
        color: black;
        font-weight: 600;
    }
    .item_detail{
        min-height: 100vh;
        background-color: #fff;
    }
    .selectCity_title {
        margin: 10pt 10pt;
        padding: 10pt 0;
        border-bottom: 1px solid #ccc;
    }
    .selectCity_title img {
        width: 11pt;
        height: 12pt;
        vertical-align: middle;
        position: relative;
        top: -1pt;
    }
    .selectCity_alphabet {
        position: fixed;
        top: 25%;
        right: 10px;
    }
    .city_item_list {
        height: 40px;
        line-height: 40px;
        background-color: #ccc;
    }
    .fastSelActive {
        color: #F28300;
    }
</style>
<template>
    <div class="item_detail" id="cityFullScreen" ref="cityFullScreen">
        <div class="selectCity_title">
            <img src="../../assets/images/shop/ic_gps@2x.png" alt="">
            <span style="margin-left: 10pt;font-size: 12pt;color:rgba(0, 0, 0, 1);">武汉</span>
            <span style="margin-left: 10pt;font-size: 9pt;color:rgba(0, 0, 0, 0.5);">GPS定位</span>
        </div>
        <div>
            <div :key="index" v-for="(item, index) in cityList" ref="allCityBar">
                <div :id="item.id" class="city_item_list">{{item.name}}</div>
                <div v-for="(list, key) in item.content" :key="key">
                    <div style="height: 40px;line-height: 40px;">
                        {{list.name}}
                    </div>
                </div>
            </div>
        </div>
        <div class="selectCity_alphabet">
            <ul v-for="(item, index) in leftNav" :key="index">
                <li :class="index === curCityListIndex ? 'fastSelActive': ''" @click="goAnchor(item)">{{item.name}}</li>
            </ul>
        </div>
    </div>
</template>
<script>
  export default {
    name: 'city',
    data() {
      return {
          cityList: [
              {
                  name: '标题A',
                  id: 'A',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              },
              {
                  name: '标题B',
                  id: 'B',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              },
              {
                  name: '标题C',
                  id: 'C',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              },
              {
                  name: '标题D',
                  id: 'D',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              },
              {
                  name: '标题E',
                  id: 'E',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              },
              {
                  name: '标题F',
                  id: 'F',
                  content: [
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      },
                      {
                          name: '文本'
                      }
                  ]
              }
          ],  // 所有城市
          curCityListIndex: 0, // 点击右边导航栏选中
          leftNav: [
              {
                  name: 'A',
                  id:0
              },
              {
                  name: 'B',
                  id:1
              },
              {
                  name: 'C',
                  id:2
              },
              {
                  name: 'D',
                  id:3
              },
              {
                  name: 'E',
                  id:4
              },
              {
                  name: 'F',
                  id:5
              },
          ], // 右边导航栏数据
          listHeight: [], // 每组城市的高度
          scrollY: '' // 滑动
      }
    },
    created() {
        // this.calculateHeight();
    },
    mounted() {
      this.calculateHeight()
      this.$refs.cityFullScreen.addEventListener('scroll', this.handleScroll, true);  // 监听（绑定）滚轮滚动事件
    },
    watch: {
    },
    destroyed: function () {
        this.$refs.cityFullScreen.removeEventListener('scroll', this.handleScroll);   //  离开页面清除（移除）滚轮滚动事件
    },
    methods: {
        handleScroll () {
            // console.log(this.$refs.cityFullScreen.scrollTop)
            this.scrollY = this.$refs.cityFullScreen.scrollTop
            // console.log(this.scrollY)
            const listHeight = this.listHeight
            // 当滚动到顶部
            if (this.scrollY < 0) {
                this.curCityListIndex = 0
                return;
            }
            // 在中间部分滚动
            for (let i = 0; i < listHeight.length - 1; i++) {
                let height = listHeight[i]
                let height2 = listHeight[i + 1]
                // 思路 拿数组里面的两个值进行范围比较如  0 - 300 是A  300-600 是B
                if (this.scrollY >= height && this.scrollY < height2) {
                    // console.log('-----------合格----------')
                    // console.log(listHeight[i])
                    this.curCityListIndex = i
                }
            }
        },
        // 右边导航栏点击
        goAnchor(type) {
            // console.log(type)
            this.curCityListIndex = type.id
            document.querySelector("#" + type.name).scrollIntoView(true);
        },
       // 计算每个城市区块的高度
        calculateHeight() {
            this.$nextTick(() => {
                let cityList = this.$refs.allCityBar
                let height = 0;
                this.listHeight.push(height);
                for (let i = 0; i < cityList.length; i++) {
                    let item = cityList[i];
                    height += item.clientHeight; // 可见高度
                    this.listHeight.push(height);
                }
                console.log(this.listHeight)
            });
        },
    }
  }
</script>