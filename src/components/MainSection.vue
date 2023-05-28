<script>
import { ref, onMounted, watchEffect } from 'vue'
export default {
  data() {
    return {
      selectedFile: null,
      imageUrl: '',
	  isShow:true,
	  threshold:128,
	  metalValue:[
		  // 纯铝，金，银
		  {name:'metalValue1',metalR:180,metalG:180,metalB:180},
		  {name:'metalValue2',metalR:242,metalG:192,metalB:86},
		  {name:'metalValue3',metalR:233,metalG:233,metalB:215},
		  // 铜，纯铬，不锈钢
		  {name:'metalValue4',metalR:186,metalG:110,metalB:64},
		  {name:'metalValue5',metalR:220,metalG:230,metalB:240},
		  {name:'metalValue6',metalR:128,metalG:128,metalB:126},
	  ],
	  originalImgData:'',
	  binarizationImgData:'',
	  grayScaleImgData:'',
	  a:0,
	  b:0,
	  valueR:'',
	  valueG:'',
	  valueB:'',
	  contrast:0,
	  brightness:'',
	  contrastValue:100,
	  brightnessValue:100
    }
  },
  methods: {
    onloadPic() {
      this.selectedFile = this.$refs.fileInput.files[0];
      this.previewImage();
	  this.isShow = false;
    },
    previewImage() {
      const reader = new FileReader();
      reader.onload = event => {
        const image = new Image();
        image.src = event.target.result;
        image.onload = () => {
          const canvas = this.$refs.canvas;
          const ctx = canvas.getContext('2d');
          ctx.clearRect(0, 0, canvas.width, canvas.height);
          canvas.width = image.width;
          canvas.height = image.height;
          ctx.drawImage(image, 0, 0);
		  this.originalImgData = ctx.getImageData(0, 0, image.width, image.height);
		}
      }
      reader.readAsDataURL(this.selectedFile);
    },
	// 恢复原图
	originImg(){
		const canvas = this.$refs.canvas;
		const ctx = canvas.getContext('2d');
		this.brightnessValue = 100;
		this.contrastValue = 100;
		canvas.style.backgroundColor = 'rgb(255,255,255)';
		ctx.putImageData(this.originalImgData, 0, 0);
	},
	getImageData() {
	        const canvas = this.$refs.canvas;
	        const ctx = canvas.getContext('2d');
	        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
	        const data = imageData.data;
	        return {canvas, ctx, imageData, data};
	    },
	// 灰度化
	grayScale(){
		const {canvas, ctx, imageData, data} = this.getImageData();
		if(this.b===1){
			canvas.style.backgroundColor = 'rgb(255,255,255)';
			ctx.putImageData(this.grayScaleImgData, 0, 0);
			console.log(this.b)
			// this.b--
		}else{
			this.b++
			for(let i = 0; i <data.length; i += 4){
		            const r = data[i];
		            const g = data[i + 1];
		            const b = data[i + 2];
		            const gray = 0.299 * r + 0.587 * g + 0.114 * b;
		            data[i] = gray;
		            data[i+1] = gray;
		            data[i+2] = gray;
		        }
		canvas.style.backgroundColor = 'rgb(255,255,255)';
		ctx.putImageData(imageData, 0, 0);
		this.grayScaleImgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
		return this.grayScaleImgData
		}
		        
	},
	// 二值化
	binarization(){
		const {canvas, ctx, imageData, data} = this.getImageData();
			// 设置阈值,一般为128，值越高产生的黑点越多
		        for (let i = 0; i < data.length; i += 4) {
		            const r = data[i];
		            const g = data[i + 1];
		            const b = data[i + 2];
		            const gray = 0.299 * r + 0.587 * g + 0.114 * b;
					const value = gray<this.threshold?0:255;
					data[i] = value;
					data[i + 1] = value;
					data[i + 2] = value;
		        }
		        ctx.putImageData(imageData, 0, 0);
				this.binarizationImgData = ctx.getImageData(0, 0, canvas.width, canvas.height); // 保存二值化图像数据的副本
		        return this.binarizationImgData
		    // }
	},
	// 设置金属背景
	metalBack(name){
		const metalBackGroup = this.metalValue.filter(c => c.name === name);
		const valueR = metalBackGroup[0].metalR;
		const valueG = metalBackGroup[0].metalG;
		const valueB = metalBackGroup[0].metalB;
		const getmetalValue = `rgb(${valueR},${valueG},${valueB})`;
		const canvas = this.$refs.canvas;
		const ctx = canvas.getContext('2d');
		if(this.a===1){
			ctx.putImageData(this.binarizationImgData,0,0)
		}else{
			this.a++
		}
		const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
		        const data = imageData.data;
				for(let i = 0;i <data.length ;i += 4){
					const r = data[i];
					const g = data[i + 1];
					const b = data[i + 2];
					if(r===255&&g===255&&b===255){
						data[i] = valueR;
						data[i+1] = valueG;
						data[i+2] = valueB;
					}
				}
		canvas.style.backgroundColor = `rgb(${valueR},${valueG},${valueB})`;
		ctx.putImageData(imageData, 0, 0);
	},
	setMatelVlue(){
		const canvas = this.$refs.canvas;
		const ctx = canvas.getContext('2d');
		if(this.a===1){
			ctx.putImageData(this.binarizationImgData,0,0)
		}else{
			this.a++
		}
		const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
		        const data = imageData.data;
				for(let i = 0;i <data.length ;i += 4){
					const r = data[i];
					const g = data[i + 1];
					const b = data[i + 2];
					if(r===255&&g===255&&b===255){
						data[i] = this.valueR;
						data[i+1] = this.valueG;
						data[i+2] = this.valueB;
					}
				}
		ctx.putImageData(imageData, 0, 0);
	}
  }
}
</script>
	
