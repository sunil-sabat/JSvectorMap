<script >
	import jsVectorMap from 'jsvectormap';
	import 'jsvectormap/dist/maps/world'
	import "jsvectormap/dist/css/jsvectormap.css";
	var markers = [{
        name: '456 Russia',
        coords: [61, 105],
        // style: {
        //   r: 8
        // }
      },
      {
        name: '238 Greenland',
        coords: [72, -42],
        style: {
          r: 8,
        }
      },
      {
        name: '455 Canada',
        coords: [56, -106],
        style: {
          r: 8,
        }
      },
      {
        name: "990 India" ,
        coords: [20.5937, 78.9629],
        style: {
          r: 7,
        }
      },
      {
        name: '657 Brazil',
        coords: [-14.2350, -51.9253],
      },
      {
        name: 'China',
        coords: [35.8617, 104.1954],
        style: {
          image: ""
        },
        offsets: [2, 2]
      },
    ];
let upperMap;
const createMap = ()=> {
	var map = new jsVectorMap({
      map: 'world',
      selector: '#map-wrapper',

      regionsSelectable: true,
      markersSelectable: true,
      selectedMarkers: [0],
      // -------- Events --------
      onRegionSelected: function (index, isSelected, selectedRegions) {
        console.log(index, isSelected, selectedRegions);
      },
      onMarkerSelected: function (code, isSelected, selectedMarkers) {
        //console.log('llllllllllllllllllllll')
        console.log(code, isSelected, selectedMarkers)
      },
      onRegionTooltipShow: function (tooltip, code) {
        
        if (code === 'RU') {
          tooltip.getElement().innerHTML = tooltip.text() + ' <b>(900)</b>'
        }else if(code === "IN" ){
          tooltip.getElement().innerHTML = tooltip.text() + ' <b>(900)</b>'
        }
      },
      onMarkerTooltipShow: function (tooltip, index) {
       
        console.log(tooltip)
        
        tooltip.getElement().innerHTML = `<div class="bg-success" style='background-color:white; display:flex; overflow:hidden'><p  style='color:green'>${tooltip.text()}<p/><img alt='tree' src='LemonTree3.svg'/></div>`
      },
     
      // -------- Labels --------
      labels: {
        markers: {
          render: function(marker) {
            //console.log(cc,mm , marker)
            return marker.name
          },
          offsets: function(index) {
            return markers[index].offsets || [0, 0]
          }
        },
        regions: {
          render: function(code) {
            var codes = ['EG', 'KZ', 'CN']

            if (codes.indexOf(code) > -1) {
              return ''
            }
          },
        }
      },

      // -------- Region and label style --------
      regionStyle: {
        selected: {
          fill: "#5c5cff"
        }
      },
      regionLabelStyle: {},

      // -------- Marker and label style --------
      markers: markers,
      markerStyle: {
        initial: {
          fill: 'red'
        },
        hover: {
          stroke: "#4bdc77",
          strokeWidth: 1,
          fill: "#4bdc77"
        },
        selected: {
          fill: 'red'
        }
      },
      markerLabelStyle: {
        initial: {
          fontFamily: 'Poppins',
          fontSize: 18,
          fontWeight: 500,
          fill: '#35373e',
        },
      },

      // -------- Series --------
      series: {
        markers: [
          {
            attribute: "fill",
            legend: {
              title: "Something (marker)",
              // vertical: true,
            },
            scale: {
              "Criteria one": "#ffd400",
              "Criteria two": "#4761ff"
            },
            values: {
              0: "Criteria one",
              1: "Criteria two",
              2: "Criteria two"
            }
          },
        ],
       

        // ---------- Region series ----------
        regions: [
          {
            attribute: 'fill',
            attributes: {
              // EG: 'red'
            },
            legend: {
              title: 'Some title (region)',
              vertical: true,
            },
            scale: {
              "Criteria": "#4bdc77",
              "Another Criteria": "#ff5566"
            },
            values: {
              GB: "Another Criteria",
              MX: "Criteria",
              // LY: "Criteria",
            },
          },
        ]
      },
    })
	upperMap = map
}

function getHtmlElement(selector) {
      return document.querySelector(selector)
    }


const reset = ()=>{
	upperMap.reset()
}

const getRegions = () =>{
	 if (! upperMap.getSelectedRegions().length) {
        alert('No regions selected')
      } else {
        alert(map.getSelectedRegions())
      }
}
    
const clearRegions = ()=>{
	upperMap.clearSelectedRegions()
}
   
const stringToHTML = function (str) {
	var parser = new DOMParser();
	var doc = parser.parseFromString(str, 'text/html');
  console.log(doc.body , doc)
	return doc.body;
};

const getMarkers = () =>{
	var selectedMarkers = upperMap.getSelectedMarkers()
	
	if (! selectedMarkers.length) {
	  alert('No regions markers')
	} else {
	  alert(selectedMarkers)
	}
}
    
const clearMarkers = () =>{
	upperMap.clearSelectedMarkers()
}

const addMarker=()=>{
	upperMap.addMarker({
	  name: 'Egypt',
	  coords: [26.8, 30],
	  offsets: [0, 0]
	})
}
    

const changeBgColor = ()=>{
	var colors = ['#5c5cff', '#ff9251', '#56de80', '#FFF', '#000', '#f5d4f5'],
	index = Math.floor((Math.random() * colors.length - 1) + 1)
	upperMap.setBackgroundColor(colors[index])
}
  

   


</script>

<main>
	<div class="text-center mt-4">

		<div id="map-wrapper" use:createMap style="width: 750px; height: 400px; margin: auto; border: 1px solid #EEE"></div>
	
		<div class="container mt-4">
		  <button id="change-bg-color" on:click={changeBgColor} class="btn btn-primary">Change bg color randomly</button>
		  <button id="reset" on:click={reset} class="btn btn-primary">Reset Map</button>
		  <button id="get-regions" on:click={getRegions} class="btn btn-primary">Get selected regions</button>
		  <button id="clear-regions"  on:click={clearRegions} class="btn btn-primary">Clear selected regions</button>
		</div>
	
		<div class="container mt-1">
		  <button id="get-markers" on:click={getMarkers} class="btn btn-primary">Get selected markers</button>
		  <button id="clear-markers" on:click={clearMarkers} class="btn btn-primary">Clear selected markers</button>
		  <button id="add-marker" on:click={addMarker} class="btn btn-primary">Add Marker</button>
		</div>
	  </div>
	
	
	
</main>

<style>
	.green {
    background-color: greenyellow;
  }

  .tooltip-bg-color{
    background-color: greenyellow;
  }
  .jvm-tooltip{
    background-color: greenyellow;
  }

  
</style>