<template lang="pug">
    #searchAndInsert.searchAndInsert
        .marked.mt-2.float-right(v-if="!showMark")
            div 景點資料來源:
                a(href="https://data.gov.tw/dataset/7777") 觀光局
        .main.col-md.col-sm.card.mt-5
            ul.nav.nav-tabs(role="tablist")
                li.nav-item: a#searchTab.nav-link.active(data-toggle="tab" href="#search" role="tab" aria-controls="#search" aria-selected="false") 查詢
                li.nav-item.active: a#insertTab.nav-link(data-toggle="tab" href="#insert" role="tab" aria-controls="#insert" aria-selected="false") 新增
            .tab-content
                #search.tab-pane.fade.show.active
                    .form-group
                        label(for="searchType") 查詢類型
                        select#serchType.form-control(v-model="selectSearchType")
                            option(v-for="item in searchType" :value="item.url") {{ item.text }}
                        label(for="scounty") 縣市
                        select#scounty.form-control(v-model="selectCounty")
                            option(value="null" disabled selected) 請選擇縣市
                            option(v-for="item in county" :value="item.name"  :key="item.id") {{ item.name }}
                        label(for="splace" v-if="selectSearchType=='mw_qryspt01.php' || selectSearchType=='mw_qrycnt02.php' || selectSearchType=='mw_qrycnt03.php'") 景點
                        select#splace.form-control.col(v-model="selectSight" v-if="selectSearchType=='mw_qryspt01.php' || selectSearchType=='mw_qrycnt02.php' || selectSearchType=='mw_qrycnt03.php'")
                            option(value="null" disabled selected) 請選擇景點
                            option(v-for="item in sightsOptions" :value="item.Id") {{ item.Name }}
                        label(for="sdate") 日期
                        br
                        date-picker#sdate.date.form-group(v-model="date" format="YYYY/MM/DD" v-show="selectSearchType!='mw_qrycnt03.php'")
                        date-picker#sdate.date.form-group(v-model="date" type="month" format="YYYY/MM" v-show="selectSearchType=='mw_qrycnt03.php'")
                        br
                        button(@click="submit('search')").form-control.btn.btn-outline-primary.offset-md-1.col-md-5 查詢
                        button(@click="collapse()").form-control.btn.btn-outline-danger.col-md-5 取消
                        .result
                #insert.tab-pane.fade
                    .form-group
                        label(for="scounty") 縣市
                        select#county.form-control(v-model="selectCounty")
                            option(value="null" disabled selected) 請選擇縣市
                            option(v-for="item in county" :value="item.name" :key="item.id") {{ item.name }}
                        label(for="place") 景點
                        //- select#place.form-control.col(v-model="selectSight")
                            option(value="null" disabled selected) 請選擇景點
                            option(v-for="item in sightsOptions" :value="item.Id") {{ item.Name }}
                        Multiselect(
                            v-model="selectInsertSights",
                            :options="sightsOptions"
                            :multiple="true",
                            placeholder="請選擇景點"
                            track-by="Name",
                            label="Name")
                            template(slot="singleLabel", slot-scope="{ sightOptions }") {{ sightOptions.Name }}

                        label(for="peopleNum") 人數
                        input(type="number" v-model="peopleNum" min="0" placeholder="0")#people.form-control.col
                        label(for="date") 日期
                        br
                        date-picker#date.date.form-group(v-model="date" format="YYYY/MM/DD")
                        br
                        button(@click="submit('insert')").form-control.btn.btn-outline-primary.offset-md-1.col-md-5 新增
                        button(@click="cancel()").form-control.btn.btn-outline-danger.col-md-5 取消
                        .result
                .marked.offset-md-6.offset-sm-9.col-md-7.col-sm-8(v-if="showMark")
                    h6 景點資料來源:
                        a(href="https://data.gov.tw/dataset/7777") 觀光局
</template>
<script>
import "bootstrap";
import "bootstrap/dist/css/bootstrap.css";
import axios from 'axios';
import jQuery from 'jquery';
import moment from 'moment';
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import Multiselect from 'vue-multiselect';
import 'vue-multiselect/dist/vue-multiselect.min.css';


