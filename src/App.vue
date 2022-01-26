<template>
  <div id="app" class="py-5 text-center container">
    <h2 class="mb-5">아브 운석 계산기</h2>
    <div class="input-box">
      <input class="form-control" v-model="time" maxlength="4" placeholder="노란메테오 시간을 4자리로 입력해주세요."
             @keypress="onlyNumber" @keyup.enter="calculateTime">
      <div class="small mt-1">엔터를 누르거나 밑의 버튼을 누르면 계산이 됩니다.</div>
      <button type="button" class="btn btn-light mt-2" @click="calculateTime">계산하기</button>
    </div>
    <div class="mt-5" v-if="makeSec && chammeMakeSec">
      <div class="box">
        장판 생성 시간<br>
        <small>(첫번째, 세번째)</small>
        <div class="fs-1"><strong>{{ makeSec }}</strong></div>
      </div>
      <div class="box">
        2번째 장판 생성 시간<br>
        <small>(찬미)</small>
        <div class="fs-1"><strong>{{ chammeMakeSec }}</strong></div>
      </div>
    </div>
  </div>
</template>

<script lang='ts'>
import { Component, Vue } from 'vue-property-decorator';
import luHeader from '@/layout/header.component.vue';

const MINUS_SECOND = 100;
const CHANME_MINUS_SECOND = 70;

@Component
export default class App extends Vue {
  time: string = '';
  makeSec: string = '';
  chammeMakeSec: string = '';

  onlyNumber($event: KeyboardEvent) {
    if ($event.keyCode < 48 || $event.keyCode > 57) {
      $event.returnValue = false;
    }
  }

  calculateTime() {
    if (this.time.length === 4) {
      const convertSec = (Number(this.time.substr(0, 2)) * 60) + Number(this.time.substr(2, 2));
      const makeSec = convertSec - MINUS_SECOND;
      const chanmeMakeSec = convertSec - CHANME_MINUS_SECOND;

      this.makeSec = this._format(makeSec);
      this.chammeMakeSec = this._format(chanmeMakeSec);
    }
  }

  private _format(second: number) {
    const min = Math.floor(second / 60);
    const sec = second - (min * 60);

    return `${min}:${sec}`;
  }
}
</script>

<style lang="scss">
body {
  background-color: #181818;
  color: rgb(237, 237, 237);
}

.input-box {
  width: 480px;
  margin: auto;
}

.box {
  border: 1px solid #dddddd;
  padding: 20px;
  border-radius: 10px;
  width: 280px;
  display: inline-block;

  &:first-child {
    margin-right: 15px;
  }
}
</style>
