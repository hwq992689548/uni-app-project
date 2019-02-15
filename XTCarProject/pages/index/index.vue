<template>
	<view>
		<view class="uni-common-mt">
			<view>
				<!-- <map id="map" show-location="true" :latitude="latitude" :longitude="longitude" :markers="covers"></map>
				<button @click="clickMeAction" type="default" style="margin-top: 300px;">点我导航</button> -->
				<button @click="mapAction" type="default">地图</button>
				<button @click="clickMeAction" type="default">扫一扫</button>
				
				<!-- <view style="background-color: green; width: 100%; height: 50px;">
					<text style="font-size: 14px;">{{barcodeContent}}</text>
				</view> -->
			</view>
		</view>
	</view>
</template>
<script>
	var barcode = null;
	export default {
		data() {
			return {

			}
		},
		
		onLoad() { 
			
		},

		methods: {
			mapAction(){
				uni.navigateTo({
					url:"../map/map"
				});
			},
			
			clickMeAction() {
				uni.navigateTo({
					url:"../scan/scan"
				});
			},
  
		},
	}
</script>
<style>
	map {
		width: 100%;
		height: 600upx;
	}
</style>
