<template>
	<div id="app">
		<b-table :items="items" hover striped dark class="content-table">
		</b-table>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			items: null
		}
	},
	methods: {
		findAllergens() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/Allergens").then(res => {
				this.items = res.body;
			})
		}
	},
	created() {
		this.findAllergens();
	}
}
</script>

<style>

.content-table {
	position:relative;
	background-color:rgba(0,0,0,0.5);
}
</style>