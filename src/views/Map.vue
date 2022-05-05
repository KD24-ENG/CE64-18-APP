<template>
    <div class="map" v-cloak>
        <v-row>
            <v-col class="d-flex ml-10" elevation="3" cols="4" sm="3">
                <v-select :items="items" label="เลือกถนน" solo></v-select>
            </v-col>
            <v-checkbox v-model="checkbox">
                <template v-slot:label>
                    <div>
                        ปัจจุบัน
                    <v-tooltip bottom>
                <template v-slot:activator="{ on }">
                <a
                target="_blank" href="https://vuetifyjs.com" @click.stop v-on="on">
                </a>
            </template>
            Opens in new window
                </v-tooltip>
        </div>
                </template>
    </v-checkbox>        <div class="ts"><p class="grey--text text--darken-2 body-2 font-weight-bold"> {{time}} </p></div>
        </v-row>
        <l-map class="buttom" style="height: 610px" :zoom="zoom" :center="center">
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
        <l-polyline class="lat01" :l-style="{weight: 5 }" :lat-lngs="polyline01.lat01" :color="polyline01.color"></l-polyline>
        <l-polyline class="lat02" :l-style="{weight: 5 }" :lat-lngs="polyline02.lat02" :color="polyline02.color"></l-polyline>
        <l-polyline class="lat1" :l-style="{weight: 5 }" :lat-lngs="polyline1.lat1" :color="polyline1.color"></l-polyline>
        <l-polyline class="lat2" :l-style="{weight: 5 }" :lat-lngs="polyline2.lat2" :color="polyline2.color"></l-polyline>
        <l-polyline class="lat3" :l-style="{weight: 5 }" :lat-lngs="polyline3.lat3" :color="polyline3.color"></l-polyline>
        <l-polyline class="lat4" :l-style="{weight: 5 }" :lat-lngs="polyline4.lat4" :color="polyline4.color"></l-polyline>
        <l-polyline class="lat5" :l-style="{weight: 5 }" :lat-lngs="polyline5.lat5" :color="polyline5.color"></l-polyline>
        <l-polyline class="lat6" :l-style="{weight: 5 }" :lat-lngs="polyline6.lat6" :color="polyline6.color"></l-polyline>
        </l-map>
    </div>
</template>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
<script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>

<script>
import {LMap, LTileLayer, LPolyline} from 'vue2-leaflet';
import axios from 'axios';

