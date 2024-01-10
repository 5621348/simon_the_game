<template>
  <div>
    <table>
      <tbody>
        <tr>
          <td class="treetr" rowspan="3">
            <template>
              <!--кнопочка старт, для звука-->
              <div v-if="workMode === 'startGame'" >
                <input
                    v-on:click="restartGame()"
                    type="button"
                    value="Start"
                >
              </div>
              <template v-if="( workMode !== 'gameOver' )">
                Уровень сложности
                <div v-for="dif in Difficulty" :key="dif.id" >
                  <input
                      v-bind:id="dif.id"
                      v-bind:checked="(dif.updateTime === currentUpdateTime)"
                      v-bind:disabled="(workMode === 'example' || workMode === 'wait')"
                      v-on:change="difficultySelection(dif.id)"
                      name='difficulty'
                      type='radio'
                  >
                  <label v-bind:for="dif.id" > {{ dif.text }} </label>
                </div>
              </template>
              <template v-else>
                <!--кнопочка рестарт-->
                <input
                    v-on:click="restartGame()"
                    type="button"
                    class="buttonGameOver"
                    value="click to restart"
                >
              </template>
              <div>
                <!--статистика-->
                Уровень: {{ gameCounter.currentLength-1 }} Рекорд: {{ compMemLocalStorage }}
              </div>
            </template>
          </td>
          <td class="twootd" colspan="2">{{ workMode }}</td>
        </tr>
        <tr v-for="tr in arrForTable" :key="tr" >
          <td v-for="td in arrForTable" :key="td" >
<!--            {{ tr*2 + td }} {{ tr }} {{ td }}-->
            <template v-if="Bottoms[tr*2+td].flag" >
<!--              {{ Bottoms[tr*2+td].id }}-->
              <input
                  type="image"
                  v-bind:class="Bottoms[tr*2+td].class"
                  v-bind:src=" Bottoms[tr*2+td].img_1 "
                  v-on:click="workMode === 'play' ? checkInPlay(Bottoms[tr*2+td]) : ''"
              >
              <!--img
                  v-bind:class="Bottoms[tr*2+td].class"
                  v-bind:src=" Bottoms[tr*2+td].img_1 "
                  v-on:click="workMode === 'play' ? checkInPlay(Bottoms[tr*2+td]) : ''"
              /-->
            </template>
            <template v-else>
              <img v-bind:class="Bottoms[tr*2+td].class" v-bind:src="Bottoms[tr*2+td].img_2" />
            </template>
          </td>
        </tr>
        <!--<tr>
          <td>1&nbsp;</td>
          <td>2&nbsp;</td>
        </tr>
        <tr>
          <td>3&nbsp;</td>
          <td>4&nbsp;</td>
        </tr>-->
      </tbody>
    </table>

<!--    {{ gameCounter.arr }} <br>-->
    <!--{{ workMode }} <br>
    <div>
      Уровень: {{ gameCounter.currentLength-1 }} Рекорд: {{ compMemLocalStorage }}
    </div>

    <div>
      <template v-if="( workMode !== 'gameOver' )">
        Уровень сложности
        <div v-for="dif in Difficulty" :key="dif.id" >
          <input
              v-bind:id="dif.id"
              v-bind:checked="(dif.updateTime === currentUpdateTime)"
              v-on:change="difficultySelection(dif.id)"
              name='difficulty'
              type='radio'
          >
          <label v-bind:for="dif.id" > {{ dif.text }} </label>
        </div>
      </template>
      <template v-else>
        <input
            v-on:click="restartGame()"
            type="button"
            class="buttonGameOver"
            value="click to restart"
        >
      </template>
    </div>

    <div class="sraka" v-for="bott in Bottoms" :key='bott.id' >
      <template v-if="bott.flag" >
        {{ bott.id }}
        <img
            v-bind:class="bott.class"
            v-bind:src=" bott.img_1 "
            v-on:click="workMode === 'play' ? checkInPlay(bott) : ''"
        />
      </template>
      <template v-else>
        <img v-bind:class="bott.class" v-bind:src="bott.img_2" />
      </template>
    </div>-->

  </div>
</template>

<script>

let idBottom = 0;
let idComplexity = 0;
let imgUrls = [
  require("@/assets/simon_the_game_assets/00.png"),
  require("@/assets/simon_the_game_assets/01.png"),
  require("@/assets/simon_the_game_assets/10.png"),
  require("@/assets/simon_the_game_assets/11.png"),
  require("@/assets/simon_the_game_assets/20.png"),
  require("@/assets/simon_the_game_assets/21.png"),
  require("@/assets/simon_the_game_assets/30.png"),
  require("@/assets/simon_the_game_assets/31.png")
];
let imgSound = [
  require("@/assets/simon_the_game_assets/msft_entermode.mp3"),
  require("@/assets/simon_the_game_assets/msft_mobilitysense.mp3"),
  require("@/assets/simon_the_game_assets/msft_safetysense.mp3"),
  require("@/assets/simon_the_game_assets/msft_tellmemore.mp3")
];

