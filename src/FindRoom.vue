<template>
	<div id="app">
		<b-form-select :options="meetings" size="small" class="mb-3" />
		<b-form inline>
			<b-input class="mb-2 mr-sm-2 mb-sm-0" placeholder="Event Room Size" v-model="roomSizeFilter" />
			<b-input class="mb-2 mr-sm-2 mb-sm-0" placeholder="Event Room Cost" v-model="costFilter" />
			<b-input class="mb-2 mr-sm-2 mb-sm-0" placeholder="Venue City" v-model="venueFilter" />
			<b-button variant="secondary" @click="applyFilter">Apply</b-button>
		</b-form>
		<br />
		<b-table :items="filteredItems" :fields="fields" hover striped dark class="content-table">
			<template slot="show_details" slot-scope="row">
				<!-- we use @click.stop here to prevent emitting of a 'row-clicked' event  -->
				<b-button size="sm" @click.stop="row.toggleDetails" class="mr-2">
				{{ row.detailsShowing ? 'Hide' : 'Show'}} Details
				</b-button>
				</template>
				<template slot="row-details" slot-scope="row">
				<b-card bg-variant="light" text-variant="dark">
					<b-row class="mb-2">
					<b-col sm="3" class="text-sm-right"><b>Event Room Function:</b></b-col>
					<b-col>{{ row.item.EventRoomFunction }}</b-col>
					</b-row>
					<b-row class="mb-2">
					<b-col sm="3" class="text-sm-right"><b>Event Room Layout:</b></b-col>
					<b-col>{{ row.item.EventRoomLayout }}</b-col>
					</b-row>
					<b-row class="mb-2">
					<b-col sm="3" class="text-sm-right"><b>Venue:</b></b-col>
					<b-col>{{ row.item.VenueName }}<br />{{row.item.VenueAddress}}</b-col>
					</b-row>
					<b-row class="mb-2">
					<b-col sm="3" class="text-sm-right"><b>AV Equipments:</b></b-col>
					<b-col>{{ row.item.AVEquipments }}</b-col>
					</b-row>
					<b-button size="sm" @click="row.toggleDetails">Hide Details</b-button>
				</b-card>
			</template>
			<template slot="select" slot-scope="row">
				<b-button size="sm">Select</b-button>
			</template>
		</b-table>
	</div>
</template>

<script>
export default {
	name: 'app',
	data () {
		return {
			fields: [ 'EventRoomName', 'EventRoomSize', 'EventRoomCost', 'VenueCity', 'show_details', 'select' ],
			items: null,
			venueFilter: "",
			costFilter: "",
			roomSizeFilter: "",
			filteredItems: [],
			meetings: []
		}
	},
	methods: {
		findRooms() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/FindRoom").then(res => {
				this.items = res.body;
				this.filteredItems = this.items;
			})
		},
		findMeeting() {
			this.$http.get("https://flowbitserver.cf.isus.emc.com/api/Meeting").then(res => {
				this.meetings = res.body;
			})
		},
		filteredList() {
			var city, cost, roomSize;
			if (this.venueFilter == "")
				city = "";
			else
				city = this.venueFilter;
			if (this.costFilter == "")
				cost = 99999;
			else
				cost = this.costFilter;
			if (this.roomSizeFilter == "")
				roomSize = 0;
			else
				roomSize = this.roomSizeFilter;
			
			var filteredList = [];
			for (var key in this.items)
				if (this.items[key].EventRoomSize >= roomSize
				&& this.items[key].EventRoomCost <= cost
				&& this.items[key].VenueCity.indexOf(city) != -1)
					filteredList.push(this.items[key]);

			return this.filteredItems = filteredList;
		},
		applyFilter() {
			this.filteredList();
		}
	},
	created() {
		this.findRooms();
		this.findMeeting();
	}
}
</script>

<style>

.content-table {
	position:relative;
	background-color:rgba(0,0,0,0.5);
}
</style>
