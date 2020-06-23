<template>
  <v-card>
    <div
      :ref="chart.uuid"
    >
    </div>
  </v-card>
</template>

<script>
  import Plotly from 'plotly.js-dist';

  export default {
    props: ["chart"],

    methods: {
      handleResize () {
        Plotly.relayout(this.$refs[this.chart.uuid], this.chart.layout);
      }
    },

    created () {
      this.console = window.console;
    },

    mounted () {
      Plotly.plot(this.$refs[this.chart.uuid], this.chart.traces, this.chart.layout, {displayModeBar: false});
      window.addEventListener('resize', this.handleResize)
    },

    beforeDestroy () {
      window.removeEventListener('resize', this.handleResize)
    },

    watch: {
      chart: {
        handler () {
          Plotly.react(
            this.$refs[this.chart.uuid], 
            this.chart.traces,
            this.chart.layout,
          );
        },
        deep: true
      },
    },

    data: () => ({
      title: "Hello Plotly",
    }),
  };
</script>