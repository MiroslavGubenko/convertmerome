<template>
  <div class="main-container">
    <nav>
      <div class="logo">
        <img class="icon" src="./assets/logo/roman-logo.svg" alt="logo" />
        <p class="site-name">rome <br />convertator</p>
      </div>
      <button class="btn btn-translate">
        <p
          v-for="t in translate.options"
          :key="t"
          :class="{ current: t == translate.current }"
        >
          {{ t }}
        </p>
      </button>
    </nav>
    <section class="view-result">
      <div class="result">
        <button class="btn btn-copy">
          <img src="./assets/i-copy.svg" alt="copy" />
        </button>
        <p class="result-text">{{ result }}</p>
      </div>
    </section>
    <section class="input-section">
      <input type="text" v-model="input" />
      <div class="input-btn">
        <button class="btn btn-help">?</button>
        <button
          @click="switchMode(mode.current)"
          class="btn btn-change-number-system"
        >
          {{ mode.current }} <span class="to">to</span> {{ mode.second }}
          <span class="icon">&#8646;</span>
        </button>
      </div>
    </section>
    <section class="history">
      <span class="discription">history</span>
      <p class="history-text" v-for="(ht, i) in history" :key="i">
        {{ i + 1 }}. {{ ht }}
      </p>
    </section>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      result: "",
      input: "",
      translate: {
        options: ["Rus", "Eng"],
        current: "",
      },
      history: [""],
      mode: {
        current: "arabic",
        second: "roman",
      },
    };
  },
  mounted() {
    this.translate.current = "Eng";
  },
  methods: {
    numeralsToRoman: function (number) {
      return "I"
        .repeat(number)

        .replace(/IIIII/g, "V")
        .replace(/IIII/, "IV")
        .replace(/VV/g, "X")
        .replace(/VIV/, "IX")

        .replace(/XXXXX/g, "L")
        .replace(/XXXX/, "XL")
        .replace(/LL/g, "C")
        .replace(/LXL/, "XC")

        .replace(/CCCCC/g, "D")
        .replace(/CCCC/, "CD")
        .replace(/DD/g, "M")
        .replace(/DCD/, "CM");
    },
    romanToArabic: function (roman) {
      roman = roman.toUpperCase();
      if (roman == null || roman.charAt(0) == 0) return 0;
      let totalValue = 0,
        value = 0,
        prev = 0,
        map_rome_symbols = {
          I: 1,
          V: 5,
          X: 10,
          L: 50,
          C: 100,
          D: 500,
          M: 1000,
        };

      for (let i = 0; i < roman.length; i++) {
        let current = map_rome_symbols[roman.charAt(i)];
        if (current > prev) {
          // Undo the addition that was done, turn it into subtraction
          totalValue -= 2 * value;
        }
        if (current !== prev) {
          // Different symbol?
          value = 0; // reset the sum for the new symbol
        }
        value += current; // keep adding same symbols
        totalValue += current;
        prev = current;
      }
      return totalValue;
    },
    switchMode: function (currentMode) {
      if (currentMode == "arabic") {
        this.mode.current = "roman";
        this.mode.second = "arabic";
      } else {
        this.mode.current = "arabic";
        this.mode.second = "roman";
      }
    },
  },
  watch: {
    input: function () {
      switch (this.mode.current) {
        case "arabic":
          this.result = this.numeralsToRoman(this.input);
          break;
        case "roman":
          this.result = this.romanToArabic(this.input);
          break;
      }
    },
  },
};
</script>

<style lang="scss">
@import "./assets/style/main";
</style>
