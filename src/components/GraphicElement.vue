<template>
    <div ref="plot"
          class="graphic-style"
    ></div>
    </template>
    
    <script>
    import Plotly from 'plotly.js-dist';
    
    export default {
      name: 'graphic-element',
      data() {
        return {
          slicedItems: [],
          xData: [],
          yData: [],
          zData: [],
        }
      },
      props: {
        items: Array,
        },
      mounted() {
        this.slicedItems = this.slicing(Array((this.items.map(item => item)))[0]);
        this.fillXYZ();
        this.createGraph(); 

      },
      methods: {
        createGraph() {
          const data = [
            {
              x: this.xData[0],
              y: this.yData,
              z: this.zData,
                
    
              type: 'surface',
            
              contours: {
                z: {
                  show: true,
                  usecolormap: true,
                  highlightcolor: '#42f462',
                  project: { z: true },
                },
              },
          
              colorscale: 'Viridis',
            },
          ];
    
          const layout = {
            paper_bgcolor: "rgba(0,0,0,0)",
            autosize: true,
            scene: {
              xaxis: { title: 'X Axis' },
              yaxis: { title: 'Y Axis' },
              zaxis: { title: 'Z Axis' },
            },
            margin: {
              l: 0,
              r: 0,
              b: 0,
              t: 0,
            },
          };
    
          Plotly.newPlot(this.$refs.plot, data, layout);
        },
    
        fillXYZ() {
          let tmp;
          console.log(this.slicedItems[0]);
          this.xData = Array(this.slicedItems[0].map(item => Number(item.x)));
    
          for (let i = 0; i < this.slicedItems.length; i++){
            tmp = this.slicedItems[i].map(item => Number(item.y));
            this.yData.push(tmp[i]);
          }
    
          for (let i = 0; i < this.slicedItems.length; i++){
            tmp = this.slicedItems[i].map(item => Number(item.t));
            this.zData.push(tmp);
          }
        },
    
        slicing(values) {
          let tmp;
          let result = [];
          
          for (let i = 0; i < values.length; i++) {
            tmp = values.filter(item => item.line_id == i);
            if(tmp.length > 0) result.push(tmp)
          }
    
          return result
        }
      }
    };
    
    </script>
    
    <style lang="scss">
    .graphic-style{
      width: 70vw;
      height: 100%;
    }
    </style>