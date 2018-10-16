<template>
	<div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" :key="item.id" class="menu-item" :class="{'current':currentIndex===index}">
					<span class="text border-1px">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul>
				<li v-for="item in goods" :key="item.id" class="food-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" :key="food.id" class="food-item">
							<div class="icon">
								<img width="57" height="57" :src="food.icon" alt="">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span class="count">月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
								</div>
							</div>	
						</li>
					</ul>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
			goods: [],
			listHeight:[], // 用来存放右侧每一个分类/li的商品高度
			scrollY:0
    };
	},
	computed:{
		// 计算右侧的高度来决定左边哪个分类选中
		currentIndex(){
			for(let i=0;i<this.listHeight.length;i++){
				// 这两个高度就是1个li从上到下的整个高度
				let height1=this.listHeight[i]  // 获取当前的上高度
				let height2=this.listHeight[i+1]  // 获取当前的下高度
				if(!height2||(this.scrollY>height1&&this.scrollY<height2)){
					return i  // 返回当前索引
				}
			}
			return 0 
		}
	},
  created() {
		this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
    this.$http.get("/api/goods").then(res => {
      res = JSON.parse(res.request.response);
      if (res.code == 0) {
        console.log(res);
				this.goods = res.data;
				// created钩子中DOM没有渲染,所以需要$nextTick来驱动
				this.$nextTick(()=>{
					// 商品列表渲染完成之后初始化better-scroll
					this._initScroll()
					this._calculateHeight()
				})
			
      }
    });
    
	},
	methods:{
		// 初始化滚动组件
		_initScroll(){
			this.menuScroll=new BScroll(this.$refs.menuWrapper,{})
			this.foodsScroll=new BScroll(this.$refs.foodsWrapper,{
				 probeType: 3 // 获取实时滚动的位置，详见bscroll API
			})
			this.foodsScroll.on('scroll',(pos)=>{
				this.scrollY=Math.abs(Math.random(pos.y))
			})
		},
		_calculateHeight(){
			// 获取右侧一个商品的整个li，一个li里面包含了该分类下的所有商品
			let foodList=this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
			let height=0
			this.listHeight.push(height)
			for(let i=0;i<foodList.length;i++){
				let item=foodList[i]
				height+=item.clientHeight
				this.listHeight.push(height)
			}
		}
	}
};
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl'
.goods
	display flex
	position absolute
	top 174px
	bottom 46px
	width 100%
	overflow hidden
	.menu-wrapper
		flex 0 0 80px
		width 80px
		background #f3f5f7
		.menu-item
			display table // 用于垂直居中
			width 56px
			height 54px
			padding 0 12px
			line-height 14px
			&.current
				position relative
				z-index 10
				margin-top -1px
				background #fff
				font-weight 700
				.text
					border-none()
			.icon
				display inline-block
				vertical-align top
				width 12px
				height 12px
				margin-right 2px
				background-size 12px 12px
				background-repeat no-repeat
				&.decrease // 减
					bg-image('decrease_3')
				&.discount // 折
					bg-image('discount_3')
				&.guarantee // 套餐
					bg-image('guarantee_3')
				&.invoice // 票
					bg-image('invoice_3')
				&.special // 保
					bg-image('special_3')
			.text
				display table-cell
				width 56px
				vertical-align middle
				font-size 12px
				border-bottom 1px solid rgba(7, 17, 27, 0.1)
				// border-1px(rgba(7,17,27,0.1))	//TODO: 不知道为什么border-1px报错
	.foods-wrapper
		flex 1
		.title
			padding-left 14px
			height 26px
			line-height 26px
			color rgb(147, 153, 159)
			background-color #f3f5f7
		.food-item
			display flex
			margin 18px 
			padding-bottom 18px
			border-bottom 1px solid rgba(7, 17, 27, 0.1)
			// border-1px(rgba(7,17,27,0.1))	//TODO: 不知道为什么border-1px报错
			&:last-child
				border-none()
				margin-bottom 0
			.icon
				flex 0 0 57px
				margin-right 10px
			.content
				flex  1
				.name 
					margin 2px 0 8px 0
					height 14px
					line-height 14px 
					font-size 14px 
					color rgb(7,17,27)
				.desc,.extra
					line-height 10px
					font-size 10px
					color rgb(147,153,159)
				.desc
					margin-bottom 8px
					line-height 12px
				.extra
					 .count
						margin-right 12px
				.price
					font-weight 700
					line-height 24px
					.now 
						margin-right 8px
						font-size 14px
						color rgb(240,20,20)
					.old
						text-decoration line-through 
						font-size 10px
						color rgb(147,153,159)
					
</style>


