<template>
	<div id="app">
		<b-table :items="items" :fields="fields" hover striped dark class="content-table">
			<template slot="Size" slot-scope="row">
				<b-form-select v-model="row.item.TeeShirtSize" :options="options" size="small" class="mb-3" />
			</template>
		</b-table>
		<br />
		<b-button v-b-modal.modal1>Show Summary</b-button>

		<b-modal id="modal1" title="Summary">
			<p>S : {{getSummary()["S"]}}</p>
			<p>M : {{getSummary()["M"]}}</p>
			<p>L : {{getSummary()["L"]}}</p>
			<p>XL : {{getSummary()["XL"]}}</p>
		</b-modal>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			fields: [ 'Name', 'EmailAddress', 'Size' ],
			items: null,
			options: ['S', 'M', 'L', 'XL'],
			selected: 'M'
		}
	},
	methods: {
		findBudget() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/TeeShirtSize").then(res => {
				this.items = res.body;
			})
		},
		getSummary() {
			var retVal = []
			for (var key in this.items)
				if (this.items[key].TeeShirtSize != null)
				{
					if (!retVal[this.items[key].TeeShirtSize])
						retVal[this.items[key].TeeShirtSize] = 0;
					retVal[this.items[key].TeeShirtSize]++;
				}

			console.log(retVal);
			return retVal;
		}
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
