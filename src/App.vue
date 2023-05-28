<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import MainSection from './components/MainSection.vue'
import Hello from './components/Hello.vue'
import test from './components/test.vue'
import CardSection from './components/CardSection.vue'
</script>

<template>
	<Hello/>
	<CardSection/>
	<MainSection/>
	<!-- <test/> -->
  
</template>

<style>
	body{
		background-image:url('./图片/主图.jpg');
		background-repeat: no-repeat;
		background-size: 100%;
		margin-top: 0px;
	}
</style>
