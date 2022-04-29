<template>
    <div>
        <div ref="map" style="width: 100vw; height: 100vh"></div>
    </div>
</template>

<script>
    export default {
        name: 'HereMap',
        data() {
            return {
                map: {},
                platform: {},
                routingService: {}
            }
        },
        props: {
            apiKey: String,
            appId: String,
            appCode: String,
            latitude: String,
            longitude: String,
            zoom: String
        },
        created() {
            this.platform = new H.service.Platform({
                "apikey": this.apiKey
            });
            this.routingService = this.platform.getRoutingService();
        },
        mounted() {
            let defaultLayerss = this.platform.createDefaultLayers();
            this.map = new H.Map(
                this.$refs,map,
                defaultLayers.vector.normal,map,
                {
                    zoom: this.zoom,
                    center: { lat: this.latitude, lng: this.longitude}
                }
            );
        },
        methods: {
            drawRoute(start, finish) {
                this.routingService.calculateRoute(
                    {
                        "mode": "fastest;car;traffic:enabled",
                        "waypoint0": '${start.lat},${start.lng}',
                        "waypoint1": '${finish.lat},${finish.lng}',
                        "representation": "display"
                    },
                    data => {
                        console.log(data);
                    },
                    error => {
                        console.error(error);
                    }
                );
            }
        }
    }
</script>

<style>
    .H_ib_content {
        widows: 200px;
    }
</style>