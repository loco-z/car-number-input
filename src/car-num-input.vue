<template>
  <div>
    <div class="z-input-body">
      <div
        class="z-plat-num"
        :class="currentIndex == index ? 'z-plat-active' : ''"
        v-for="(value, index) in inputPlates"
        :key="index"
        @click="inputPlat(index)"
      >
        <span class="z-plat-text">{{ value }}</span>
      </div>
      <div
        class="z-plat-num"
        v-if="!newEnergy"
        @click="
          newEnergy = true;
          currentIndex = 7;
          showKeyboard = true;
        "
        style="background: #13ad7e; color: #fff; font-size: 11px"
      >
        新能源
      </div>
      <div
        class="z-plat-num"
        :class="currentIndex == 7 ? 'z-plat-active' : ''"
        v-else
        @click="inputPlat('index7')"
      >
        <span class="z-plat-text">{{ index7 }}</span>
      </div>
    </div>
    <transition name="fade">
      <div class="z-keyboard" v-show="showKeyboard">
        <div class="keyboard-title">
          <span @click="showKeyboard = false">关闭</span>
        </div>
        <div class="keyboard-body">
          <div class="keyboard-line" v-show="currentIndex == 0">
            <div
              class="key"
              v-for="(v, i) in keyboard1.slice(0, 10)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>

          <div
            class="keyboard-line"
            style="width: 90%"
            v-show="currentIndex == 0"
          >
            <div
              class="key"
              v-for="(v, i) in keyboard1.slice(10, 19)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>

          <div
            class="keyboard-line"
            style="width: 70%"
            v-show="currentIndex == 0"
          >
            <div
              class="key"
              v-for="(v, i) in keyboard1.slice(19, 26)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>
          <div
            class="keyboard-line"
            style="width: 50%"
            v-show="currentIndex == 0"
          >
            <div
              class="key"
              v-for="(v, i) in keyboard1.slice(26)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>

          <div class="keyboard-line" v-show="currentIndex !== 0">
            <div
              class="key"
              v-for="(v, i) in keyboardNumber.slice(0, 10)"
              :key="v + i"
              :class="
                currentIndex == 1 && disableKey.indexOf(v) > -1 ? 'key-ban' : ''
              "
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>
          <div class="keyboard-line" v-show="currentIndex !== 0">
            <div
              class="key"
              v-for="(v, i) in keyboardNumber.slice(10, 20)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>
          <div class="keyboard-line" v-show="currentIndex !== 0">
            <div
              class="key"
              v-for="(v, i) in keyboardNumber.slice(20, 30)"
              :key="v + i"
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>
          <div
            class="keyboard-line"
            v-show="currentIndex !== 0"
            style="width: 80%; margin: 0"
          >
            <div
              class="key"
              v-for="(v, i) in keyboardNumber.slice(30, 40)"
              :key="v + i"
              :class="
                currentIndex == 1 && disableKey.indexOf(v) > -1 ? 'key-ban' : ''
              "
              @click="setValue(v)"
            >
              {{ v }}
            </div>
          </div>
          <div class="key-delete" @click="deleteValue">x</div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "z-car-num-input",
  components: {},
  data() {
    return {
      showKeyboard: false,
      isKeyboard: true,
      isNumberKB: true,
      tapNum: true,
      disableKey: "1234567890学港澳警",
      keyboardNumber: "1234567890ABCDEFGHJKLMNPQRSTUVWXYZ学港澳警",
      keyboard1:
        "京沪粤津冀晋蒙辽吉黑苏浙皖闽赣鲁豫鄂湘桂琼渝川贵云藏陕甘青宁新",
      brandIndex: "",
      modelIndex: "",
      carNoColorIndex: 0,
      inputPlates: ["", "", "", "", "", "", ""],
      index7: "",
      currentNumIndex: "",
      flag: false,
      newEnergy: false,
      currentIndex: 0,
    };
  },
  methods: {
    inputPlat(index) {
      if (index == "index7") {
        this.currentIndex = 7;
      } else {
        this.currentIndex = index;
      }

      this.showKeyboard = true;
    },
    setValue(v) {
      let value = v;
      if (this.currentInput == "1" && this.disableKey.includes(v)) {
        return;
      } else {
        if (this.currentIndex <= 6) {
          this.$set(this.inputPlates, this.currentIndex, value);
          if (this.currentIndex == 6 && !this.newEnergy) {
            return;
          }
          this.currentIndex += 1;
        } else if (this.currentIndex == 7 && this.newEnergy == true) {
          this.index7 = value;
        }

        //  console.log(value);
      }
    },
    deleteValue() {
      let index = this.currentIndex;
      if (index == 7) {
        this.index7 = "";
      } else {
        this.$set(this.inputPlates, this.currentIndex, "");
      }
      this.currentIndex -= 1;
    },
  },
  computed: {
    carNumber() {
      return this.inputPlates.join("") + this.index7;
    },
  },
  watch: {
    carNumber(newValue) {
      this.$emit("carNum", newValue);
    },
  },
};
</script>
<style scoped lang="scss">
.z-input-body {
  display: flex;
  align-items: center;
  height: 67px;
  color: #555;
  font-size: 10px;
  margin-top: 12px;
  padding: 0 11px;
  justify-content: center;
  .z-plat-num {
    width: 29px;
    height: 40px;
    border: 1px solid #ccc;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    margin: 0 5px;
    box-sizing: border-box;
    border-radius: 5px;
    .z-plat-text {
      flex: 1;
      height: 50px;
      font-size: 17px;
      line-height: 50px;
      box-sizing: border-box;
      border-radius: 2px;
      font-weight: normal;
    }
  }
  .z-plat-active {
    border: 1px solid #13ad7e;
  }
}
.z-keyboard {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  .keyboard-title {
    text-align: right;
    padding: 10px;
    color: #13ad7e;
    // box-shadow: #333 0 -1px 0;
    border-top: 1px solid #aaa;
  }
  .keyboard-body {
    background: #d7d8dc;
    position: relative;
    .keyboard-line {
      width: 99%;
      display: flex;
      text-align: center;
      margin: 0px auto;
      .key {
        font-family: "微软雅黑";
        flex-grow: 1;
        text-align: center;
        font-size: 17px;
        height: 43px;
        line-height: 40px;
        background: #fff;
        margin: 5px 2.5px;
        color: #333;
        border-radius: 5px;
        box-shadow: 0px 1px 0px #a9a9a9;
        flex: 1 1 8%;
        &:active {
          background: #13ad7e;
        }
      }
      .key-ban {
        box-shadow: 0 0 0;
        background: #e5e5e5;
      }
    }
    .key-delete {
      position: absolute;
      bottom: 5px;
      right: 5px;
      width: 69px;
      height: 43px;
      background-color: #fff;
      box-shadow: 0px 1px 0px #a9a9a9;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 5px;
      &:active {
        background: #13ad7e;
      }
    }
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translateY(50%);
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
  transform: translateY(0%);
}
</style>
