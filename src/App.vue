<template>
	<div id="app">
		<div class="bg-cover"></div>
		<img src="./assets/logo.png" class="logo" />
		<div class="sidebar">
			<div style="height:120px;"/>
			<div v-for="(category, key) in categories" :key="key" class="sidebar-menu-item"
			:class="{'sidebar-menu-item-selected': index == key}" @click="index=key">
				&nbsp;&nbsp;{{category}}
			</div>
		</div>
		<div class ="content">
			<FindRoom v-show="index==0" />
			<Allergens v-show="index==1" />
			<Budget v-show="index==2" />
			<Tshirt v-show="index==3" />
			<participants v-show="index==4" />
		</div>
	</div>
</template>

<script>
import FindRoom from './FindRoom.vue'
import Allergens from './Allergens.vue'
import Budget from './Budget.vue'
import Tshirt from './Tshirt.vue'
import Participants from './Participants.vue'

export default {
	name: 'app',
	components: {
		FindRoom,
		Allergens,
		Budget,
		Tshirt,
		Participants
	},
	data () {
		return {
			categories: ['Find Room', 'Personnel Allergies', 'Budget', 'T-Shirt Size', 'Participants'],
			index : 0
		}
	},
	methods: {
	},
	created() {
	}
}
</script>

<style>
.logo {
	width:300px;
	position: relative;
	top: 20px;
	left: 20px;
}

.sidebar-menu-item {
	padding-top:10px;
	padding-bottom: 10px;
	padding-left: 20px;
	color:#aaaaaa;
	cursor: pointer;
}

.sidebar-menu-item:hover {
	color:#ffffff;
	cursor: hand;
}

.sidebar-menu-item-selected {
	background-color: rgba(0,0,0,0.5);
	color: white;
}

.sidebar {
	position: fixed;
	width: 250px;
	height: 100vh;
	/* background-color: rgba(2, 3, 10, 0.05);  */
}

.content-table {
	position:relative;
	background-color:rgba(0,0,0,0.5);
}

.content {
	position: relative;
	left: 280px;
	top:60px;
	width: calc(100vw - 380px);
}

.bg-cover {
	/* background-color: rgba(2, 3, 10, 0.7); */
	position: fixed;
	width: 100vw;
	height: 100vh;
	z-index: -1;
	background: linear-gradient(to left, rgba(139,51,91,0.9), rgba(0,21,46,0.9));
}

body {
	background: url(./assets/bg-default1.jpg) no-repeat center center fixed;
	background-repeat: no-repeat;
	background-attachment: fixed;
}
</style>
