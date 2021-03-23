<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	.uni-tabbar__item:nth-child(4){
		// position: fixed !important;
		// left: 0;
		// right: 0;
		// height: 160rpx !important;
		.uni-tabbar__bd{
			
		}
	}
</style>