export default {
  name: "SimonTheGame",
  data() {
    return {
      currentUpdateTime: 1500,  //можно убрать с заменой на currentDifficulty.updateTime
      currentDifficulty: {},
      workMode: 'startGame',  //example; play; gameOver; startGame; wait;
      swapTimer: null,
      gameCounter: { arr:[], i: 0, currentLength: 1 },
      Difficulty: [
        { id: idComplexity++, text: 'Лёгкий', updateTime: 1500, mode: 'easy' },
        { id: idComplexity++, text: 'Средний', updateTime: 1000, mode: 'medium' },
        { id: idComplexity++, text: 'Сложный', updateTime: 400, mode: 'hard' }
      ],
      Bottoms: [
        { id: idBottom++, class: 'img-1', img_1: imgUrls[0], img_2: imgUrls[1], sound: new Audio(imgSound[0]), flag: true },
        { id: idBottom++, class: 'img-2', img_1: imgUrls[2], img_2: imgUrls[3], sound: new Audio(imgSound[1]), flag: true },
        { id: idBottom++, class: 'img-3', img_1: imgUrls[4], img_2: imgUrls[5], sound: new Audio(imgSound[2]), flag: true },
        { id: idBottom++, class: 'img-4', img_1: imgUrls[6], img_2: imgUrls[7], sound: new Audio(imgSound[3]), flag: true }
      ],
      arrForTable: [0, 1],
      standardValueForMemory: {
        'easy': 0,
        'medium': 0,
        'hard': 0
      },
      styleWorkMode: {
        'startGame': '#FFEBCD',
        'example': '#4682B4',
        'wait': '#FFFF00',
        'play': '#00FF7F',
        'gameOver': '#B22222'
      }
    }
  },
  computed: {
    compMemLocalStorage() {
      let buf = JSON.stringify( this.standardValueForMemory );
      if ( localStorage.getItem('STGmaxWinValue') === null ){
        localStorage.setItem('STGmaxWinValue', buf );
        console.log( 'setItem STGmaxWinValue' );
      }
      console.log(window.localStorage);
      buf = JSON.parse( localStorage.getItem('STGmaxWinValue') );
      if ( this.gameCounter.currentLength -2 >= buf[ this.currentDifficulty.mode ] ) {
        ++buf[ this.currentDifficulty.mode ];
        localStorage.setItem('STGmaxWinValue', JSON.stringify( buf ));
        console.log(window.localStorage);
      }
      //localStorage.removeItem('STGmaxWinValue');``
      return buf[ this.currentDifficulty.mode ];
    }
  },
  methods: {
    swapper(img) {
      if (img.flag) {
        window.setTimeout( () => this.swapper(img), this.currentUpdateTime/2 );
        img.sound.pause();
        img.sound.currentTime = 0.0;
        img.sound.play();
      }
      img.flag = !img.flag;
    },
    difficultySelection(id) {
      this.currentUpdateTime = this.Difficulty[id].updateTime;
      this.currentDifficulty = this.Difficulty[id];
      this.restartGame();
      // this.workMode = 'example';
      // this.recImg(0);
      // console.log( this.currentUpdateTime );
    },
    creatingRandomArray() {
      this.gameCounter.arr = Array.from( {length: 10}, () => Math.floor(Math.random() * 4) );
      console.log('arr loaded');
    },
    recImg( id ) {
      if ( this.workMode === 'example' ) {
        let incid = id + 1;
        //console.log('id= '+id+'; incid= '+incid);
        //console.log('WorkMode: example == ' + this.workMode);
        this.swapper( this.Bottoms[ this.gameCounter.arr[id] ] );
        window.setTimeout( () => this.recImg(incid), this.currentUpdateTime * 2 );
        if ( incid >= this.gameCounter.currentLength ) {
          //console.log('swapWorkMode');
          window.setTimeout( () => this.workMode = 'play', this.currentUpdateTime );
        }
      } else {
        console.log('WorkMode: play == ' + this.workMode);
      }
    },
    checkInPlay( img ) {
      let i = this.gameCounter.i;
      if ( img.id === this.gameCounter.arr[ i ] ) {
        console.log('img correct');
        this.swapper( img );
        if ( i < this.gameCounter.currentLength - 1 ) {
          this.gameCounter.i = i + 1;
        } else {
          this.gameCounter.currentLength++;
          this.gameCounter.i = 0;
          this.workMode = 'wait';
          window.setTimeout(() => {
            this.workMode = 'example';
            this.recImg(0);
          }, this.currentUpdateTime * 2);

          // console.log( JSON.parse(localStorage.getItem('STGmaxWinValue')).easy );
          // console.log( JSON.parse(localStorage.getItem('STGmaxWinValue'))['easy'] );
          // let diff = JSON.parse(localStorage.getItem('STGmaxWinValue'));
          // if ( this.gameCounter.currentLength -2 >= diff[ this.currentDifficulty.mode ] ) {
          //   ++diff[ this.currentDifficulty.mode ];
          //   localStorage.setItem('STGmaxWinValue', JSON.stringify( diff ));
          //   console.log(window.localStorage);
          // }

          if ( this.gameCounter.currentLength >= this.gameCounter.arr.length ) {
            this.gameCounter.arr.push( Math.floor(Math.random() * 4) );
          }
        }

      } else {
        console.log('GAME OVER');
        this.workMode = 'gameOver';
      }
    },
    restartGame() {
      this.workMode = 'example';
      this.creatingRandomArray();
      this.gameCounter.i = 0;
      this.gameCounter.currentLength = 1;

      window.setTimeout( () => this.recImg(0), this.currentUpdateTime );
    },
    memLocalStorage() {
      let buf = JSON.stringify( this.standardValueForMemory );
      if ( localStorage.getItem('STGmaxWinValue') === null ){
        localStorage.setItem('STGmaxWinValue', buf );
        console.log( 'setItem STGmaxWinValue' );
      }
      console.log(window.localStorage);
      buf = JSON.parse( localStorage.getItem('STGmaxWinValue') );
      //console.log( buf );
      //localStorage.removeItem('STGmaxWinValue');
      // switch( this.currentDifficulty.text ) {
      //   case 'Лёгкий':
      //     return buf.easy;
      //   case 'Средний':
      //     return buf.medium;
      //   case 'Сложный':
      //     return buf.hard;
      // }
      return buf[ this.currentDifficulty.mode ];
    },
    // async iteratorImg() {
    //   if ( this.workMode === 'example' ) {
    //     for ( let i = 0; i < this.gameCounter.currentLength; i++ ){
    //       //this.swapper( this.Bottoms[ this.gameCounter.arr[i] ] );
    //       console.log('вызов testPromis; ' + 'i: ' + i);
    //       await this.testPromis( this.Bottoms[ this.gameCounter.arr[i] ] ).then( (mes) => {
    //         console.log('ok' + mes);
    //         window.setTimeout((resolve) => console.log(resolve), this.currentUpdateTime)
    //       } );
    //       console.log('дождались testPromis; ' + 'i: ' + i + '; ждём задержки по сложности');
    //       //await new Promise(resolve => window.setTimeout(() => resolve(true), this.currentUpdateTime) ).then( (mes) => console.log( 'ok' + mes ) );
    //     }
    //   }
    //   //window.setTimeout(this.iteratorImg(), 10000);
    // },
    // testPromis(img){
    //   return new Promise((resolve) => {
    //     window.setTimeout(() => {
    //       this.swapper(img);
    //       console.log('вызов swapper');
    //       resolve(true);
    //     }, this.currentUpdateTime);
    //   })
    // }
  },
  beforeMount() {  //hook, что выполняется перед монтированием
    //this.creatingRandomArray();
    this.currentDifficulty = this.Difficulty[0];
    //this.iteratorImg();
    //this.recImg( this.gameCounter.arr[0] );
  },
  mounted() {
    // this.recImg( 0 );
    // window.setTimeout( () => this.recImg(0), this.currentUpdateTime*2 );
  },
  watch: {
    workMode() {
      //this.sraka();
    }
    /*Bottoms: {
      flag: function () {
        console.log('ok')
        //this.work(img)
      },
      deep: true
    }*/
  }
}
</script>

<style scoped>

table {
  background-color: black;
  /*width: 100%;*/
  margin-left: auto;
  margin-right: auto;
  align-content: center;
  text-align: center;
}
td, th {
  border: 2px solid black;
  /*width: 32%;*/
}
.treetr {
  width: 180px;
  background-color: #FFEBCD;
}
.twootd {
  background-color: v-bind( styleWorkMode[ workMode ] );
}

img, .img-1, .img-2, .img-3, .img-4 {
  height: 140px;
}

</style>