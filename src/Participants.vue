<template>
	<div id="app">
		<b-table :items="items" :fields="fields" hover striped dark class="content-table">
			<template slot="Committee" slot-scope="row">
				<b-form-select v-model="row.item.CommitteeDescription" :options="options" size="small" class="mb-3" />
			</template>
		</b-table>
		<br />
		<b-button>Save</b-button>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			fields: [ 'Name', 'Committee' ],
			items: null,
			options: [],
			selected: 'M'
		}
	},
	methods: {
		findParticipants() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/Participants").then(res => {
				this.items = res.body;
			})
		},
		findCommittee() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/Committee").then(res => {
				this.options = res.body;
			})
		}
	},
	created() {
		this.findParticipants();
		this.findCommittee();
	}
}
</script>

<style>

.content-table {
	position:relative;
	background-color:rgba(0,0,0,0.5);
}
</style>