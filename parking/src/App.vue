<script setup>
import HelloWorld from './components/HelloWorld.vue'
import {ref} from 'vue'

const stopFlag = ref(false)
const stop = () => {
	stopFlag.value = true
}
const start = () => {
	redLight()
}
const redLight = async (time) => {
	if (!stopFlag.value) {
		let t1 = await new Promise((resolve) => {
			setTimeout(() => {
				resolve('red')
			}, time*1000)
		}).then(() => {
			clearTimeout(t1)
			greenLight(1)
		})
	}
}
const greenLight = async (time) => {
	let t1 = await new Promise(() => {
		return setTimeout(() => {
			console.log('red')
		}, time*1000)
	}).then(() => {
		clearTimeout(t1)
		yellowLight(2)
	})
}
const yellowLight = async (time) => {
	let t1 = await new Promise(() => {
		return setTimeout(() => {
			console.log('red')
		}, time*1000)
	}).then(() => {
		clearTimeout(t1)
		redLight(3)
	})
}
// 价格计算
const priceRef = ref(0)
const yearRef = ref(0)
const rateRef = ref(5)
const rentRef = ref(250)
const rentRateRef = ref(10)
const result = ref('--')
const resultDetail = ref('')
const truthCalc = () => {
	console.log(priceRef.value.value)
	console.log(888)
	resultDetail.value = ''
	result.value = priceRef.value.value
	let year = yearRef.value.value
	let rate = rateRef.value.value
	let rent = rentRef.value.value*0.0001
	let rentRate = rentRateRef.value.value
	let yearCalc = (num, year) => {
		if (year === 0) {
			return ((num -rent*12) * (1+rate*0.01)).toFixed(4)
		} else {
			return ((num - rent*12) * (1+rate*0.01)).toFixed(4)
		}
	}
	for (let i = 0; i < year; i++) {
		console.log(`第${i+1}年，租金${rent}`)
		result.value = yearCalc(result.value, i)
		resultDetail.value = resultDetail.value + `第${i+1}年，剩余${result.value}万,租金每月${(rent*10000).toFixed(1)}元<br>`
		rent = rent*(1+rentRate*0.01)
	}
}
console.log(rateRef.value,  333)
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <button @click="start">start</button>
  <button @click="stop">stop</button> -->
  <div style="fontSize:30px">车位计算</div>
  <div style="textAlign:left">
  <!-- <div style="width: 500px;"> -->
		购买价格<input type="text" value="7.3" ref="priceRef">万
		<br>
		使用年数<input type="text" value="10" ref="yearRef">
		<br>
		理财利率<input type="text" value="5" ref="rateRef">%
		<br>
		租金起始值<input type="text" value="250" ref="rentRef">元
		<br>
		租赁涨幅<input type="text" value="6" ref="rentRateRef">%
		<div>
			<button @click="truthCalc">提交</button>
		</div>
		计算结果：
		<br>
		若不购买车位，购车位款项用于理财，理财年化利率按{{rateRef.value}}%计算，车位租赁起始租金{{rentRef.value}}，租金年涨幅{{rentRateRef.value}}%，{{ yearRef.value }}年后，共计余额{{ result }}万。
		<br>
		若购买车位，{{ yearRef.value }}年后售出，相同余额售价需达到{{ result }}万。
		<br>
		计算明细：
		<br>
		<div v-html="resultDetail"></div>
		计算规则：
		<div style="color:red">
			当年结余 = 车位总价-车位租赁价格*（1+涨幅）      <span style="color:black">注：首年涨幅为0</span>
			<br>
			当年价值 = 当年结余 * （1+理财年化利率）
			<br>
			次年结余=当年价值-车位租赁价格*（1+涨幅）
			<br>
			...
		</div>
  </div>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
