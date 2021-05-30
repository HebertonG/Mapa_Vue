<template>
    <div>
        <div style="display: flex;">
            <button @click="initMap('DRIVING')">Driving</button>
            <button @click="initMap('WALKING')">Walking</button>
            <button @click="initMap('BICYCLING')">Bicycling</button>
            <button @click="initMap('TRANSIT')">Transit</button>
        </div>

        <div id="map">
        </div>
    </div>
</template>
    <script>
    import { defineComponent } from '@vue/composition-api'
    
    export default defineComponent({
        setup() {
            
        },
    })
    </script>
    
    <script>
        export default {
            name: 'Map',
            data() {
                return {
                    map: null,
                    mapCenter: {
                        lat: 37.773972,
                        lng: -122.431297
                    },
                    restaurant: {
                        lat: 37.7928015,
                        lng: -122.3940881
                    }
                }
            },

            mounted() {
                this.initMap()
                this.setMarker(this.mapCenter, "A")
                this.setMarker(this.restaurant, "B")
            },

            methods: {
                initMap(travels) {
                    let dS = new google.maps.DirectionsService();
                    let dD = new google.maps.DirectionsRenderer();

                    this.map = new google.maps.Map(document.getElementById('map'), {
                        center: this.mapCenter,
                        zoom: true,
                        maxZoom: 20,
                        minZoom: 3,
                        streetViewControl: true,
                        mapTypeControl: true,
                        fullscreenControl: true,
                        zoomControl: true,
                    })

                    let request = {
                        origin: this.mapCenter,
                        Destination: this.restaurant,
                        travelMode: travels
                    };

                    dD.setMap(this.ourMap);

                    dS.route(request, function(result, status) {
                        if (status == "OK") {
                            console.log(result)
                            let steps = result.routes[0].legs[0].steps;
                            steps.forEach((res, key) => {
                                const markers = new google.maps.MArker({
                                    position: {
                                        lat: res.start_location.lat(),
                                        lng: res.start_location.lng()
                                    },
                                    map: this.ourMap,
                                    label: {
                                        text: "",
                                        color: "#fff"
                                    }
                                })
                            })
                            dD.setDirections(result);
                        } else {
                            console.log(status);
                        }
                    })
                },

                setMarker(Points, Label) {
                    const markers = new google.maps.Marker({
                        position: Points,
                        map: this.map,
                        label: {
                            text: Label,
                            color: "#FFF"
                        }
                    })
                },

            }
        }
    </script>