export default {
    data () {
    return {
        items: ['ราชวิถี', 'นครสวรรค์', 'พระราม5', 'ลาดพร้าว', 'ศรีอยุธยา', 'สามเสน', 'หลานหลวง', 'พระราม4', 'พญาไท', 'ดินแดง', 'พระราม9', 'ราชปรารภ', 'จตุรทิศ', 'พิษณุโลก', 'เพชรบุรี', 'สวรรคโลก', 'เทอดดำริ', 'นครไชยศรี', 'มหาไชย', 'ราชดำเนินนอก', 'กรุงเกษม', 'บรรทัดทอง', 'ประดิพัทธ์', 'สมเด็จพระปิ่นเกล้า', 'ราชดำริ', 'รางน้ำ', 'บำรุงเมือง', 'สิรินธร', 'พระราม8', 'สุทธิสารวินิจฉัย', 'พระสุเมรุ', 'บวรนิเวศน์', 'ไกรสีห์', 'ศรีอยุธยา', 'เพชรอุทัย', 'หลวง', 'พลับพลาไชย', 'ยุคล1', 'ยุคล2', 'จักรวรรดิ', 'เยาวราช', 'จักรเพชร', 'ราชดำเนินกลาง', 'ดินสอ', 'จักรพรรดิ์พงษ์', 'ตะนาว', 'ประชาธิปไตย', 'อรุณอมรินทร์', 'ราชดำเนินใน', 'สนามไชย', 'ราชินี', 'พระปกเกล้า', 'อิสรภาพ', 'ทหาร', 'เตชะวณิช', 'นครราชสีมา', 'รองเมือง', 'พระราม1', 'ประชาธิปก', 'สมเด็จเจ้าพระยา', 'อังรีดูนังต์', 'เจริญกรุง', 'วิทยุ', 'ลาดหญ้า', 'อินทรพิทักษ์', 'สมเด็จพระเจ้าตากสิน', 'เพชรเกษม', 'ประดิษฐ์มนูธรรม', 'นิคมมักกะสัน', 'สีลม', 'เจริญนคร', 'สุทธาวาส', 'เลียบทางรถไฟตลิ่งชัน', 'บรมราชชนนี', 'รัชดาภิเษก', 'เทอดไท', 'สาทรเหนือ', 'ประชาราษฏร์ สาย1', 'จรัญสนิทวงศ์', 'มเหสักข์', 'มหานคร', 'มหาเศรษฐ์', 'มหาพฤฒาราม', 'สันติภาพ', 'มิตรพันธ์', 'ไมตรีจิตต์', 'นราธิวาสราชนครินทร์', 'สุรวงศ์', 'สี่พระยา', 'ทรงวาด', 'ราชวงศ์', 'มิตรภาพไทย-จีน', 'ตรีมิตร', 'ทรงเสริม', 'ทรงสวัสดิ์', 'วานิช1', 'เยาวพานิช', 'ผดุงด้าว', 'แปลงนาม', 'พลับพลาไชย', 'สารสิน', 'จันทร์', 'คอนแวนต์', 'ศาลาแดง', 'ข้าวหลาม', 'มังกร', 'สุขุมวิท71', 'เอกมัย', 'ดวงพิทักษ์', 'หับเผย', 'หลักเมือง', 'เพลินจิต', 'รัชดาภิเษก', 'อโศกมนตรี', 'สาธุประดิษฐ์', 'ยานนาวา', 'เจริญราษฎร์', 'นางลิ้นจี่', 'สวนพลู', 'รามคำแหง', 'ลาดพร้าว64', 'วัฒนธรรม', 'เทียมร่วมมิตร', 'ประชาอุทิศ', 'หัวหมาก', 'พัฒนาการ', 'อ่อนนุช', 'ศรีนครินทร์', 'วชิรธรรมสาธิต', 'นวมินทร์', 'กรุงเทพกรีฑา', 'ศรีนครินทร์-ร่มเกล้า'],
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
        attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        zoom: 15,
        center: [13.76492902071622, 100.53823436762788],
        time: [],
        polyline01: {
            lat01: [],
            color: 'MediumSeaGreen'
        },
        polyline02: {
            lat02: [],
            color: 'MediumSeaGreen'
        },
        polyline1: {
            lat1: [],
            color: 'red'
        },
        polyline2: {
            lat2: [],
            color: 'yellow'
        },
        polyline3: {
            lat3: [],
            color: 'green'
        },
        polyline4: {
            lat4: [],
            color: 'red'
        },
        polyline5: {
            lat5: [],
            color: 'yellow'
        },
        polyline6: {
            lat6: [],
            color: 'green'
        }
        
        }       
    },
        created() {
            axios.get('https://kd24-eng.github.io/bkk/time.json')
            .then((response) => {
                    this.time = response.data.response[0].ts
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/fwd0.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let flat0 = []
                flat0.push(response.data.response[i].fwdlat0)
                flat0.push(response.data.response[i].fwdlong0)

                let flat1 = []
                flat1.push(response.data.response[i].fwdlat1)
                flat1.push(response.data.response[i].fwdlong1)
                
                let temp = []
                temp.push(flat0)
                temp.push(flat1)

                temp1.push(temp)
                }

                this.polyline01.lat01 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/bwd0.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let blat0 = []
                blat0.push(response.data.response[i].bwdlat0)
                blat0.push(response.data.response[i].bwdlong0)

                let blat1 = []
                blat1.push(response.data.response[i].bwdlat1)
                blat1.push(response.data.response[i].bwdlong1)
                
                let temp = []
                temp.push(blat0)
                temp.push(blat1)

                temp1.push(temp)
                }

                this.polyline02.lat02 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/fwd1.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                    let flat0 = []
                flat0.push(response.data.response[i].fwdlat0)
                flat0.push(response.data.response[i].fwdlong0)

                let flat1 = []
                flat1.push(response.data.response[i].fwdlat1)
                flat1.push(response.data.response[i].fwdlong1)
                
                let temp = []
                temp.push(flat0)
                temp.push(flat1)

                temp1.push(temp)
                }

                this.polyline1.lat1 = temp1
            })
            .catch((err) => {
                console.log(err)
            })
            
            axios.get('https://kd24-eng.github.io/bkk/fwd2.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let flat0 = []
                flat0.push(response.data.response[i].fwdlat0)
                flat0.push(response.data.response[i].fwdlong0)

                let flat1 = []
                flat1.push(response.data.response[i].fwdlat1)
                flat1.push(response.data.response[i].fwdlong1)
                
                let temp = []
                temp.push(flat0)
                temp.push(flat1)

                temp1.push(temp)
                }

                this.polyline2.lat2 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/fwd3.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let flat0 = []
                flat0.push(response.data.response[i].fwdlat0)
                flat0.push(response.data.response[i].fwdlong0)

                let flat1 = []
                flat1.push(response.data.response[i].fwdlat1)
                flat1.push(response.data.response[i].fwdlong1)
                
                let temp = []
                temp.push(flat0)
                temp.push(flat1)

                temp1.push(temp)
                }

                this.polyline3.lat3 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/bwd1.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let blat0 = []
                blat0.push(response.data.response[i].bwdlat0)
                blat0.push(response.data.response[i].bwdlong0)

                let blat1 = []
                blat1.push(response.data.response[i].bwdlat1)
                blat1.push(response.data.response[i].bwdlong1)
                
                let temp = []
                temp.push(blat0)
                temp.push(blat1)

                temp1.push(temp)
                }

                this.polyline4.lat4 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/bwd2.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let blat0 = []
                blat0.push(response.data.response[i].bwdlat0)
                blat0.push(response.data.response[i].bwdlong0)

                let blat1 = []
                blat1.push(response.data.response[i].bwdlat1)
                blat1.push(response.data.response[i].bwdlong1)
                
                let temp = []
                temp.push(blat0)
                temp.push(blat1)

                temp1.push(temp)
                }

                this.polyline5.lat5 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

            axios.get('https://kd24-eng.github.io/bkk/bwd3.json')
            .then((response) => {
                let temp1 = []

                for (let i =0; i< response.data.response.length; i++) {

                let blat0 = []
                blat0.push(response.data.response[i].bwdlat0)
                blat0.push(response.data.response[i].bwdlong0)

                let blat1 = []
                blat1.push(response.data.response[i].bwdlat1)
                blat1.push(response.data.response[i].bwdlong1)
                
                let temp = []
                temp.push(blat0)
                temp.push(blat1)

                temp1.push(temp)
                }

                this.polyline6.lat6 = temp1
            })
            .catch((err) => {
                console.log(err)
            })

        },
    components: {
    LMap,
    LTileLayer,
    LPolyline,
    },
}
</script>

<style type="text/css">
.buttom {
    z-index: 3;
    margin-top: -30px;
    margin-bottom: 10px;
}
.map {
    margin: -20px 15px 10px 15px;
}
.ts {
    margin-top: px;
}
</style>