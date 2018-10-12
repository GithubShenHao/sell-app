<template>
	<div class="header">
		<div class="content-wrapper">
			<!-- 商家头像 -->
			<div class="avatar">
				<img :src="seller.avatar" width="64" height="64">
			</div>
			<div class="content">
				<!-- 商家名 -->
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<!-- 描述和配送时间 -->
				<div class="description">
				{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<!-- 使用v-if是因为初始化时seller为空 -->
				<div v-if="seller.supports" class="support">
					<!-- 获取第一条活动数据 -->
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<!-- 活动个数按钮 -->
			<div v-if="seller.supports" class="support-count" @click="shouDetail">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<!-- 公告 -->
		<div class="bulletin-wrapper" @click="shouDetail">
			<span class="bulletin-title"></span>
			<span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>			
		</div>
		<!-- 头部模糊背景图 -->
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<!-- 公告弹层 -->
		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix" >
				<div class="detail-main">
					<h1 class="name">{{seller.name}}</h1>

				</div>
			</div>
			<div class="detail-close">
				<i class="icon-close"></i>
			</div>
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
  data(){
	return {
		detailShow:false
	}
  },
  methods:{
	  shouDetail(){
		  this.detailShow=true
	  }
  },
  created() {
	this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
};
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl'
.header
	position relative // 这里作为背景图的定位
	overflow hidden
	color #fff
	background rgba(7, 17, 27, 0.5)
	// background: #666
	.content-wrapper // 外层
		position relative
		padding 24px 12px 18px 24px
		font-size 0
		.avatar // 头像
			display inline-block
			vertical-align top
			img
				border-radius 2px
		.content // 头像右侧内容
			display inline-block
			margin-left 16px
			.title // 商家名
				margin 2px 0 8px 0
				.brand // 商家图标
					display inline-block
					width 30px
					height 18px
					bg-image('brand')
					background-size 30px 18px
					background-repeat no-repeat
					vertical-align top
				.name // 
					margin-left 6px
					font-size 16px
					line-height 18px
					font-weight bold
			.description // 描述
				margin-bottom 10px
				line-height 12px
				font-size 12px
			.support // 优惠
				.icon
					display inline-block
					vertical-align top
					width 12px
					height 12px
					margin-right 4px
					background-size 12px 12px
					background-repeat no-repeat
					&.decrease // 减
						bg-image('decrease_1')
					&.discount // 折
						bg-image('discount_1')
					&.guarantee // 套餐
						bg-image('guarantee_1')
					&.invoice // 票
						bg-image('invoice_1')
					&.special // 保
						bg-image('special_1')
				.text // 活动文字
					line-height 12px
					font-size 10px
					font-weight 100
		.support-count // 右边活动个数
			position absolute
			right 12px
			bottom 14px
			padding 0 8px
			height 24px
			line-height 24px
			border-radius 14px
			background rgba(0, 0, 0, 0.2)
			text-align center
			.count
				vertical-align top
				line-height 24px
				font-size 12px
			.icon-keyboard_arrow_right
				vertical-align top
				margin-left 2px
				line-height 24px
				font-size 12px
	.bulletin-wrapper // 公告
		position relative
		height 28px
		line-height 28px
		padding 0 22px 0 12px
		white-space nowrap
		overflow hidden
		text-overflow ellipsis
		background rgba(7, 17, 27, 0.2)
		.bulletin-title // 公告图片
			display inline-block
			vertical-align top
			margin-top 8px
			width 22px
			height 12px
			bg-image('bulletin')
			background-size 22px 12px
			background-repeat no-repeat
		.bulletin-text
			vertical-align top
			margin 0 4px
			font-size 10px
		.icon-keyboard_arrow_right
			position absolute
			font-size 10px
			right 12px
			top 8px
	.background // 背景图片
		position absolute
		top 0
		left 0
		width 100%
		height 100%
		z-index -1
		// 设置背景图片模糊
		filter blur(10px)
	.detail //公告弹层
		position fixed
		z-index 100
		top 0 
		left 0
		width 100%
		height 100%
		overflow auto
		background-color rgba(7,17,27,0.8)
		.detail-wrapper
			width 100%
			min-height 100%
			.detail-main
				margin-top 64px
				paddgin-bottom 64px
				.name
					line-height 16px
					text-align center
					font-size 16px
					font-weight 700
		.detail-close
			position relative
			width 32px
			height 32px
			margin -64px auto 0 auto 
			clear both 
			font-size 32px
</style>


