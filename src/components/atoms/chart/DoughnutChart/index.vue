<script>
export default {
  components: {
    DoughnutChart: () => import('@/plugins/Charts/DoughnutChart'),
  },
  props: {
    id: { default: 'doughnut-chart' },
    name: { default: 'Durasi Parkir' },
    value: { default: () => [] },
    width: { default: '80%' },
    height: { default: 256 },
    dir: { default: 'ltr' },
    is_showed_legend: { default: true },
    legend_format: { default: 'top-bottom' },
    additional_class: { default: '' },
  },
  data: () => ({
    series: [],
    helper: {
      chart_class: '',
      legend_class: '',
    },
    charts: {
      labels: [],
      datasets: [
        {
          label: '',
          backgroundColor: [],
          data: [],
        },
      ],
    },
    options: {
      legend: {
        display: false,
      },
    },
  }),
  watch: {
    value: function () {
      this.processDataChart()
    },
  },
  created() {
    this.processThemingChart()
    this.processDataChart()
    this.formatPositionLegend()
  },
  methods: {
    formatPositionLegend() {
      // we have 4 mode for setting up position
      // top-bottom (top chart bottom legend)
      // bottom-top (bottom chart top legend)
      // right-left (left chart right legend)
      // left-right (right chart left legend)
      // and each format has top-bottom format in responsive mode
      // this mode will translated to bootstrap class name
      if (this.legend_format === 'top-bottom') {
        this.helper.chart_class = 'col-12 mx-3'
        this.helper.legend_class = 'col-12 mx-3'
      } else if (this.legend_format === 'bottom-top') {
        this.helper.chart_class = 'col-12 mx-3 order-lg-2'
        this.helper.legend_class = 'col-12 mx-3 order-lg-1'
      } else if (this.legend_format === 'right-left') {
        this.helper.chart_class = 'col-12 col-lg-6'
        this.helper.legend_class = 'col-12 col-lg-6'
      } else if (this.legend_format === 'left-right') {
        this.helper.chart_class = 'col-12 order-lg-2 col-lg-6'
        this.helper.legend_class = 'col-12 order-lg-1 col-lg-6'
      } else {
        this.helper.chart_class = 'col-12 mx-3'
        this.helper.legend_class = 'col-12 mx-3'
      }
    },

    processThemingChart() {
      this.charts.datasets[0].backgroundColor = [
        this.$utility.getPrimaryColor(),
        '#74788d',
        '#5fa2f4',
        '#5abf78',
        '#6983aa',
        '#e97171',
        '#ede682',
        '#ba7967',
        '#f1c5c5',
        '#f69e7b',
      ]
      this.charts.datasets[0].label = this.name
    },

    processDataChart() {
      this.charts.datasets[0].data = []
      this.charts.labels = []
      this.value.forEach((item) => {
        this.charts.datasets[0].data.push(item.values)
        this.charts.labels.push(item.name)
      })
    },

    processFillChart() {
      try {
        this.value !== '' ? this.processDataChart() : ''
      } catch (error) {
        this.$utility.setErrorContextSentry(error)
        this.$sentry.captureMessage(
          `${error.message} at processFillChart in PieChart`
        )
      }
    },
  },
}
</script>
<template>
  <div class="clearfix w-100" :id="id" :ref="id">
    <div class="row">
      <div
        :class="`${helper.chart_class}`"
        :id="`${id}__wrapper`"
        :ref="`${id}__wrapper`"
      >
        <doughnut-chart
          :chart-data="charts"
          :options="options"
          :height="height"
        ></doughnut-chart>
      </div>
      <div
        :class="`${helper.legend_class} my-5`"
        v-if="is_showed_legend"
        :id="`${id}__legend`"
        :ref="`${id}__legend`"
      >
        <b-row>
          <b-col
            v-for="(legend, index) in value"
            cols="6"
            class="mt-2"
            :style="`border-left: 4px solid ${charts.datasets[0].backgroundColor[index]}`"
            :key="index"
          >
            <p
              class="font-size-11 mb-0"
              :id="`${id}__legend-${index}`"
              :ref="`${id}__legend-${index}`"
            >
              <strong>{{ legend.name }}</strong>
              <br />
              {{ legend.values }}
            </p>
          </b-col>
        </b-row>
      </div>
    </div>
  </div>
</template>
