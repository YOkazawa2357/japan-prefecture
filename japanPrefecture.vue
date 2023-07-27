<template>
  <v-container>
    <v-row class="pa-6">
      <v-col>
        <v-row 
          class="btn-double justify-center" 
          :key="row"
        >
          <v-col 
            cols="auto" 
            v-for="n in 8" 
            :key="n"
          >
            <v-btn 
              v-if="n >= 8" 
              class="square-btn" 
              block 
              v-bind:class="getRegionClass(-1, n)" 
              @click="handleClick(-1, n)"
              >
              <div class="btn-text">
                {{ getPrefectureName(-1, n) }}
              </div>
            </v-btn>
          </v-col>
        </v-row>
        <v-row 
          class="btn-row justify-center" 
          v-for="row in 9" 
          :key="row"
        >
          <v-col 
            cols="auto" 
            v-for="n in 16" 
            :key="`col-${row}-${n}`"
          >
            <v-btn
              v-if="shouldDisplayButton(row-1, n)" 
              class="square-btn" 
              block 
              v-bind:class="getRegionClass(row-1, n)" 
              @click="handleClick(row-1, n)"
              >
              <div class="btn-text">
                {{ getPrefectureName(row-1, n) }}
              </div>
            </v-btn>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>   
</template>

<script setup>
  import { ref, defineEmits } from 'vue';

  // define your emit event
  const emit = defineEmits(['select-prefecture'])

  const handleClick = (row, n) => {
    const prefectureName = getPrefectureName(row, n);
    // Emit the selected prefecture name
    emit('select-prefecture', prefectureName);
  };

  const prefectures = [
    "北海道", "青森", "秋田", "岩手", "山形", "宮城", "石川", "富山", "新潟", 
    "栃木", "福島", "島根", "鳥取", "兵庫", "京都", "滋賀", "福井", "岐阜", 
    "長野", "群馬", "埼玉", "茨城", "長崎", "佐賀", "福岡", "山口", "広島", 
    "岡山", "大阪", "奈良", "三重", "愛知", "静岡", "山梨", "神奈川", "東京", 
    "千葉", "熊本", "大分", "愛媛", "香川", "和歌山", "鹿児島", "宮崎", "高知", 
    "徳島", "沖縄"
  ];

  const prefectureMap = new Map([
    ["-1,8", 0], 
    ["0,15", 1], 
    ["1,14", 2], ["1,15", 3], 
    ["2,14", 4], ["2,15", 5], 
    ["3,11", 6], ["3,12", 7], ["3,13", 8], ["3,14", 9], ["3,15", 10], 
    ["4,5", 11], ["4,6", 12], ["4,7", 13], ["4,8", 14], ["4,9", 15], ["4,10", 16], ["4,11", 17], ["4,12", 18], ["4,13", 19], ["4,14", 20], ["4,15", 21],
    ["5,1", 22], ["5,2", 23], ["5,3", 24], ["5,4", 25], ["5,5", 26], ["5,6", 27], ["5,7", 28], ["5,8", 29], ["5,9", 30], ["5,10", 31], ["5,11", 32], ["5,12", 33], ["5,13", 34], ["5,14", 35], ["5,15", 36],
    ["6,2", 37], ["6,3", 38], ["6,5", 39], ["6,6", 40], ["6,8", 41],
    ["7,2", 42], ["7,3", 43], ["7,5", 44], ["7,6", 45],
    ["8,1", 46]
  ]);

  const getPrefectureName = (row, n) => {
    const index = prefectureMap.get(`${row},${n}`);
    return index !== undefined ? prefectures[index] : undefined;
  };

  const getRegionClass = (row, n) => {
    const prefectureIndex = prefectureMap.get(`${row},${n}`);
    if (prefectureIndex === undefined) return null;

    switch(prefectureIndex) {
      case 0: return 'hokkaido'; // 北海道
      case 1: case 2: case 3: case 4: case 5:  case 10: return 'tohoku'; // 東北
      case 9: case 19: case 20: case 21: case 34: case 35: case 36: return 'kanto'; // 関東
      case 6: case 7: case 8: case 16: case 17: case 18: case 31: case 32: case 33: return 'chubu'; // 中部
      case 13: case 14: case 15: case 28: case 29: case 30: case 41: return 'kinki'; // 近畿
      case 11: case 12: case 25: case 26: case 27: return 'chugoku'; // 中国
      case 39: case 40: case 44: case 45: return 'shikoku'; // 四国
      case 22: case 23: case 24: case 37: case 38: case 42: case 43: case 46: return 'kyushu'; // 九州
    }
  };

  const shouldDisplayButton = (row, n) => {
    // ボタンを非表示にする条件をここに記述します
    if (row === 0 && n !== 15) {
      return false;
    }

    if (row === 1 && (n < 14 || n > 15)) {
      return false;
    }

    if (row === 2 && (n < 14 || n > 15)) {
      return false;
    }

    if (row === 3 && (n < 11 || n > 15)) {
      return false;
    }

    if (row === 4 && (n < 5 || n > 15)) {
      return false;
    }

    if (row === 5 &&  n > 15) {
      return false;
    }

    if (row === 6 && ![2, 3, 5, 6, 8].includes(n)) {
      return false;
    }

    if (row === 7 && ![2, 3, 5, 6].includes(n)) {
      return false;
    }

    if (row === 8 && n !== 1) {
      return false;
    }
    return true;
  };

</script>

<style scoped>
.btn-row > .v-col,
.btn-double > .v-col {
  flex: 1 0 auto;
  max-width: 5.5%; 
  padding: 1px; 
}

.btn-double > .v-col {
  flex: 1 0 auto;
  max-width: 11%; 
  padding: 1px; 
}

.square-btn {
  width: 100%;
  padding-bottom: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-text {
  width: 100%;
  text-align: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: rgb(255, 255, 255);
  font-weight: bold;
}

.hokkaido { background-color: #299ffa; } /* 北海道 */
.tohoku { background-color: #65a6e8; } /* 東北 */
.kanto { background-color: #00baba; } /* 関東 */
.chubu { background-color: #7ece72; } /* 中部 */
.kinki { background-color: #cb97dc; } /* 近畿 */
.chugoku { background-color: #f07faa; } /* 中国 */
.shikoku { background-color: #f3b052; } /* 四国 */
.kyushu { background-color: #FC6E6F; } /* 九州 */

</style>