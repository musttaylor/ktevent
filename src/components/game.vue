<!DOCTYPE html>
<template>
  <div class="ktEvent">
    <body margin="0">
      <!--button id="show-modal" @click="showModal = true">Show Modal</button-->
      <!-- use the modal component, pass in the prop -->
      <modal :show="showModal">
        <template #header>
          <h3>custom header</h3>
        </template>
      </modal>
      <ul>
        <li>
          <img id="logo" src="../assets/kt_logo.png">
        </li>
        <li>
          <img id="img-desc-top" src="../assets/img_desc_top.png">
        </li>
      </ul>
      <!--div class="container-box">
        <div class="container-row">
          <div class="overlap">
            <img class="over-img" src="../assets/underline_2.png" alt="Snow">
            <div class="over-text">게임도</div>
          </div>
          <div class="normal-text">하고</div>
        </div>
        <div class="container-row">
          <div class="overlap">
            <img class="over-img" src="../assets/underline_2.png" alt="Snow">
            <div class="over-text">선물도</div>
          </div>
          <div class="normal-text">받고!</div>
        </div>
        <div class="container-row">
          <span>지금 플레이하세요~</span>
        </div>
      </div-->
    </body>
    <body>
      <div class="container-game">
        <img class="device-img" v-bind:src="deviceImage">
        <img class="game-img" v-bind:width="imgWidth" v-bind:src="gameImage">
        <form class="button-form" v-bind:style="{top: btnPlayTop}">
          <a v-bind:href= "gameURI">
            <img v-bind:src="btnPlayImage">
          </a>
        </form>
      </div>
    </body>
    <body :bgcolor="'#f1f6f6'">
      <div class="container-titleEvent">
        <span id="titleEvent"></span>
      </div>
      <div class="container-eventPeriod">
        <img class="bg-eventPeriod" src="../assets/bg_period.png">
        <div class="inner-container-eventPeriod">
          <span class="span-eventPeriod" id="eventPeriod-caption">이벤트기간</span>
          <span class="span-eventPeriod" id="eventPeriod-date"></span>
        </div>
      </div>
      <div id="table-wrapper">
        <div id="table-scroll">
          <table id="table-prize" v-bind:key="items.key">
            <template v-for="(item, idx) in items">
              <tr id="tr-item" v-bind:key="item.key">
                <td id="td-image">
                  <img id="td-li-image" height="80px" width="80px" :src="prizeUrl+'/'+storeIdx+'/'+prizeImgName+(idx+1)+prizeImgExt" />
                </td>
                <td>
                  <ul>
                    <li class="td-li-title">
                      <span class="td-span-title">경품 {{idx+1}}</span>
                    </li>
                    <li :max-width="prizeDescWidth" class="td-li-desc">
                      {{item.name}}
                    </li>
                  </ul>
                </td>
              </tr>
              <tr v-bind:key="item.key">
                <td id="tr-div-line" colspan="2"/>
              </tr>
            </template>
          </table>
        </div>
      </div>
      <div class="container-bottom-info" id="container-bottom-info">
        <ul class="bottom-info" id="bottom-info">
          <li>게임 참여하고 당첨 시 캡처 화면 저장 후 매장 방문 시 혜택 제공</li>
          <li>증정품 조기 소진으로 이벤트가 종료될 수 있음</li>
          <li>증정품의 반품이나 교환은 불가능</li>
        </ul>
      </div>
    </body>
  </div>
</template>

