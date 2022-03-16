<template>
    <div class="calculator">
        <DisplayComponent :value="displayValue"></DisplayComponent>
        <ButtonComponent label="AC" triple @CalcButton="clearMemory"></ButtonComponent>
        <ButtonComponent label="/" operation @CalcButton="setOperation"></ButtonComponent>
        <ButtonComponent label="7" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="8" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="9" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="*" operation @CalcButton="setOperation"></ButtonComponent>
        <ButtonComponent label="4" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="5" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="6" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="-" operation @CalcButton="setOperation"></ButtonComponent>
        <ButtonComponent label="1" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="2" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="3" @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="+" operation @CalcButton="setOperation"></ButtonComponent>
        <ButtonComponent label="0" double @CalcButton="addDigit"></ButtonComponent>
        <ButtonComponent label="." @CalcButton="addDigit" ></ButtonComponent>
        <ButtonComponent label="=" operation @CalcButton="setOperation" ></ButtonComponent>
    </div>
</template>

<script>
import DisplayComponent from "@/components/Display";
import ButtonComponent from "@/components/Button";

export default {
    name: "CalculatorComponent",
    components :{
        ButtonComponent,
        DisplayComponent,
    },
    data: function (){
      return {
          displayValue: "0",
          clearDisplay: false,
          operation: null,
          values: [0, 0],
          current: 0
      }
    },
    methods: {
        clearMemory() {
            Object.assign(this.$data, this.$options.data()) //volta o objeto do component ao estado inicial
        },
        setOperation(o) {
            if(this.current === 0) {
                this.operation = o
                this.current = 1
                this.clearDisplay = true
            } else {
                const equals = o === "="
                const currentOperation = this.operation

                try {
                    this.values[0] = eval(`${this.values[0]} ${currentOperation} ${this.values[1]}`)
                } catch (e) {
                    this.$emit('onError', e)
                }

                this.values[1] = 0

                this.displayValue = this.values[0]
                this.operation = equals ? null : o
                this.current = equals ? 0 : 1
                this.clearDisplay = !equals

            }
        },
        addDigit(n) {
            if(n === "." && this.displayValue.includes(".")){
                return
            }

            const clear = this.displayValue === "0" || this.clearDisplay
            const currentValue = clear ? "" : this.displayValue
            const display = currentValue + n

            this.displayValue = display
            this.clearDisplay = false
            this.values[this.current] = display

        }
    }
}
</script>

<style scoped>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}

</style>