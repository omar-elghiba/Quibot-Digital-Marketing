<template>
  <div>
    <div class="map" ref="map">

    </div>
    <div class="stats">
      <h6 class="mt-1">Total Conversion</h6>
      <p class="h3 m-0">
      <span class="mr-xs fw-normal"><AnimatedNumber :value="total_puchase"
                                                    v-bind="animateNumberOptions"></AnimatedNumber></span>
        <i class="fa fa-map-marker"/>
      </p>
    </div>
  </div>

</template>

<script>
import axios from 'axios'
import AnimatedNumber from "animated-number-vue";
import * as am4core from "@amcharts/amcharts4/core";
import * as am4maps from "@amcharts/amcharts4/maps";
import am4geodata_germanyHigh from "@amcharts/amcharts4-geodata/germanyHigh";

// import cities from './mock';

export default {
  name: 'Map',
  components: { AnimatedNumber },
  data() {
    return {
      total_puchase : [],
      cities :   [{
                  latitude: 51.250351,
                  longitude: 6.690717,
                  tooltip: 'Meerbusch, North Rhine-Westphalia, Germany'
                },
                {
                  latitude: 47.595661,
                  longitude: 11.072352,
                  size : 4,
                  tooltip: 'Oberammergau, Bavaria, Germany',
                },
                {
                  latitude: 50.810001,
                  longitude: 8.770833,
                  size : 4,
                  tooltip: 'Marburg, Hesse State, Germany',
                },
                {
                  latitude: 47.966923,
                  longitude: 11.184414,
                  size : 4,
                  tooltip: 'Andechs, Bavaria, Germany',
                },
                {
                  latitude: 48.653320,
                  longitude: 13.624967,
                  size : 4,
                  tooltip: 'Hauzenberg, Bavaria, Germany',
                },
                {
                  latitude: 48.798618,
                  longitude: 8.440026,
                  size : 4,
                  tooltip: 'Bad Herrenalb, Germany',
                },
                // {
                //   latitude: 27.1500,
                //   longitude: -13.2000,
                //   size : 4,
                //   tooltip: 'Laayoune',
                // },
                // {
                //   latitude: 30.4167,
                //   longitude: -9.5833,
                //   size : 5,
                //   tooltip: 'Souss - Massa',
                // }],
                ],

      animateNumberOptions: {
        duration: 1000,
        easing: 'easeInQuad',
        formatValue(value) {
          let number = value.toFixed(0);
          let numberAsArrayWithSapces = [];
          while (number >= 1) {
            numberAsArrayWithSapces.unshift(number % 1000);
            number = (number/1000).toFixed();
          }
          return numberAsArrayWithSapces.join(' ');
        }
      }
    }
  },

  // async created(){
  //    await axios
  //               .get('http://127.0.0.1:8000/Countries_Purchase')
  //               .then(response => {
  //                   this.cities[0]['size'] = response.data[1]}
                    
  //               );
                

  // },


// ss

  mounted() {
    //  await axios
    //             .get('http://127.0.0.1:3000/Countries_Purchase')
    //             .then(response => {
    this.cities[0]['size'] = 1630/50;
    this.cities[1]['size'] = 565/50;
    this.cities[2]['size'] = 985/50;
    this.cities[3]['size'] = 439/50;
    this.cities[4]['size'] = 697/50;
    this.cities[5]['size'] = 760/50;
    this.total_puchase = 13500 - 1000
                    
                // );
    let map = am4core.create(this.$refs.map, am4maps.MapChart);
    map.geodata = am4geodata_germanyHigh;
    map.projection = new am4maps.projections.NaturalEarth1();
    map.chartContainer.wheelable = false;
    map.seriesContainer.draggable = false;
    map.seriesContainer.resizable = false;
    let polygonSeries = map.series.push(new am4maps.MapPolygonSeries());
    polygonSeries.useGeodata = true;
    map.homeZoomLevel = 1;

    map.zoomControl = new am4maps.ZoomControl();
    map.zoomControl.align = 'left';
    map.zoomControl.valign = 'bottom';
    map.zoomControl.dy = -20;

    map.zoomControl.minusButton.background.fill = am4core.color("#C7D0FF");
    map.zoomControl.minusButton.background.fillOpacity = 0.24;
    map.zoomControl.minusButton.background.stroke = null;
    map.zoomControl.plusButton.background.fill = am4core.color("#C7D0FF");
    map.zoomControl.plusButton.background.fillOpacity = 0.24;
    map.zoomControl.plusButton.background.stroke = null;
    map.zoomControl.plusButton.label.fill = am4core.color("#fff");
    map.zoomControl.plusButton.label.fontWeight = 600;
    map.zoomControl.plusButton.label.fontSize = 16;
    map.zoomControl.minusButton.label.fill = am4core.color("#fff");
    map.zoomControl.minusButton.label.fontWeight = 600;
    map.zoomControl.minusButton.label.fontSize = 16;
    let plusButtonHoverState = map.zoomControl.plusButton.background.states.create("hover");
    plusButtonHoverState.properties.fillOpacity = 0.24;
    let minusButtonHoverState = map.zoomControl.minusButton.background.states.create("hover");
    minusButtonHoverState.properties.fillOpacity = 0.24;

    let polygonTemplate = polygonSeries.mapPolygons.template;
    polygonTemplate.tooltipText = "{name}";
    polygonTemplate.fill = am4core.color("#474D84");
    polygonTemplate.fillOpacity = 1;
    let hs = polygonTemplate.states.create("hover");
    hs.properties.fillOpacity = 0.5;

    polygonTemplate.stroke = am4core.color("#6979C9");
    polygonTemplate.strokeOpacity = 1;

    let citySeries = map.series.push(new am4maps.MapImageSeries());
    citySeries.data = this.cities;
    citySeries.dataFields.value = "size";

    let city = citySeries.mapImages.template;
    city.nonScaling = true;
    city.propertyFields.latitude = "latitude";
    city.propertyFields.longitude = "longitude";
    let circle = city.createChild(am4core.Circle);
    circle.fill = am4core.color("#0965dd");
    circle.stroke = am4core.color("#ffffff");
    circle.strokeWidth = 0;
    let circleHoverState = circle.states.create("hover");
    circleHoverState.properties.strokeWidth = 1;
    circle.tooltipText = '{tooltip}';
    circle.propertyFields.radius = 'size';

    this.map = map;
  },
};
</script>

<style src="./Map.scss" lang="scss" />
