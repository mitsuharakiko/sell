<template>
	<div class="cartcontrol">
		<transition name="move">
			<div class="cart-decrease" v-show="food.count > 0" @click="dcreaseCart($event)">
				<span class="inner">
					<i class="icon-remove_circle_outline"></i>
				</span>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
		<div class="cart-add">
			<i class="icon-add_circle" @click="addCart($event)"></i>
		</div>
	</div>
</template>
<script type="text/javascript">
	import Vue from 'vue';

	export default {
		props: {
			food: {
				type: Object
			}
		},
		methods: {
			addCart: function ($event) {
				if(!$event._constructed){
					return;
				}

				if (!this.food.count) {
					Vue.set(this.food,'count',1);
				} else {
					this.food.count += 1;
				}
			},
			dcreaseCart: function ($event) {
				if(!$event._constructed){
					return;
				}

				if (this.food.count) {
					this.food.count --;
				}
			}
		}
	};
</script>

<style lang="scss" scoped>
	.cartcontrol {
		font-size: 0;

		.cart-decrease {
			display: inline-block;
			padding: 0 6px;
			transition: all 0.4s linear;

			.inner {
				display: inline-block;
				font-size: 24px;
				line-height: 24px;
				color: rgb(0,160,220);
				transition: all 0.4s linear;
				transform: rotate(0);
			}
			
			&.move-enter,&.move-leave-active {
				opacity: 0;
				transform: translate3D(24px,0,0);
				.inner {
					transform: rotate(180deg);
				}
			}

			
		}

		.cart-count {
			display: inline-block;
			vertical-align: top;
			width: 12px;
			line-height: 24px;
			text-align: center;
			font-size: 14px;
			color: rgb(147,153,159);
		}

		.cart-add {
			display: inline-block;
			padding: 0 6px;
			font-size: 24px;
			line-height: 24px;
			color: rgb(0,160,220);
		}
	}
</style>