<script>
import axios from 'axios'
import Modal from './Modal.vue'
const baseURI = 'https://api.ktddoddostore.com/events'
const gameURI = ''
var resultJson
var storeIdx = -1
var currentUrl = window.location.href
var gameImage = require('../assets/img_game_roulette.png')
var deviceImage = require('../assets/device_bg_small.png')
var btnPlayImage = require('../assets/btn_play.png')
var gameType = -1
var gameName = 'mgkt_ladder'
var imgWidth = 208
var imgHeight = 208
var prizeDescWidth = 145
var prizeImgMargin = '0px 0px 0px 40px'
var btnPlayTop = 345
var items = [ {
  prob: '',
  name: ''
}]
export default {
  name: 'game',
  components: {
    Modal
  },
  data () {
    return {
      items,
      prizeUrl: 'https://static.ktddoddostore.com/user_img',
      prizeImgName: 'prize',
      prizeImgExt: '.png',
      gameURI,
      storeIdx,
      gameImage,
      gameType,
      gameName,
      imgWidth,
      imgHeight,
      deviceImage,
      btnPlayImage,
      prizeDescWidth,
      prizeImgMargin,
      btnPlayTop,
      showModal: false
    }
  },
  methods: {
    setGameImage () {
      //
      //  Set Image Size according to window width
      //
      if (window.innerWidth >= 600) {
        this.imgWidth = 260
        this.imgHeight = 260
        this.deviceImgWidth = 400
        this.deviceImgHeight = 415
        this.btnPlayImage = require('../assets/btn_play_long.png')
        this.deviceImage = require('../assets/device_bg_large.png')
        document.getElementById('table-wrapper').style.width = '400px'
        document.getElementById('bottom-info').style.width = '400px'
        document.getElementById('td-image').style.width = '120px'
        this.prizeImgMargin = '0px 0px 0px 40px'
        this.prizeDescWidth = 226
        this.btnPlayTop = '345px'
      } else if (window.innerWidth < 600 && window.innerWidth >= 330) {
        this.imgWidth = 208
        this.imgHeight = 208
        this.deviceImgWidth = 300
        this.deviceImgHeight = 357
        this.btnPlayImage = require('../assets/btn_play.png')
        this.deviceImage = require('../assets/device_bg_small.png')
        document.getElementById('table-wrapper').style.width = '317px'
        document.getElementById('bottom-info').style.width = '317px'
        document.getElementById('td-image').style.width = '110px'
        this.prizeImgMargin = '0px 0px 0px 30px'
        this.prizeDescWidth = 160
        this.btnPlayTop = '282px'
      } else {
        this.imgWidth = 208
        this.imgHeight = 208
        this.deviceImgWidth = 300
        this.deviceImgHeight = 357
        this.btnPlayImage = require('../assets/btn_play.png')
        this.deviceImage = require('../assets/device_bg_small.png')
        document.getElementById('table-wrapper').style.width = '300px'
        document.getElementById('bottom-info').style.width = '300px'
        document.getElementById('td-image').style.width = '110px'
        this.prizeImgMargin = '0px 0px 0px 30px'
        this.prizeDescWidth = 145
        this.btnPlayTop = '282px'
      }
      //
      //  Set Game Type
      //
      switch (this.gameType) {
        case 1:
          //  사다리게임
          this.gameImage = require('../assets/img_game_ladder.png')
          this.gameName = 'mgkt_ladder'
          break
        case 2:
          //  룰렛
          this.gameImage = require('../assets/img_game_roulette.png')
          this.gameName = 'mgkt_roulette'
          break
        case 3:
          //  복권
          this.gameImage = require('../assets/img_game_lottery.png')
          this.gameName = 'mgkt_speedo'
          break
        default:
          this.gameImage = require('../assets/img_game_ladder.png')
          this.gameName = 'mgkt_ladder'
          console.warn('Invalid Game Type = ' + this.gameType)
          break
      }
      //  Set Game full URI
      const baseGameURI = 'http://ktddoddostore.com'
      this.gameURI = `${baseGameURI}/${this.gameName}.html?id=${this.storeIdx}`
      //  console.log('parsed gameName = ' + this.gameName)
      //  console.log('parsed gameURI = ' + this.gameURI)
    },
    getStoreID () {
      console.log('Cur URL is ' + currentUrl)
      try {
        var tempArr = currentUrl.split('/')
        var tempID = tempArr[tempArr.length - 1]
        this.storeIdx = parseInt(tempID)
        //  this.storeIdx = 1 //  Test Index
        if (isNaN(this.storeIdx)) {
          console.warn('Invalid Store ID = ' + storeIdx + '\nChanged Store ID to 1 temporarily')
          this.storeIdx = 1
        }
      } catch (error) {
        console.log('parsing failed')
        this.storeIdx = -1
      }
    },
    requestAPI () {
      axios.get(`${baseURI}/${this.storeIdx}`)
        .then((result) => {
          //  Parse result object to Json
          resultJson = JSON.parse(JSON.stringify(result.data))
          //  set Data from Json
          this.setData()
          //  console.log('getting API result is completed')
          //  set Game Image depends Window Size
          this.setGameImage()
        })
    },
    showExpiredPopup (toDateArr) {
      console.log('toDate = ' + toDateArr)
      //  Get To Date
      const arrToDate = toDateArr.split('-')
      const toYear = arrToDate[0]
      const toMonth = parseInt((arrToDate[1])).toString()
      const toDay = arrToDate[2]
      console.log('arrResultToDate = ' + toYear)
      console.log('arrResultToDate = ' + toMonth)
      console.log('arrResultToDate = ' + toDay)
      //  Get Cur Date
      const curDate = new Date()
      const curYear = curDate.getFullYear()
      const curMonth = curDate.getMonth() + 1
      const curDay = curDate.getDate()
      console.log('Date - year = ' + curYear)
      console.log('Date - month = ' + curMonth)
      console.log('Date - day = ' + curDay)
      //  Check Year
      if (toYear > curYear) {
        this.showModal = false
        return
      } else if (toYear < curYear) {
        this.showModal = true
        return
      }
      //  Check Month
      if (toMonth > curMonth) {
        this.showModal = false
        return
      } else if (toMonth < curMonth) {
        this.showModal = true
        return
      }
      //  Check Day
      if (toDay < curDay) {
        this.showModal = true
      } else {
        this.showModal = false
      }
    },
    setData () {
      //  Set Game Title
      document.getElementById('titleEvent').innerHTML = resultJson['title']
      //  Set Event Period
      document.getElementById('eventPeriod-date').innerHTML = resultJson['fromDate'] + ' ~ ' + resultJson['toDate']
      //  Set Show Popup
      this.showExpiredPopup(resultJson['toDate'])
      //  Set prize array
      var prizeArr = resultJson['prize']
      for (let i = 0; i < items.length; i++) {
        this.items.pop()
      }
      for (let i = 0; i < prizeArr.length; i++) {
        this.items.splice(i, 1, prizeArr[i])
      }
      //  Game Type
      this.gameType = resultJson['gameType']
      console.log('parsed gameType = ' + this.gameType)
    }
  },
  created () {
    window.addEventListener('resize', this.setGameImage)
    //  Get Store ID from curUrl
    this.getStoreID()
    //  API data to Json
    this.requestAPI()
  },
  destroyed () {
    window.removeEventListener('resize', this.setGameImage)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#img-desc-top {
  margin-bottom: 30px;
}

.container-game {
  position: relative;
  margin: 0;
  padding: 0;
}

.device-img {
  position: relative;
  margin: 0px;
  top: 0%;
  transform: translate(0%, 1.0%);
}

.game-img {
  position: absolute;
  top: 16.8%;
  left: 50%;
  transform: translate(-50%, 0%);
  border-radius: 15px;
}

.button-form {
  position: absolute;
  top: 282px;
  left: 50%;
  transform: translate(-50%, 0%);
}

.container-titleEvent {
  padding: 30px 0px 0px 0px;
}

#titleEvent {
  width: 190px;
  height: 28px;
  flex-grow: 0;
  font-family: NanumSquare_acEB;
  font-size: 21px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.33;
  letter-spacing: -0.06px;
  text-align: center;
  color: #000;
}

