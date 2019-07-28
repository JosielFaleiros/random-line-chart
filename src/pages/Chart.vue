<template>
  <div>
    <line-chart :chart-data="datacollection"></line-chart>
  </div>
</template>

<script>
    import LineChart from '../components/LineChart.js'
    export default {
        name: 'Chart',
        components: { LineChart },
        mounted() {
            this.infiniteLoop();
        },
        data() {
            return {
                showTime: 1500,
                datacollection: {}
            };
        },
        methods: {
            getRandomInt () {
                return Math.floor(Math.random() * (50 - 5 + 1)) + 5
            },
            infiniteLoop() {
                setTimeout(() => {
                    this.fillData();
                    this.infiniteLoop();
                }, this.showTime);
            },
            fillData () {
                let max = this.getRandomInt(0, this.datacollection.datasets.length)
                let min = max - 10 || 0;
                this.datacollection = {
                    labels: [ ...Array(Number(this.getRandomAmount().toFixed(0))).keys() ].map(x => x + 1),
                    datasets: this.datacollection.datasets.slice(min, max)
                        .concat([ ...Array(Number(this.getRandomAmount().toFixed(0))).keys() ]
                            .map(() => {
                                const color = this.rgbToHex(this.getRandomInt(0, 255), this.getRandomInt(0, 255), this.getRandomInt(0, 255));
                                return {
                                    label: color,
                                    backgroundColor: color,
                                    data: [...Array(Number(this.getRandomAmount().toFixed(0))).keys()].map(x => this.getRandomInt())
                                }
                            }))
                }
            },
            componentToHex(c) {
                var hex = c.toString(16);
                return hex.length == 1 ? "0" + hex : hex;
            },
            rgbToHex(r, g, b) {
                return "#" + this.componentToHex(r) + this.componentToHex(g) + this.componentToHex(b);
            },
            getRandomInt (min = 1, max = 50) {
                return Math.floor(Math.random() * max) + min
            },
            getRandomAmount () {
                return this.getRandomInt(1, 25);
            }
        }
    }
</script>

<style scoped>

</style>
