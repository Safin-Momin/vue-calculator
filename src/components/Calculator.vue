<template>
  <body>
    <div
      class="min-h-[300px] rounded-lg bg-slate-400/25 backdrop-blur-[6px] border-2 border-slate-400/20"
    >
      <div class="w-full h-36 flex justify-center items-center">
        <input
          type="text"
          id="display"
          placeholder="0"
          v-model="displayValue"
          readonly
        />
      </div>
      <div class="w-full h-full grid grid-cols-4 p-5 gap-5">
        <span
          v-for="value in spanValues"
          :key="value"
          @click="handleSpanClick(value)"
          :class="{
            btn: isOperator(value),
            redBtn: isNumber(value),
            equal: isEqual(value),
            plus: isPlus(value),
            cVal: isCVal(value),
          }"
          >{{ value }}</span
        >
      </div>
    </div>
  </body>
</template>

<script>
import { ref } from "vue";

export default {
  name: "Calculator",
  setup() {
    const displayValue = ref("");
    let currentValue = "";
    let isCalculationDone = ref(false);

    const spanValues = [
      "7",
      "8",
      "9",
      "/",
      "4",
      "5",
      "6",
      "*",
      "1",
      "2",
      "3",
      "-",
      "0",
      ".",
      "=",
      "+",
      "C",
      "D",
    ];

    const appendValue = (value) => {
      if (isCalculationDone && isNaN(value)) {
        isCalculationDone = false;
      }
      if (isCalculationDone && !isNaN(value)) {
        clearDisplay();
        isCalculationDone = false;
      }

      displayValue.value += value;
      currentValue += value;
    };

    const clearDisplay = () => {
      displayValue.value = "";
      currentValue = "";
    };

    const calculate = () => {
      try {
        currentValue = eval(currentValue).toString();
        displayValue.value = currentValue;
        isCalculationDone = true;
      } catch (error) {
        displayValue.value = alert("Please Provide a Valid Input");
        isCalculationDone = true;
        currentValue = "";
      }
    };

    const clearLastDigit = () => {
      if (currentValue.length > 0) {
        currentValue = currentValue.slice(0, -1);
        displayValue.value = currentValue;
      }
    };

    const isOperator = (value) =>
      ["+", "-", "*", "/", "C", "D"].includes(value);
    const isNumber = (value) => /^\d+$/.test(value) || value === ".";
    const isEqual = (value) => value === "=";
    const isPlus = (value) => value === "+";
    const isCVal = (value) => value === "C";

    const handleSpanClick = (value) => {
      switch (value) {
        case "=":
          calculate();
          break;
        case "C":
          clearDisplay();
          break;
        case "D":
          clearLastDigit();
          break;
        default:
          appendValue(value);
      }
    };

    return {
      displayValue,
      spanValues,
      handleSpanClick,
      isOperator,
      isNumber,
      isEqual,
      isPlus,
      isCVal,
    };
  },
};
</script>