.container-eventPeriod {
  width: auto;
  height: 24px;
  margin: 6px 0px 0px 0px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0%);
}

.bg-eventPeriod {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.inner-container-eventPeriod {
  width: 241px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.span-eventPeriod {
  display: table;
}

#eventPeriod-caption {
  width: auto;
  height: 14px;
  display: inline-table;
  float: none;
  flex-grow: 0;
  font-family: NanumSquare_acB;
  font-size: 13px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #444;
}

#eventPeriod-date {
  width: auto;
  height: 14px;
  display: inline-table;
  float: none;
  flex-grow: 0;
  font-family: NanumSquare_acB;
  font-size: 13px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #444;
}

ul.bottom-info {
  list-style-type: disc;
}

.container-bottom-info {
  min-width: 300px;
  margin: 10px 0px 0px 0px;
  padding: 0px 0px 100px 0px;
}

.bottom-info {
  font-family: NanumSquare_acR;
  float:right;
  text-align:left;
  position:relative;
  left:-50%;
  transform: translate(50%, 0%);
}

.bottom-info ul {
  list-style:none;
  position:relative;
  left:50%;
}

.bottom-info li {
  position:relative;
  font-size: 13px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.46;
  letter-spacing: normal;
  text-align:start;
  color: #969696;
}

