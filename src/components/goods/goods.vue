<template>
	<div class="goods">
		<div class="menu-wrapper">
			<ul>
				<li v-for="item in goods" :key="item.id" class="menu-item">
					<span class="text border-1px">
						<span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper">
			<ul>
				<li v-for="item in goods" :key="item.id" class="food-list">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" :key="food.id" class="food-item">
							<div class="icon">
								<img :src="food.icon" alt="">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span>月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span>￥{{food.price}}</span>
									<span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
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
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: []
    };
  },
  created() {
    this.$http.get("/api/goods").then(res => {
      res = JSON.parse(res.request.response);
      if (res.code == 0) {
        console.log(res);
        this.goods = res.data;
      }
    });
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
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
</style>


