<template>
    <div> id="test">
        <Heremap
            ref="map"
            apiKey="xJnFOabQsbR6Khst-nSvg4ynGv3vRP_Id1TgGIF5DK0"
            appId="0JK53jN7Faa5a5rF35Sz"
            appCode="SqWcupOF8jY3JHYYKD0NSw"
            latitude="37"
            longitude="-121"
            zoom="10"
        />
    </div>
</template>

<script>
    import HereMap from '@/components/HereMap.vue'

    export default {
        name: 'test',
        components: {
            HereMap
        },
        mounted () {
            let map = this.$refs.map;
            map.drawRoute(
                {
                    lat: "37",
                    lng: "-121"
                },
                {
                    lat: "38",
                    lng: "-122"
                }
            )
        }
    }
</script>

<style>
    body {
        margin: 0;
    }
</style>