export default {
    props: {
        collapse: Function
    },
    data() {
        return {
            returnData: [],
            peopleNum:  null,
            date: new Date(),
            county: [
                {
                    id: 1,
                    name: "臺北市",
                    local: [25.05, 121.56666],
                },
                {
                    id: 2,
                    name: "臺中市",
                    local: [24.18333, 120.61666],
                },
                {
                    id: 3,
                    name:  "基隆市",
                    local: [25.13333, 121.73333],
                },
                {
                    id: 4,
                    name: "臺南市",
                    local: [23.13333, 120.28333],
                },
                {
                    id: 5,
                    name: "高雄市",
                    local: [22.8, 120.35],
                },
                {
                    id: 6,
                    name: "新北市",
                    local: [25.05, 121.56666],
                },
                {
                    id: 7,
                    name: "宜蘭縣",
                    local: [24.76666, 121.75],
                },
                {
                    id: 8,
                    name: "桃園市",
                    local: [24.95, 121.21666],
                },
                {
                    id: 9,
                    name: "嘉義市",
                    local: [23.48333, 120.45],
                },
                {
                    id: 10,
                    name: "新竹縣",
                    local: [24.73333, 121.08333],
                },
                {
                    id: 11,
                    name: "苗栗縣",
                    local: [24.55, 120.81666],
                },
                {
                    id: 12,
                    name: "南投縣",
                    local: [24.05, 120.85],
                },
                {
                    id: 13,
                    name: "彰化縣",
                    local: [23.96666, 120.56666],
                },
                {
                    id: 14,
                    name: "新竹市",
                    local: [24.73333, 121.08333],
                },
                {
                    id: 15,
                    name: "雲林縣",
                    local: [23.66666, 120.38333],
                },
                {
                    id: 16,
                    name: "嘉義縣",
                    local: [23.48333, 120.45],
                },
                {
                    id: 17,
                    name: "屏東縣",
                    local: [22.36666, 120.58333],
                },
                {
                    id: 18,
                    name: "花蓮縣",
                    local: [23.98333, 121.6],
                },
                {
                    id: 19,
                    name: "臺東縣",
                    local: [22.75, 121.15],
                },
                {
                    id: 20,
                    name: "金門縣",
                    local: [24.5, 118.41666],
                },
                {
                    id: 21,
                    name: "澎湖縣",
                    local: [23.56666, 119.55],
                },
                {
                    id: 22,
                    name: "連江縣",
                    local: [26.2, 119.88333],
                },
            ],
            searchType: [
                {
                    text: "查詢單一景點資訊",
                    url: "mw_qryspt01.php"
                }, {
                    text: "查詢單一縣市景點資訊",
                    url: "mw_qryspt02.php"
                }, {
                    text: "查詢單一縣市單一日期人數資訊",
                    url: "mw_qrycnt01.php"
                }, {
                    text: "查詢單一景點單一日期人數資訊",
                    url: "mw_qrycnt02.php"
                }, {
                    text: "查詢單一景點整個月份人數資訊",
                    url: "mw_qrycnt03.php"
                }
            ],
            selectCounty: null,
            selectSight: null,
            selectInsertSights: null,
            selectSearchType: 'mw_qryspt01.php',
            passObj: {},
            responseData: [],
        }
    },
    watch: {
        date() {
            // console.log(moment(this.date).format("YYYY/MM/DD"))
        },
        selectSight() {
            // console.log(this.selectSight)
        },
        selectCounty() {
            // this.selectInsertSights = null;
            // console.log(this.selectCounty);
        },
        selectInsertSights() {
            // console.log(this.selectInsertSights.map(el => el.Id))
        },

    },
    components: {
        DatePicker,
        Multiselect
    },
    created() {
        // let cors = 'https://cors-anywhere.herokuapp.com/';
        // let url = 'https://gis.taiwan.net.tw/XMLReleaseALL_public/scenic_spot_C_f.json';
        let url = './static/sightdata.json'
        axios
            .get(`${url}`)
            .then((result) => result.data)
            .then((jsonData) => {
                // console.log(jsonData.XML_Head)
                this.responseData = jsonData.XML_Head.Infos.Info
            })
           /* .then(() => {
                this.passDataToMap(this.responseData.filter(el=> el.Region == "高雄市"))
            })*/
    },
    mounted() {
        jQuery('#searchTab').on('click', function (e) {
            e.preventDefault();
            jQuery('a[href="#search"]').tab('show')
        });
        jQuery('#insertTab').on('click', function (e) {
            e.preventDefault();
            jQuery('a[href="#insert"]').tab('show')
        });
    },
    computed: {
        sightsOptions() {
            let returnData = this
                .responseData
                .filter(el => el.Region == this.selectCounty);
            return this.selectCounty == null
                ? this.responseData
                : returnData;
        },
        showMark() {
            return jQuery(window).width() > 500 ? true : false;
        }
    },
    methods: {
        getApi() {
			let cors = 'https://cors-anywhere.herokuapp.com/';
            let obj = {
                apikey: 'listennn08776b216a1db5916031137c'
            };
            let url = `${cors}http://menswalk.prjlife.com/${this.selectSearchType}?apikey=${obj.apikey}`
            // let url = `$http://menswalk.prjlife.com/${this.selectSearchType}?apikey=${obj.apikey}`
            switch (this.selectSearchType) {
                case 'mw_qryspt01.php':
                    obj.id = this.selectSight;
					url += `&id=${obj.id}`
                    break;
                case 'mw_qryspt02.php':
                    obj.region = this.selectCounty;
					url += `&region=${obj.region}`
                    break;
                case 'mw_qrycnt01.php':
                    obj.region = this.selectCounty;
                    obj.date = moment(this.date).format('YYYYMMDD');
					url += `&region=${obj.region}&date=${obj.date}`
                    break;
                case 'mw_qrycnt02.php':
                    obj.id = this.selectSight;
                    obj.date = moment(this.date).format('YYYYMMDD');
					url += `&id=${obj.id}&date=${obj.date}`
                    break;
                case 'mw_qrycnt03.php':
                    obj.id = this.selectSight;
                    obj.date = moment(this.date).format('YYYYMM');
					url += `&id=${obj.id}&date=${obj.date}`
                    break;
            };
            // console.log(`${cors}http://menswalk.prjlife.com/${this.selectSearchType}`)

			axios.get(url)
                .then((result) =>  result.data)
                .then((data) => {
                    this.returnData = [];
                    data.forEach(element => {
                        // console.log(element.c)
                        this.returnData.push({
                            id: element.i,
                            name: element.n,
                            add: element.a,
                            local: [element.y, element.x],
                            count: element.c
                        })
                    });
                })
                .then(async() => {
                    this.passObj = {
                        data: this.returnData,
                        type: this.selectSearchType,
                        region: this.county.filter( (el) => el.name == this.selectCounty)[0],
                        date: moment(this.date).format('YYYYMM')
                    }
                    await this.$emit('returnMapData', this.passObj)
                })
        },
        async insertApi() {
            let cors = 'https://cors-anywhere.herokuapp.com/';
            let url = 'http://menswalk.prjlife.com/';
            url = `${cors}${url}`
            let obj = {
                apikey: 'listennn08776b216a1db5916031137c',
                // id: this.selectSight,
                date: moment(this.date).format('YYYYMMDD'),
                count: this.peopleNum
            }
            axios.all(this.selectInsertSights.map((el, index) => {
                return axios.get(`${url}${this.selectSearchType}?apikey=${obj.apikey}&id=${el.Id}&date=${obj.date}&count=${obj.count}`)
                    .then((result) => {
                //     console.log(result)
                //     if (result.status == 200) {
                //         alert('上傳資料成功');
                //     }
                        try {
                            return {
                                id: el.Id,
                                status: result.status
                            }
                        } catch (error) {
                            return {
                                id: index,
                                status: error
                            }
                        }

                    })
            })).then( (statusArray) => statusArray.filter(el => {
                    if (el.status != 200) return el.id;

            })).then( (errorArray) => {
                if (!errorArray.length) {
                     alert('全部景點已上傳完成!')
                } else {
                    let sight = [];
                    errorArray.forEach( (el) => {
                        return this.responseData.filter( (data) => {
                            if (data.Id == el.id) {
                                sight.push(data.Name);
                            }
                        })
                    })
                    alert(`未上傳完成景點： ${sight.join(', ')}`)
                }
            })
            this.selectCounty = null;
            this.selectInsertSights = null;
        },
        submit(type) {
            switch (type.toLowerCase()) {
                case 'search':
                    if (this.selectSearchType == "mw_qryspt01.php" || this.selectSearchType == "mw_qrycnt02.php") {
                        if (!this.selectSight) {
                            alert('請選擇景點欄位!');
                            return;
                        }
                    } else {}
                    this.getApi();
                    break;
                case 'insert':
                    if (type == 'insert') {
                        this.selectSearchType = "mw_addcnt01.php";
                    }
                    if (this.selectSight == '') {
                        alert('請填寫景點!');
                        return;
                    }
                    if (!this.peopleNum || this.peopleNum <= 0) {
                        alert('請填寫人數, 人數不可小於1');
                        return;
                    }
                    this.insertApi();
                    break;
                default:
                    alert('Error');

            }
            this.collapse();
        },
        passDataToMap() {
            arguments[0].forEach((el) => {
                this.returnData.push({
                            id: el.Id,
                            name: el.Name,
                            add: el.Add,
                            local: [el.Py, el.Px],
                            count: Math.floor(Math.random() * 20000)
                        })
            })

            this.$emit('returnMapData', { data: this.returnData, type: this.selectSearchType, region: this.selectCounty });
            // this.$emit('test', arguments[0])
        },
        cancel() {
            this.selectInsertSights = null;
            this.selectSight = null;
            this.selectCounty = null;
            this.collapse();
        }
    }
}
</script>
<style lang="sass" scoped>
.date
    width: 100%
</style>
