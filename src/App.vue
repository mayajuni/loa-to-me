<template>
  <div id="app" class="py-5 text-center container">
    <h2 class="mb-5">아브 운석 계산기</h2>
    <div v-if="isPlaying === false">
      <div>
        <button type="button" class="btn btn-light" @click="start">입장시 클릭</button>
      </div>
    </div>
    <div v-else>
      <h3>아브 진행중</h3>
      <button type="button" class="mt-1 btn btn-sm btn-light " @click="restart">재입장</button>
      <div class="mt-4">
        <div class="box">
          <div class="mb-4" v-if="first">
            <div>첫번째 장판 생성 시간</div>
            <h1 class="mt-3">{{ first }}</h1>
          </div>
          <h4 class="mb-4" v-else>첫번쨰</h4>
          <button type="button" class="btn btn-light" @click="yellowMeteor1st">노란메테오</button>
          <button type="button" class="btn btn-light" @click="blueMeteor1st">파란메테오</button>
        </div>
      </div>
      <div class="mt-4">
        <div class="box">
          <div class="mb-4" v-if="second">
            <div>두번째 장판 생성 시간</div>
            <h1 class="mt-3">{{ second }}</h1>
          </div>
          <h4 class="mb-4" v-else>두번쨰</h4>
          <button type="button" class="btn btn-light" @click="yellowMeteor2st">두번쨰 노란메테오</button>
          <button type="button" class="btn btn-light" @click="blueMeteor2st">두번째 파란메테오</button>
        </div>
      </div>
      <div class="mt-4">
        <div class="box">
          <div class="mb-4" v-if="third">
            <div>세번째 장판 생성 시간</div>
            <h1 class="mt-3">{{ third }}</h1>
          </div>
          <h4 class="mb-4" v-else>세번쨰</h4>
          <button type="button" class="btn btn-light" @click="yellowMeteor3st">세번쨰 노란메테오</button>
          <button type="button" class="btn btn-light" @click="blueMeteor3st">세번쨰 파란메테오</button>
        </div>
      </div>
    </div>
    <div class="mt-5">
      <div class="box">
        메테오 계산법
        <div class="mt-3">
          노란 메테오: 클릭 시점 시간 - 100초(1분 40초)<br>
          파란 메테오: 클릭 시점 시간 - 40<br><br>
          기존 파란색 메테오는 노란색 메테오 이후 60초 이후에 생성됨<br>
          '두번째' 메테오는 위의 시간에 + 30초를 함. 찬미 영상을 30초로 계산
        </div>
      </div>
    </div>
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator';
import luHeader from '@/layout/header.component.vue';

@Component
export default class App extends Vue {
  isPlaying: boolean = false;
  startDt: number = 0;
  first: string = '';
  second: string = '';
  third: string = '';
  makeTime: number = 100;
  nextBlueTime: number = 40;
  chanMe: number = 30;

  start() {
    this.isPlaying = true;
    this.startDt = Date.now();
  }

  restart() {
    this.isPlaying = true;
    this.startDt = Date.now();
    this.first = '';
    this.second = '';
    this.third = '';
  }

  yellowMeteor1st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + this.makeTime;
    this.first = this.getDate(1200 - now);
  }

  blueMeteor1st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + this.nextBlueTime;
    this.first = this.getDate(1200 - now);
  }

  yellowMeteor2st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + (this.makeTime - this.chanMe);
    this.second = this.getDate(1200 - now);
  }

  blueMeteor2st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + (this.nextBlueTime - this.chanMe);
    this.second = this.getDate(1200 - now);
  }

  yellowMeteor3st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + (this.makeTime - this.chanMe);
    this.third = this.getDate(1200 - now);
  }

  blueMeteor3st() {
    const now = Math.ceil((Date.now() - this.startDt) / 1000) + (this.nextBlueTime - this.chanMe);
    this.third = this.getDate(1200 - now);
  }

  getDate(duration: number) {
    const min = Math.floor((duration % 3600) / 60);
    const sec = Math.floor(duration % 3600 % 60);

    return `${min ? `${min < 10 ? '0' + min : min}:` : '00:'}` +
        `${sec ? `${sec < 10 ? '0' + sec : sec}` : '00'}`;
  }
}
</script>

<style lang="scss">
body {
  background-color: #181818;
  color: rgb(237, 237, 237);
}

.box {
  border: 1px solid #dddddd;
  padding: 20px;
  border-radius: 10px;
  min-width: 320px;
  display: inline-block;
}
</style>
