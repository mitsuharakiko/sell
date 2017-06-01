<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img :src="seller.avatar" width="64" height="64">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{ seller.name }}</span>
				</div>
				<div class="description">
					{{ seller.description +' / '+ seller.deliveryTime }}
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper">
			
		</div>
	</div>
</template>

<script>
  	export default {
  	props: { seller: { type: Object } },
  	created: function () { alert(this.seller); }
  	};
</script>

<style lang="scss" scoped>
</style>
