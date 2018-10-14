<template>
	<div class="star" :class="starType">
		<span v-for="itemClass in itemClasses" :key="itemClass.id"
		:class="itemClass" class="star-item"></span>
	</div>
</template>

<script>
const length=5
const class_on='on'
const class_half='half'
const class_off='off'
export default {
	//传入尺寸和分数
	props:{
		size:{
			type:Number
		},
		score:{
			type:Number
		}
	},

	computed:{
		starType(){
			return 'star-'+this.size
		},
		itemClasses(){
			let result=[]
			let score=Math.floor(this.score*2)/2
			console.log(score);
			
			// 检测是否有小数，控制半星
			let hasDecimal=score%1!==0
			console.log(hasDecimal);
			
			// 整数，即有多少个星
			let integer=Math.floor(score)
			for(let i=0;i<integer;i++){
				result.push(class_on)
			}
			// 有小数加半星
			if(hasDecimal){
				result.push(class_half)
			}
			// 前面都加完之后数组长度还不到，就补齐空星
			while (result.length<length) {
				result.push(class_off)			
			}
			return result
		}
	}
}
</script>

<style lang="stylus">
@import '../../common/stylus/mixin.styl';
	.star 
		font-size 0
		.star-item
			display inline-block
			background-repeat no-repeat
		&.star-48
			.star-item
				width 20px
				height 20px
				margin-right  20px
				background-size 20px 20px
				&.on
					bg-image('star48_on')
				&.half
					bg-image('star48_half')
				&.off
					bg-image('star48_off')					
		&.star-36
			.star-item
				width 15px
				height 15px
				margin-right 6px
				background-size 15px 15px
				&.on
					bg-image('star36_on')
				&.half
					bg-image('star36_half')
				&.off
					bg-image('star36_off')
		&.star-24	
			.star-item
				width 10px
				height 10px
				margin-right 3px
				background-size 10px 10px
				&.on
					bg-image('star24_on')
				&.half
					bg-image('star24_half')
				&.off
					bg-image('star24_off')
</style>

