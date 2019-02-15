<template>
	<view>
		<view class="uni-common-mt">
			<view>
				<map id="map" show-location="true" :latitude="latitude" :longitude="longitude" :markers="covers"></map>
				<button @click="clickMeAction" type="default" style="margin-top: 300px; ">点我导航</button>
			</view>
		</view>
	</view>
</template>
<script>
	var timer = null;
	export default {
		data() {
			return {				
				showLocationFlag: false,
				title: 'map',
				latitude: 39.909,
				longitude: 116.39742,
				covers: [{
					latitude: 39.9085,
					longitude: 116.39747,
					iconPath: '../../static/logo.png'
				}, {
					latitude: 39.90,
					longitude: 116.39,
					iconPath: '../../static/logo.png'
				}]
			}
		},
		
		
		onReady() { 
			console.log('dsfsdifsdlkfjkljflksdjf');
			//定时去获取定位信息
			if(timer != null) {
				timer = null;
			}
			timer = setInterval(function() {
				uni.getLocation({
					type: 'wgs84',
					success: function(res) {
						console.log('当前位置的经度：' + res.longitude);
						console.log('当前位置的纬度：' + res.latitude);
						var message = '当前位置经度：' + res.longitude + '\n当前位置纬度: ' + res.latitude;
						uni.showToast({
							title:message,
							icon:'none',
						});
						this.latitude = res.latitude;
						this.longitude = res.longitude;
						if (res.longitude > 1) { //获取成功
							if (!this.showLocationFlag) {
								this.showLocationFlag = true;
								uni.createMapContext("map", this).moveToLocation();
							}
						}
					}
				});
			}, 5000);			
		},

		methods: {
			mapAction(){
				uni.navigateTo({
					url:"../map/map"
				});
			},
			
			clickMeAction() {
				let url = "iosamap://navi?sourceApplication=XTCarProject&lat=22.53036618462818&lon=113.5063134856093&dev=0&style=0"
				plus.runtime.openURL(url, function(error) {
					console.log(error.code);
					if (error.code == -3) {
						uni.showToast({
							title: "您还没有安装高德地图",
							icon: "none",
						});
					}
				}, "高德地图");
			}, 
		},
		
		onUnload() {
			// timer.clearInterval();
		}
	}
</script>
<style>
	map {
		width: 100%;
		height: 600upx;
	}
</style>
