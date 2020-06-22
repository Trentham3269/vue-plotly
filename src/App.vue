<template>
	<v-app id="inspire">
		<v-navigation-drawer
			v-model="drawer"
			app
			clipped
		>
			<v-list dense>
				<v-list-item link>
					<v-list-item-action>
						<v-icon>mdi-view-dashboard</v-icon>
					</v-list-item-action>
					<v-list-item-content>
						<v-list-item-title>Dashboard</v-list-item-title>
					</v-list-item-content>
				</v-list-item>
				<v-list-item link>
					<v-list-item-action>
						<v-icon>mdi-cog</v-icon>
					</v-list-item-action>
					<v-list-item-content>
						<v-list-item-title>Settings</v-list-item-title>
					</v-list-item-content>
				</v-list-item>
			</v-list>
		</v-navigation-drawer>

		<v-app-bar
			app
			clipped-left
		>
			<v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
			<v-toolbar-title>Application</v-toolbar-title>
		</v-app-bar>

		<v-main>
			<v-container
				class="fill-height"
				fluid
			>
			<v-row>
				<v-col
					cols="4"
				>
					<v-select
						:items="options"
						v-model="option"
						label="Select movement type" 
						outlined
					></v-select>
				</v-col>
				<v-col
					cols="4"
				>
					<v-select
						:items="measures"
						v-model="measure"
						label="Select measure"
						outlined
					></v-select>
				</v-col>
				<v-col
					cols="4"
				>
					<v-select
						:items="themes"
						v-model="theme"
						label="Select heatmap theme"
						outlined
					></v-select>
				</v-col>
			</v-row>
			<v-row
				align="center"
				justify="center"
			>
				<v-col>
					<PlotlyChart 
						:chart="heatmap"
					></PlotlyChart>
				</v-col>
			</v-row>
			</v-container>
		</v-main>

		<v-footer app>
			<span>&copy; 2020</span>
		</v-footer>
	</v-app>
</template>

<script>
	import PlotlyChart from './components/PlotlyChart';

	export default {
		components: {
			PlotlyChart
		},

		methods: {
			handleResize () {
				this.console.log('Main windows updated');
			},
			getAxisX () {
				if (this.option === 'Entries') {
					this.heatmap.traces[0].x = x_entries;
				} else if (this.option === 'Exits') {
					this.heatmap.traces[0].x = x_exits;
				}
			},
			getAxisY () {
				this.heatmap.traces[0].y = y_axis;
			},
			getAxisZ () {
				if (this.option === 'Entries') {
					this.heatmap.traces[0].z = z_entries;
				} else if (this.option === 'Exits') {
					this.heatmap.traces[0].z = z_exits;
				}
			},
			getTheme () {
				this.heatmap.traces[0].colorscale = this.theme;
			}
		},

		created () {
			this.console = window.console;
			this.getAxisY();
		},

		mounted () {
			window.addEventListener('resize', this.handleResize);
		},

		beforeUpdate () {
			this.getAxisX();
			this.getAxisZ();
			this.getTheme();
		},

		beforeDestroy () {
			window.removeEventListener('resize', this.handleResize);
		},

		data: () => ({
			drawer: null,
			heatmap: {
				uuid: "111",
				traces: [
					{
						z: [],
						x: [],
						y: [],
						type: 'heatmap',
						hoverongaps: false,
						colorscale: []
					}
				],
				layout: {
					// width: 600,
					// height: 600,
					// xaxis: { ticksuffix: "%"},
					dragmode: false, 
					autosize: true,
					// title: {
					// 	text: 'Chart title',
					// 	font: {
					// 		size: 16
					// 	},
					// 	xanchor: "center",
					// 	x: 0.5,
					// 	y: 0.99,
					// },
					showlegend: false,
					margin: {
						t: 40,
						l: 80,
						r: 40,
						b: 40
					}
				}
			},
			options: ['Entries', 'Exits'],
			option: 'Entries',
			measures: ['Headcount', 'Percentage'],
			measure: 'Headcount',
			themes: ['YlGnBu', 'RdBu', 'Greens', 'Greys', 'Electric', 'Earth', 'Portland', 'Picnic', 'Hot', 'YlOrRd'],
			theme: 'YlGnBu'
		})
	};

	// Define data
	var x_entries = ['7-8am', '8-9am', '9-10am', '10-11am', '11-12am'];
	var x_exits = ['3-4pm', '4-5pm', '5-6pm', '6-7pm', '7-8pm'];
	var y_axis = ['Adelaide', 'Brisbane', 'Canberra', 'Darwin', 'Melbourne', 'Perth', 'Sydney'];
	var z_entries = [[8, 20, 32, 12, 6], [13, 31, 9, 8, 3], [71, 17, 34, 18, 20], [67, 131, 19, 81, 33], [76, 75, 99, 67, 29], [78, 124, 99, 88, 44], [144, 119, 79, 58, 66]];
	var z_exits = [[21, 19, 85, 77, 32], [84, 120, 132, 121, 16], [13, 31, 9, 8, 3], [71, 17, 34, 18, 20], [67, 131, 19, 81, 33], [76, 75, 99, 67, 29], [78, 124, 99, 88, 44]];
</script>