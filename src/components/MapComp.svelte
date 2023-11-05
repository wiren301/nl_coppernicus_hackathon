  <script>
      import Progress from '../components/ProgressBar.svelte';


    let map;
    let L;
    let coordinates = []
    let current_floods_polygon = [
        [
          [29.946681, -90.064208],
          [29.937178, -90.061303],
          [29.945863, -90.054186]
        ],
        [
          [29.9520, -90.0565],
          [29.9505, -90.0375],
          [29.9320, -90.0390],
          [29.9345, -90.0580]
        ],
        [
          [29.9575, -90.1015],
          [29.9600, -90.0825],
          [29.9440, -90.0800],
          [29.9415, -90.0990]
        ],
        [
          [29.9750, -90.0610],
          [29.9785, -90.0410],
          [29.9680, -90.0385],
          [29.9655, -90.0585]
        ]
      ];
    
    let current_floods_circle = [
      [29.9610, -90.0206],
      [30.0125, -90.1153],
      [30.0337, -89.9489],
      [29.9688, -90.0789],
      [30.0049, -90.0661]
    ]

    let current_floods_circle_radius = [
      1000,
      1500,
      3000,
      1200,
      1500
    ]
    

    $: if (!L) {
      import('leaflet').then(leafletModule => {
        L = leafletModule.default;

        // Map initialization moved inside reactive block so it initializes as soon as L is ready
        map = L.map('map').setView([29.942240, -90.100495], 12);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);
      }).catch(error => {
        console.error("Error initializing the map:", error);
      });
    }

    function addMarker() {
    if (L && map) {

      const combinedMarkers = [];
      
      for (let i = 0; i < current_floods_circle.length; i++) {
        combinedMarkers.push({
          type: 'circle',
          data: current_floods_circle[i],
          options: {
            color: 'red',
            fillColor: '#f03',
            fillOpacity: 0.5,
            radius: current_floods_circle_radius[i]
          }
        });
      }
      
      for (let i = 0; i < current_floods_polygon.length; i++) {
        combinedMarkers.push({
          type: 'polygon',
          data: current_floods_polygon[i],
          options: {color: 'blue'}
        });
      }

      // Shuffle the combined array
      combinedMarkers.sort(() => Math.random() - 0.5);

      // Calculate the time interval for adding each marker
      const interval = 30000 / combinedMarkers.length;

      combinedMarkers.forEach((markerData, index) => {
        setTimeout(() => {
          let marker;
          if (markerData.type === 'circle') {
            marker = L.circle(markerData.data, markerData.options).addTo(map).bindTooltip("< 15 days");
            
            // Set an interval to gradually decrease the radius
            const decreaseInterval = setInterval(() => {
                const currentRadius = marker.getRadius();
                if (currentRadius <= 0) {
                    clearInterval(decreaseInterval);
                    marker.remove();
                } else {
                    marker.setRadius(currentRadius - 10);
                }
            }, 50);

          } else if (markerData.type === 'polygon') {
            marker = L.polygon(markerData.data, markerData.options).addTo(map).bindTooltip(markerData.data.toString());
            
            setTimeout(() => {
                marker.remove();
            }, interval * (index + 1));  // This ensures that polygons are removed in sequence.
          }
        }, index * interval);
      });
    }
}

    
  let progressBar;
  
  function handleStart() {
    progressBar.startProgress();
  }

  function start() {
    addMarker();
    handleStart();
  }



  </script>

  <svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  </svelte:head>


  <div class="h-[90%] w-[100%] p-4 border-2 border-gray-600" id="map"></div>

  <div class="flex space-x-2 mt-2 items-center">
    <img class="w-10 h-10 cursor-pointer" src ="OIP.jpg" on:click={start}/> 
    <Progress bind:this={progressBar} />    
  </div>
  
