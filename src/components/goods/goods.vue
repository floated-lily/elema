<template>
  <div class="goods">
    <div class='menu-wrapper' ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
          <span class="text">
            <span v-show="item.type>0" class="icon"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class='foods-wrapper' ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="foodlist food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img :src="food.icon" width='57px'>
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">${{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">${{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartControl/cartControl';

  const ERR_OK = 0;

  export default{
    props:{
      seller:{
        type:Object
      }
    },
    data () {
      return {
        goods:[],
        listHeight:[],
        scrollY:0
      };
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;

        },
      selectFoods(){
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) =>{
            if(food.count){
              foods.push(food);
            }
          })
        })
        return foods
      }
   },
    created() {
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(()=>{
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
    },
    methods:{
      //点击事件
      selectMenu(index, event){
        //判断是否为移动端点击，否则pc会有2次事件。
        if (event._constructed) {
          //获取到foods的数组
          let foodList = document.getElementsByClassName("food-list-hook")
          let el = foodList[index]
          //better-scroll的一个接口，直接滚动到一个特定的element，300毫秒
          this.foodScroll.scrollToElement(el, 300);
        }

      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper,{
          click:true
        });
        this.foodScroll = new BScroll(this.$refs.foodsWrapper,{
          click:true,
          probeType:3
        });

        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });

      },
      _calculateHeight(){
        let foodList = document.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for(let i = 0;i < foodList.length; i++){
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
      },
    components:{
      shopcart,
      cartcontrol
    }
  }
</script>

<style>
  .goods{position:absolute;top:174px;bottom:46px;display:flex;width:100%;overflow:hidden;}
  .goods .menu-wrapper{flex:0 0 80px;width:80px;background:#f3f5f7;}
  .goods .menu-item{display:table;height:54px;width:56px;line-height:14px;padding:0 12px;}
  .goods .menu-wrapper .current{position:relative;z-index:10;margin-top:-1px;background:#fff;font-weight:700;}
  .goods .icon{display:inline-block;width:12px;height:12px;flex:0 0 57px;margin-right:4px;background:url('../../../img/special.png') no-repeat center;background-size:12px 12px;}
  .goods .text{display:table-cell;width:56px;vertical-align:middle;font-size:12px;border-bottom:1px solid rgba(7,17,27,0.1);}
  .goods .foods-wrapper{flex:1;}
  .foods-wrapper .title{padding-left:14px;height:26px;line-height:26px;border-left:2px solid #d9dde1;font-size:12px;color:#93999f;background:#f3f5f7;}
  .foods-wrapper .food-item{position:relative;display:flex;margin:18px;padding-bottom:18px;border-bottom:1px solid rgba(7,17,27,0.1);}
  .foods-wrapper .food-item:last-child{border-bottom:none;margin-bottom:0}
  .goods .foods-wrapper .food-item .content .name{margin:2px 0 8px 0;height:14px;line-height:14px;font-size:14px;color:#07111b;}
  .goods .content .desc{padding-bottom:3px;}
  .goods .foods-wrapper .food-item .content .desc, .goods .foods-wrapper .food-item .content .extra{line-height:10px;font-size:10px;color:#93999f;}
  .goods .foods-wrapper .food-item .content .price{font-weight:700;line-height:24px;}
  .goods .foods-wrapper .food-item .content .price .now{margin-right:8px;font-size:14px;color:#f01414;}
  .goods .cartcontrol-wrapper{position:absolute;right:0px;bottom:12px;}
</style>