.bottom-info li::marker {
  color: #969696;
  font-size: 2px;
}

ul {
  list-style-type: none;
  padding: 0;
}

#table-prize {
  table-layout: fixed;
  position: relative;
  width: 100%;
}

#tr-item {
  border-collapse: collapse;
  height: 99px;
}

#tr-div-line {
  width: 100%;
  height: 1px;
  background: #f5f5f5;
}

.td-li-image {
  float: left;
  padding: 0px 0px 0px 0px;
}

.td-li-title {
  text-align: start;
  width: 48px;
  height: 21px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  margin: 0px 0px 5px 15px;
  padding: 3px 10px 3px 10px;
  background-color: #000;
}

.td-span-title {
  font-family: NanumSquare_acEB;
  font-size: 12px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  letter-spacing: normal;
  text-align: left;
  color: #fff;
  position:relative;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.td-li-desc {
  font-family: NanumSquare_acB;
  font-size: 13px;
  text-align: start;
  margin: 0px 0px 0px 15px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow-wrap: break-all;
  overflow: hidden;
  text-overflow: ellipsis;
}

#table-wrapper {
  position:relative;
  background: white;
  overflow: overlay;
  margin: 50px 0px 0px 0px;
  left: 50%;
  transform: translate(-50%, 0%);
}

#table-scroll {
  height:300px;
  margin: 0px;
  padding: 0px 0px 0px 0px;
}

#table-wrapper::-webkit-scrollbar {
  width: 23px;
}

#table-wrapper::-webkit-scrollbar-track {
    background-color: transparent;
    box-shadow: inset 0 0 10px 10px transparent;
    border: solid 10px transparent;
}

#table-wrapper::-webkit-scrollbar-thumb {
    width: 3px;
    background-color: transparent;
    border-radius: 26px;
    box-shadow: inset 0 0 10px 10px #c4c4c4;
    border: solid 10px transparent;
}

#logo {
  margin: 30px;
  width: 25px;
  height: 20.4px;
  flex-grow: 0;
  margin-bottom: 30px;
}

div * {
  margin: 0;
  padding: 0;
}

body {
  margin: 0;
  padding: 0;

}

@font-face {
  font-family:'NanumSquare_acB';
  src: url('../assets/fonts/NanumSquare_acB.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquare_acEB';
  src: url('../assets/fonts/NanumSquare_acEB.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquare_acL';
  src: url('../assets/fonts/NanumSquare_acL.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquare_acR';
  src: url('../assets/fonts/NanumSquare_acR.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquareB';
  src: url('../assets/fonts/NanumSquareB.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquareEB';
  src: url('../assets/fonts/NanumSquareEB.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquareL';
  src: url('../assets/fonts/NanumSquareL.ttf') format('truetype');
}
@font-face {
  font-family:'NanumSquareR';
  src: url('../assets/fonts/NanumSquareR.ttf') format('truetype');
}
</style>

<!--
상단 타이틀 이미지 대신 코드로 구현할 떄 사용
.container-box {
  width: 250px;
  height: 111px;
  border: 0px solid transparent;
  margin: 30px 0px 30px -125px;
  position: relative;
  left: 50%;
}

.container-row {
  width: 250px;
  height: auto;
  border: 1px solid transparent;
  margin: 34px 0px 0px -125px;
  font-family: SandollGyeokdongG2-04Rg;
  font-size: 26px;
  font-stretch: normal;
  font-style: normal;
  line-height: 1.42;
  letter-spacing: normal;
  text-align: center;
  color: #000;
  position: relative;
  left: 50%;
}

.overlap {
  width: 50%;
  height: auto;
  margin: 0 0 25.1px;
  position: absolute;
  transform: translate(20%, 0%);
}

.over-img {
  width: 77.9px;
  height: auto;
  transform: translate(0%, 50%);
}

.over-text {
  width: 100px;
  height: auto;
  font-weight: bold;
  top: 50%;
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
}

.normal-text {
  width: auto;
  height: auto;
  font-weight: normal;
  position: absolute;
  text-align: start;
  left: 55%;
}
-->
