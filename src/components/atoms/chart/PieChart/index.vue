<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'PieChart',
  components: {
    apexchart: VueApexCharts,
  },
  props: {
    id: { default: 'pie-chart' },
    name: { default: 'Durasi Parkir' },
    value: { default: () => [] },
    width: { default: '100%' },
    height: { default: 'auto' },
    dir: { default: 'ltr' },
    is_showed_legend: { default: true },
    legend_format: { default: 'top-bottom' },
    is_floating_legend: { default: true },
    legend_position: { default: 'bottom' },
    legend_vertical_align: { default: 'center' },
    legend_horizontal_align: { default: 'center' },
    legend_font_size: { default: '14px' },
    legend_x_offset: { default: 0 },
    legend_y_offset: { default: 0 },
    responsive_breakpoints: { default: 600 },
    responsive_chart_height: { default: 240 },
    is_responsive_show_legend: { default: false },
    additional_class: { default: '' },
  },
  data: () => ({
    series: [],
    helper: {
      chart_class: '',
      legend_class: '',
    },
    options: {
      labels: [],
      colors: [],
      chart: {
        offsetY: 0,
      },
      legend: {
        show: false,
        position: 'bottom',
        horizontalAlign: 'center',
        verticalAlign: 'middle',
        floating: false,
        fontSize: '14px',
        offsetX: 0,
        offsetY: 10,
      },
      responsive: [
        {
          breakpoint: 0,
          options: {
            chart: {
              width: '100%',
              height: '64px',
            },
            legend: {
              show: false,
            },
          },
        },
      ],
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

    processFillChartColor() {
      this.options.colors = [
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
    },

    processFillChartOption() {
      this.options.chart.height = this.height
      this.options.chart.width = this.width
      this.options.legend.position = this.legend_position
      this.options.legend.fontSize = this.legend_font_size
      this.options.legend.show = false
      this.options.legend.floating = false
      this.options.responsive[0].breakpoint = this.responsive_breakpoints
      this.options.responsive[0].options.chart.height =
        this.responsive_chart_height
      this.options.responsive[0].options.legend.show =
        this.is_responsive_show_legend
      this.options.legend.horizontalAlign = this.legend_horizontal_align
      this.options.legend.verticalAlign = this.legend_vertical_align
      this.options.legend.offsetY = this.legend_y_offset
      this.options.legend.offsetX = this.legend_x_offset
    },

    processThemingChart() {
      this.processFillChartColor()
      this.processFillChartOption()
    },

    processDataChart() {
      this.series = []
      this.options.labels = []
      this.value.forEach((item) => {
        this.series.push(item.values)
        this.options.labels.push(item.name)
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
        <apexchart
          :class="`apex-charts ${additional_class}`"
          :height="height"
          type="donut"
          :dir="dir"
          :series="series"
          :options="options"
        ></apexchart>
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
            :style="`border-left: 4px solid ${options.colors[index]}`"
            :key="index"
          >
            <p
              class="font-size-11 mb-0"
              :id="`${id}__legend-${index}`"
              :ref="`${id}__legend-${index}`"
            >
              <strong>{{ legend.name }}</strong
              ><br />
              {{ legend.values }}
            </p>
          </b-col>
        </b-row>
      </div>
    </div>
  </div>
</template>
