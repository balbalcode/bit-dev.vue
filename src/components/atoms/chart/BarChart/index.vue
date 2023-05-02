<script>
export default {
  name: 'BarChart',
  components: {
    VueApexCharts: () => import('vue-apexcharts'),
  },
  props: {
    id: { default: 'bar-chart' },
    x_title: { default: '' },
    y_title: { default: '' },
    x_label_formatter: { default: '' },
    y_label_formatter: { default: '' },
    height: { default: 'auto' },
    type: { default: 'area' },
    chart_name: { default: () => ({}) },
    value: { default: () => [] },
    name: { default: 'Waktu kedatangan' },
    dir: { default: 'ltr' },
    additional_class: { default: '' },
    has_chart_toolbar: { default: false },
    legend_position: { default: 'top' },
    legend_vertical_align: { default: 'right' },
    legend_horizontal_align: { default: 'right' },
    has_data_labels: { default: false },
    is_stacked: { default: false },
  },
  data: () => ({
    series: [],
    options: {
      noData: {
        text: 'Chart tidak tersedia',
      },
      chart: {
        stacked: false,
        toolbar: {
          show: false,
        },
      },
      legend: {
        position: 'top',
        verticalAlign: 'right',
        horizontalAlign: 'right',
      },
      dataLabels: {
        enabled: false,
      },
      stroke: {
        curve: 'smooth',
        width: 3,
      },
      colors: [],
      xaxis: {
        type: 'string',
        categories: [],
        title: {
          text: '',
        },
        labels: {},
      },
      yaxis: {
        title: {
          text: '',
        },
        labels: {},
      },
      grid: {
        borderColor: '#f1f1f1',
      },
    },
  }),
  created() {
    this.options.chart.stacked = this.is_stacked
    this.processThemingChart()
  },
  methods: {
    processSetLabelFormatter() {
      if (this.x_label_formatter)
        this.options.xaxis.labels.formatter = this.x_label_formatter
      if (this.y_label_formatter)
        this.options.yaxis.labels.formatter = this.y_label_formatter
    },
    processSetTitles() {
      if (this.x_title) this.options.xaxis.title.text = this.x_title
      if (this.y_title) this.options.yaxis.title.text = this.y_title
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
      this.options.chart.toolbar.show = false
      this.options.legend.position = this.legend_position
      this.options.legend.verticalAlign = this.legend_vertical_align
      this.options.legend.horizontalAlign = this.legend_horizontal_align
      this.options.dataLabels.show = this.has_data_labels
    },
    processThemingChart() {
      this.processSetTitles()
      this.processFillChartColor()
      this.processFillChartOption()
      this.processSetLabelFormatter()
      this.processLabelChart()
      this.processSeriesChart()
    },
    processLabelChart() {
      this.series = []
      this.options.xaxis.label = []
      this.options.xaxis.categories = []
      this.value[0].forEach((item, index) => {
        this.options.xaxis.categories.push(item.name)
      })
    },
    processSeriesChart() {
      this.series = []
      this.chart_name.forEach((item, index) => {
        this.series.push({
          name: item,
          data: this.processValueSeries(index),
        })
      })
    },
    processValueSeries(index) {
      let series = []
      this.value[index].forEach((item) => {
        series.push(item.values)
      })
      return series
    },
  },
}
</script>
<template>
  <div :id="id" :ref="id">
    <vue-apex-charts
      :class="`apex-charts ${additional_class}`"
      :height="height"
      :type="type"
      :dir="dir"
      :series="series"
      :options="options"
    ></vue-apex-charts>
  </div>
</template>
