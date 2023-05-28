<template>
	<div class="CardTab">
		<div class="CardBox">
			<ul>
				<li v-for="list in lists">
					<div class="card">
						<div class="CardContent" >
							<h3>{{list.name}}</h3>
							<p>{{list.context}}</p>
							<a href="">获取更多</a>
						</div>
					</div>
				</li>
			</ul>							
		</div>
	</div>
</template>

<script>
	export default{
		data(){
			return{
				lists:[
					{
						name:'灰度化',
						context:'灰度图像是一种黑白图像，其中每个像素的颜色值也称为灰度值，通常范围从0到255，表示颜色的深浅程度，其中白色对应的灰度值为255，黑色对应的灰度值0，中间的灰度值的不同则表示不同深浅程度的灰色，在RGB色彩模式中，三个值相同，若值越大，则颜色越浅。',
						target:'',
						link:''
					},
					{
						name:'二值化',
						context:'二值图像是一种由黑色和白色像素组成的图像，其中每个像素只能是黑或白。在计算机视觉和图像处理中，二值图像是一种常见的图像类型，因为它们只包含两种颜色，使得它们比多彩图像更易于处理和分析。二值图像通常用于图像分割、形态学分析、字符识别等任务中。一些常见的二值图像格式包括黑白图像、位图和二进制图像等。',
						target:'',
						link:''
					},
					{
						name:'金属背景',
						context:'金属背景是指具有金属外观、纹理或光泽的背景材质或图案，可以用于各种设计和应用中，如网页设计、海报、名片、产品包装等。金属背景可以通过多种方式制作，比如使用照片或图像编辑软件进行材质渲染、纹理贴图或使用图案制作工具进行图案设计和排列组合。金属背景的类型有很多，如铜、锡、银、黄铜、钢铁或其他类型的合金金属等，每种类型的金属背景都有其特定的纹理和颜色，可以根据需要进行选择和调整。',
						target:'',
						link:''
					},
					{
						name:'亮度、对比度',
						context:'亮度和对比度是调整图像显示效果的两个参数。亮度是指整个图像的明暗程度，可以通过增加或减少图像的亮度值来调整图像的明暗程度。一般来说，增加亮度可以使整个图像变亮，减少亮度则可以使整个图像变暗。对比度则是指图像中最暗区域和最亮区域之间的差异度，就像黑白照片中的黑和白之间的差异一样。调整对比度的作用是增强图像的细节和清晰度。一般来说，增加对比度可以使图像更加清晰，减少对比度则可以使图像更加柔和。',
						target:'',
						link:''
					}
				],
			}		
		}
	}
</script>

<style>
	*{
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	.CardTab{
		width: 100%;
		height: 730px;
		display: flex;
		align-items: center;
	}
	.CardBox{
		width: 100%;
		display: flex;
		justify-content: center;
	}
	.CardBox ul{
		display: flex;
	}
	.card{
		background:rgba(255,255,255,0.3);
		backdrop-filter: blur(5px);
		border: skyblue 1px solid;
		border-radius:18px;
		width: 300px;
		height: 400px;
		margin: 30px;
	}
	.card:hover .CardContent{
		transform: translateY(0);	
		opacity: 1;
	}
	.CardContent{
		transform: translateY(100px);
		transition: 0.5s;
		opacity: 0;
		padding: 20px;
		text-align: center;
	}
	.CardContent h3{
		font-size: 30px;
		font-weight: 600;
	}
	.CardContent p{
		font-size: 15px;
	}
	.CardContent a{
		display: inline-block;
		margin-top: 30px;
		padding: 8px 20px;
		background-color: papayawhip;
		border-radius: 20px;
		text-decoration: none;
		box-shadow: 0 5px 15px rgba(0,0,0,0.3);
	}
</style>