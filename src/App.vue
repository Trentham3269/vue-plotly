<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-if="loggedIn === true"
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
  
    <v-main
    >
      <template 
        v-if="loggedIn === false"
      >
        <v-container>
          <LoginCard
            @userLoggedIn="loggedIn = $event"
          ></LoginCard>
        </v-container>
      </template>

      <template
        v-else
      >
        <v-row
          align="center"
          justify="center"
          dense
          class="pa-5"
        >
          <v-col>
            <PlotlyChart 
              :chart="heatmap"
              class="pa-5"
            ></PlotlyChart>
          </v-col>
        </v-row>

        <v-spacer></v-spacer>

        <v-row
          class="px-5"
        >
        <v-toolbar>
          <v-overflow-btn
            v-model="option"
            :items="options"
            label="Direction"
            hide-details
            class="pa-0"
            overflow
          ></v-overflow-btn>

            <template v-if="$vuetify.breakpoint.mdAndUp">
              <v-divider vertical></v-divider>

              <v-overflow-btn
                v-model="measure"
                :items="measures"
                label="Measure"
                hide-details
                class="pa-0"
                overflow
              ></v-overflow-btn>

              <v-divider vertical></v-divider>

              <v-overflow-btn
                v-model="date"
                :items="dates"
                label="Date"
                hide-details
                class="pa-0"
                overflow
              ></v-overflow-btn>

              <v-spacer></v-spacer>

              <v-btn-toggle
                v-model="theme"
                mandatory
                dense
                group
              >
                <v-btn value="YlGnBu">
                  Blues
                </v-btn>
                <v-btn value="Greens">
                  Greens
                </v-btn>
                <v-btn value="Greys">
                  Greys
                </v-btn>
                <v-btn value="Hot">
                  Reds
                </v-btn>
              </v-btn-toggle>
            </template>
          </v-toolbar>  
        </v-row>
      </template>
    </v-main>

    <v-footer app>
      <span>&copy; 2020</span>
    </v-footer>
  </v-app>
</template>

<script>
  import PlotlyChart from './components/PlotlyChart';
  import LoginCard from './components/LoginCard';
  import HeatmapConfig from './components/HeatmapConfig';
  import EntriesCount from './components/EntriesCount';
  import ExitsCount from './components/ExitsCount';

  export default {
    components: {
      PlotlyChart,
      LoginCard
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
          this.heatmap.traces[0].z = this.zEntries.count;
        } else if (this.option === 'Exits') {
          this.heatmap.traces[0].z = this.zExits.count;
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
      drawer: false,
      heatmap: HeatmapConfig,
      options: ['Entries', 'Exits'],
      option: 'Entries',
      measures: ['Headcount', 'Percentage'],
      measure: 'Headcount',
      dates: ['2020-06-25', '2020-06-24', '2020-06-23', '2020-06-22', '2020-06-19', '2020-06-18'],
      date: '2020-06-25',
      theme: undefined,
      zEntries: EntriesCount,
      zExits: ExitsCount,
      loggedIn: false,
    })
  };

  // Define data
  var x_entries = ['7-8am', '8-9am', '9-10am', '10-11am', '11-12am'];
  var x_exits = ['3-4pm', '4-5pm', '5-6pm', '6-7pm', '7-8pm'];
  var y_axis = ['Adelaide', 'Brisbane', 'Canberra', 'Darwin', 'Melbourne', 'Perth', 'Sydney'];
</script>