<template>
	<!-- 整体 -->
	<div id="MainSec">
		<!-- 软件功能部分 -->
		<div id="core">
			<!-- 菜单栏 -->
			<div id="menu">
				<ul id="ulMenu">
					<li>
						<input type="button" value="恢复上传功能" @click="isShow = true">
					</li>
					<li>
						<input type="button" value="恢复原图" @click="originImg">
					</li>
					<li>
						<input type="button" value="生成灰度图像" @click="grayScale">
					</li>
					<li>
						<input type="button" value="生成二值图像" @click="binarization">
					</li>
					<li>
						<input type="text" value="选择金属背景" id="">
						<ul>
							<li>
								<input type="button" value="纯铝" @click="metalBack('metalValue1')">
							</li>
							<li>
								<input type="button" value="黄金" @click="metalBack('metalValue2')">
								
							</li>
							<li>
								<input type="button" value="银" @click="metalBack('metalValue3')">
								
							</li>
							<li>
								<input type="button" value="铜" @click="metalBack('metalValue4')">
								
							</li>
							<li>
								<input type="button" value="纯铬" @click="metalBack('metalValue5')">
								
							</li>
							<li>
								<input type="button" value="不锈钢" @click="metalBack('metalValue6')">
							</li>
						</ul>
					</li>
					<li>
						<!-- <input type="range" v-model="brightness" min="-50" max="50"> -->
						<input type="range" min="0" max="200" v-model="brightnessValue">
						<p id="brightnessRange">亮度：{{brightnessValue}}</p>
					</li>
					<li>
					    <!-- <input type="range" max="50" min="-50" v-model="contrast" > -->
						<input type="range" min="0" max="200" v-model="contrastValue">
					    <p id="contrastRange" >对比度：{{contrastValue}}</p>
					</li>
					<li>
						自定义金属背景
						<ul>
							<li>
								<input type="text" v-model='valueR' min="0" max="255"><br>
								<input type="text" v-model='valueG' min="0" max="255"><br>
								<input type="text" v-model='valueB' min="0" max="255"><br>
								<input type="button" value="设置RGB值" @click="setMatelVlue">
							</li>
						</ul>
					</li>
				</ul>
			</div>
			<!-- 展示部分 -->
			<div id="showImg">
				<!-- 画板部分 -->
				<div id="motherboard">
					<canvas id="myCanvas" ref="canvas" :style="{ filter: `contrast(${contrastValue}%) brightness(${brightnessValue}%)` }"></canvas>				</div>
				<!-- 添加图片 -->
				<div id="uploadboard" :style="{display:isShow?'block':'none'}">
					<div id="uploadAdd">+</div>
					<div id="uploadText">点击添加图片</div>
					<input type="file" ref="fileInput" id="uploadBtn" @change="onloadPic">
				</div>
			</div>
		</div>
	</div>
</template>

<style scoped>
/* 整体 */
	#MainSec{
		height: 750px;
		padding-top: 55px;
	}
/* 功能区 */
	#core{
		display: flex;
		justify-content: center;
	}
	/* 菜单栏 */
	ul,li{
		/* display: inline; */
		list-style: none;
		margin: 0;
		padding: 0;
	}
	li{
		display: inline;
	}
	input{
		cursor: pointer;
		font-size: 25px;
		border: white;
		/* align-content: center; */
		width: 100%;
		background: rgba(255, 255, 255, 0);
	}
	ul li:hover ,ul li:hover {
		background-color: skyblue;
	} 
	ul li ul:hover{
		border: black 1px solid;
	}
	#menu{
		width: 300px;
		/* min-width: 300px; */
		height: 600px;
		font-size: 25px;
		margin-right: 40px;
		border: skyblue 1px solid;
		border-radius: 30px;
		cursor: pointer;
		background: rgba(255, 255, 255, 0.6);
		backdrop-filter: blur(2px);
	}
	#ulMenu{
		margin-top: 25px;
	}
	#menu ul li ul{
		display: none;
	}
	#menu ul li:hover>ul{
		display: block;
	}
	li p{
		font-size: 10px;
	}
	/* 底板 */
	#showImg{
		color: blue;
		min-width: 800px;
		border: skyblue 1px solid;
		border-radius: 30px;
		background: rgba(255, 255, 255, 0.6);
		backdrop-filter: blur(2px);
		display: flex;
		align-items: center;
		justify-content: center;
	}
	/* 画板部分 */
	#motherboard{
		width: 500px;
		height: 500px;
		background-color: white;
	}
	/* 上传部分 */
	#uploadboard{
		border: black 1px solid;
		width: 500px;
		height: 500px;
		font-size: 25px;
		position: absolute;
	}
	#uploadAdd{
		margin-top: 40%;
		display: flex;
		justify-content: center;
	}
	#uploadText{
		display: flex;
		justify-content: center;
	}
	#uploadBtn{
		cursor: pointer;
		height: 100%;
		width: 100%;
		position: absolute;
		top: 0;
		left: 0;
		opacity: 0;
	}
	#myCanvas{
		/* 如果高度大于500px，就设置高度100% */
		height: 100%;
/* 		height: 500px;
		width: 500px; */
	}
</style>
