<template lang="pug">
    #searchAndInsert
        .main.col.card.mt-5
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
                            option(v-for="item in county") {{ item }}
                        label(for="splace" v-if="selectSearchType=='mw_qryspt01.php' || selectSearchType=='mw_qrycnt02.php'") 景點
                        select#splace.form-control.col(v-model="selectSight" v-if="selectSearchType=='mw_qryspt01.php' || selectSearchType=='mw_qrycnt02.php'")
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
                            option(v-for="item in county") {{ item }}
                        label(for="place") 景點
                        select#place.form-control.col(v-model="selectSight")
                            option(value="null" disabled selected) 請選擇景點
                            option(v-for="item in sightsOptions" :value="item.id") {{ item.Name }}
                        label(for="peopleNum") 人數
                        input(type="number" v-model="peopleNum")#people.form-control.col
                        label(for="date") 日期
                        br
                        date-picker#date.date.form-group(v-model="date" format="YYYY/MM/DD")
                        br
                        button(@click="submit('insert')").form-control.btn.btn-outline-primary.offset-md-1.col-md-5 新增
                        button(@click="collapse()").form-control.btn.btn-outline-danger.col-md-5 取消
                        .result
                #marked.offset-md-6.offset-sm-9.col-md-7.col-sm-8
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

export default {
    data() {
        return {
            returnData: [],
            peopleNum: 0,
            date: new Date(),
            county: [
                "請選擇縣市",
                "宜蘭縣",
                "花蓮縣",
                "金門縣",
                "南投縣",
                "屏東縣",
                "苗栗縣",
                "桃園市",
                "高雄市",
                "基隆市",
                "連江縣",
                "雲林縣",
                "新北市",
                "新竹市",
                "新竹縣",
                "嘉義市",
                "嘉義縣",
                "彰化縣",
                "臺中市",
                "臺北市",
                "臺東縣",
                "臺南市",
                "澎湖縣"
            ],
            selectCounty: "請選擇縣市",
            selectSight: null,
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
            selectSearchType: 'mw_qryspt01.php',
            responseData: []
        }
    },
    watch: {
        date() {
            console.log(moment(this.date).format("YYYY/MM/DD"))
        },
    },
    components: {
        DatePicker
    },
    created() {
        let cors = 'https://cors-anywhere.herokuapp.com/';
        let url = 'https://gis.taiwan.net.tw/XMLReleaseALL_public/scenic_spot_C_f.json';
        axios
            .get(`${cors}${url}`)
            .then((result) => result.data)
            .then((jsonData) => {
                this.responseData = jsonData.XML_Head.Infos.Info
                // console.log(this.responseData[0])
            })
            .then(() => {
                this.passDataToMap(this.responseData.filter(el=> el.Region == "高雄市"))
            })
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
            return this.selectCounty == "請選擇縣市"
                ? this.responseData
                : returnData;
        }
    },
    methods: {
        getApi() {
			let cors = 'https://cors-anywhere.herokuapp.com/';
            let obj = {
                apikey: 'listennn08776b216a1db5916031137c'
            };
            let url = `${cors}http://menswalk.prjlife.com/${this.selectSearchType}?apikey=${obj.apikey}`
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
                .then((result) => {
                    console.group('result');
                    console.log(result);
                    console.groupEnd();
                    return result.data
                })
                .then((data) => {
                    this.returnData = [];
                    data.forEach(element => {
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
                    await this.$emit('returnMapData', this.returnData)
                })
        },
        insertApi() {
            let url = 'https://cors-anywhere.herokuapp.com/http://menswalk.prjlife.com/';
            let obj = {
                apikey: 'listennn08776b216a1db5916031137c',
                id: this.selectSight,
                date: moment(this.date).format('YYYYMMDD'),
                count: this.peopleNum
            }
            axios.post(`${url}${this.searchType}?apikey=${obj.apikey}&id=${obj.id}&date=${obj.date}&count=${obj.count}`)
                .then((result) => {
                    if (result.state == 200) {
                        alert('上傳資料成功')
                    }
                })
        },
        submit(type) {
            switch (type.toLowerCase()) {
                case 'search':
                    if (this.selectSearchType == "mw_qryspt01.php" || this.selectSearchType == "mw_qrycnt02.php") {
                        if (!this.selectSight) {
                            alert('請檢查景點欄位');
                            return;
                        }
                    } else {}
                    this.getApi();
                    break;
                case 'insert':
                    if (type == 'insert') {
                        this.selectSearchType = "mw_addcnt01.php";
                    }
                    if (this.peopleNum == 0 || this.selectSight == '') {
                        alert('請檢查欄位, 人數不可為0');
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
                            count: el.c
                        })
            })

            this.$emit('returnMapData', this.returnData);
            // this.$emit('test', arguments[0])
        },
        collapse() {
            // console.log(jQuery('#searchAndInsert').css('width') == "0px");
            if (jQuery('#searchAndInsert').css('width') == "0px") {
                jQuery('#searchAndInsert').css('width', "30%");
                jQuery("#map").css('width', "60%");
            } else {
                jQuery('#searchAndInsert').css('width', "0");
                jQuery("#container").css('margin-left', "0");
                jQuery("#map").css('width', "100%");
            }
        }
    }
}
</script>
<style lang="sass" scoped>
.date
    width: 100%
</style>
