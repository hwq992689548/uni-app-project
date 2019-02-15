<template>
	<view>
		<view class="uni-common-mt">
			<view>
				<!-- <map id="map" show-location="true" :latitude="latitude" :longitude="longitude" :markers="covers"></map>
				<button @click="clickMeAction" type="default" style="margin-top: 300px;">点我导航</button> -->
				 
				<!-- <button @click="scanBarCodeAction" type="default">扫一扫</button> -->
				
				<view style="position: absolute; left: 49%; margin-top: 200px; width: 20px; height: 20px; background-color: green; z-index: 9999999;" @click="openFlashAction"></view>
				
			</view>
		</view>
	</view>
</template>
<script>
	var barcode = null;
 
	export default {
		data() {
			return {
				openFlash: false,  //闪光灯
				barcodeContent: "123456789",
				 
			}
		},
		 

		methods: {
 
			clickMeAction() {
				uni.navigateTo({
					url:"../scan/scan"
				});
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
			
			scanBarCodeAction() {
				 
				if(barcode != null) {
					barcode.cancel();
					barcode.close();
					barcode = null;
				}
				
				console.log("ssss");
				var pages = getCurrentPages();
				var page = pages[pages.length - 1];
				var currentWebview = page.$getAppWebview();
				console.log(pages.length); 
		 
				barcode = plus.barcode.create('barcode', [plus.barcode.QR, plus.barcode.EAN13, plus.barcode.CODE128, plus.barcode.EAN8], {
					top: '0px',
					left: '0px',
					width: '100%',
					height: '300px',
					position: 'absolute'
				});
			 
				//此处未演示扫码成功回调的地址设置，实际请参考HTML5Plus API自行处理  
				//注意扫码区域需为正方形，否则影响扫码识别率  
				barcode.onmarked = function(type, result) {
					var text = '未知: ';
					switch (type) {
						case plus.barcode.QR:
							text = 'QR: ';
							break;
						case plus.barcode.EAN13:
							text = 'EAN13: ';
						case plus.barcode.CODE128:
							text = 'CODE128';
							break;
					}
				
					this.barcodeContent = result;
					console.log(text + type + result);
					uni.showToast({
						title:result,
						icon:'none',
						duration:2000,
					});
					
					
					setTimeout(function(){
						barcode.start(); //再次启动扫码
					}, 2000);
				}
			 
				currentWebview.append(barcode);
				barcode.start() //开始扫码
				
				
			},

			openFlashAction(){
				console.log(this.openFlash);
				this.openFlash = !this.openFlash;
				barcode.setFlash(this.openFlash) //是否开启灯光灯
			}
			 
		},
		
		onReady() {
			this.scanBarCodeAction();
			
		},
		 
		
		onUnload() {
			barcode.cancel();
			barcode.close();
		}
	}
</script>
<style>
	map {
		width: 100%;
		height: 600upx;
	}
</style>
