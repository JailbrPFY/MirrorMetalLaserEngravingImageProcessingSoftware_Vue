<template>
  <div class="container" >
    <div class="header">
      <div class="menu">
        <ul>
          <li v-for="(menu, index) in menus" :key="index">
            <a :href="menu.link" :target="menu.target">{{ menu.text }}</a>
          </li>
        </ul>
      </div>
    </div>
    <div class="content">
      <div class="title">{{ title }}</div>
      <div class="description">{{ description }}</div>
      <div class="button-container"></div>
    </div>
    <div class="arrow-container" @click="jumpToNext">
		<span class="arrow-icon"></span>
	</div>
  </div>
  <div id="targetSec"></div>
</template>

<script>
export default {
  data() {
    return {
      title: "基于Vue的镜面金属激光雕刻图像处理软件",
      description:
        "你可以在这里对图像进行灰度化、二值化等处理",
      menus: [
		  {
          text: "注册",
          link: "",
		  target:'_blank'
        },
		{
          text: "登陆",
          link: "",
		  target:'_blank'
        },
        {
          text: "首页",
          link: "",
		  target:'_self'
        },
      ],
    };
  },
  methods: {
    jumpToNext() {
      // 实现箭头点击后跳转到下一个组件的功能
	  // alert(123)
		window.location.hash = '#targetSec';
    },
  },
};
</script>

<style>
	#target-section{
		position: fixed;
		margin-top: 500px;
		height: 10px;
		width: 100%;
		background-color: red;
	}
*{
	padding: 0;
	margin: 0;
}
.container {
	width: 100%;
	height: 730px;
	backdrop-filter: blur(2px) brightness(90%);
}

.menu ul {
	padding: 20px 0px;
	list-style: none;
	display: flex;
	flex-direction: row-reverse;
}
.menu ul:hover{
	background-color: white;
	opacity: 0.6;
}

.menu ul li {
  margin-right: 30px;
}
.menu ul li:hover{
	text-decoration: underline rgb(66, 66, 66);
}

.menu ul li a {
  color: black;
  text-decoration: none;
  font-size: 25px;
}

.content {
	max-width: 900px;
	margin: 0 auto;
	text-align: center;
	padding-top: 200px;
}

.title {
	font-size: 60px;
	font-weight: bold;
	color: black;
	margin-bottom: 30px;
}

.description {
  font-size: 20px;
  color: black;
  margin-bottom: 50px;
}

.arrow {
  position: absolute;
  top: 50%;
  right: 20%;
  transform: translateY(-50%);
  height: 0;
  width: 0;
  border-width: 10px;
  border-style: solid;
  border-color: transparent transparent #fff transparent;
}

.arrow:before {
  content: "";
  position: absolute;
  top: -10px;
  left: -10px;
  height: 0;
  width: 0;
  border-width: 10px;
  border-style: solid;
  border-color: transparent transparent #e65100 transparent;
}

.arrow-container {
  position: absolute;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);
  cursor: pointer;
  transition: transform 0.3s;
}

.arrow-container:hover {
  transform: translateX(-50%) translateY(5px);
}

.arrow-icon {
  position: relative;
  display: block;
  width: 40px;
  height: 40px;
  border-style: solid;
  border-color: #fff;
  border-width: 0 0 4px 4px;
  transform: rotate(315deg);
  margin: 0 auto;
}
</style>