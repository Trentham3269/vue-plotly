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
				<v-row
					align="center"
					justify="center"
				>
					<v-col class="shrink">
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
				this.console.log('Main windows updated')
			}
		},

		created () {
			// this.$vuetify.theme.dark = true;
			this.console = window.console;
		},

		mounted () {
			window.addEventListener('resize', this.handleResize)
		},

		beforeDestroy () {
			window.removeEventListener('resize', this.handleResize)
		},

		data: () => ({
			drawer: null,
			heatmap: {
				uuid: "111",
				traces: [
					{
						z: [[8, 20, 32, 12, 6], [13, 31, 9, 8, 3], [71, 17, 34, 18, 20], [67, 131, 19, 81, 33], [76, 75, 99, 67, 29], [78, 124, 99, 88, 44], [210, 189, 79, 58, 66]],
						x: ['7-8am', '8-9am', '9-10am', '10-11am', '11-12am'],
						y: ['Adelaide', 'Brisbane', 'Canberra', 'Darwin', 'Melbourne', 'Perth', 'Sydney'],
						type: 'heatmap',
						hoverongaps: false
					}
				],
				layout: {
					// width: 600,
					// height: 600,
					// xaxis: { ticksuffix: "%"},
					dragmode: false, 
					autosize: true,
					title: {
						text: 'Chart title',
						font: {
							size: 16
						},
						xanchor: "center",
						x: 0.5,
						y: 0.99,
					},
					showlegend: false,
					margin: {
						t: 40,
						l: 80,
						r: 40,
						b: 40
					}
				}
			}
		})
	};
</script>