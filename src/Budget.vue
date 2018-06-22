<template>
	<div id="app">
		<b-table :items="items" :fields="fields" hover striped dark class="content-table">
			<template slot="TotalCost" slot-scope="row">
				{{row.item.EventRoomCost + row.item.VenueCateringCost + row.item.EquipmentCost}}
			</template>
		</b-table>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			fields: [ 'EventRoomCost', 'VenueCateringCost', 'EquipmentCost', 'TotalCost', 'TotalBudget' ],
			items: null
		}
	},
	methods: {
		findBudget() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/EventBudget").then(res => {
				this.items = res.body;
			})
		},
	},
	created() {
		this.findBudget();
	}
}
</script>

<style>

.content-table {
	position:relative;
	background-color:rgba(0,0,0,0.5);
}
</style>
