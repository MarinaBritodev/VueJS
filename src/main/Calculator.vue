<template>
    <div class="calculator">
        <Display :value="displayValue" />
        <button label="AC" triple @onClick="clearMemory" />
        <button label="/" operation @onClick="setOperation" />
        <button label="7" @onClick="addDigit" />
        <button label="8" @onClick="addDigit" />
        <button label="9" @onClick="addDigit" />
        <button label="*" operation @onClick="setOperation" />
        <button label="4" @onClick="addDigit" />
        <button label="5" @onClick="addDigit" />
        <button label="6" @onClick="addDigit" />
        <button label="-" operation @onClick="setOperation" />
        <button label="1" @onClick="addDigit" />
        <button label="2" @onClick="addDigit" />
        <button label="3" @onClick="addDigit" />
        <button label="+" operation @onClick="setOperation" />
        <button label="o" double @onClick="addDigit" />
        <button label="." @onClick="addDigit" />
        <button label="=" operation @onClick="setOperation" />
      
    </div>
</template>

<script>

import Button from "../components/Button"
import Display from "../components/Display"

import { parse } from 'path';

export default {

data: function() {
return{
    displayValue: "0",
    clearDisplay: false,
    operation : null,
    values: [0 ,0],
    current: 0
}
},
 components: { Button ,Display },
    methods: {
        clearMemory(){
                Object.assign(this.$data, this.$$options.data())
        },

        setOperation(operation){
            if (this.current === 0){
                this.operation = operation
                this.current =1
                this.clearDisplay = true
            }
               else {
                   const equals = operation === "="
                   const currentOperation = this.operation
               }     
               try{
                   this.values[0] = eval(
                       `${this.values[0]} ${currentOperation} ${this.values[1]}`
                   )
                   }catch(e){
                        this.$$emit('onError', e)
                   }
                   this.values[1] = 0

                   this.displayValue = this.values[0]
                   this.operation = equals ? null : operation
                   this.current = equals ? 0 : 1
                    this.clearDisplay = !equals
               }
        },
        addDigit(n){
            if (n == "."&& this.displayValue.includes(".")){
                return
            }
            const clearDisplay = this.displayValue =="0"
            || this.clearDisplay
            
            const currentValue = clearDisplay? "" : this.displayValue
            const displayValue =  currentValue + n
           
           
           //alternativa 1
            //this.displayValue = displayValue
            //this.clearDisplay = false
            //this.values[this.current] = displayValue


        //alternativa 2
      if (n !== "."){
          const i = this.current
        const newValue = parseFloat(displayValue)
          this.values[i] = newValue
           }
        }

    }


</script>

<style>
.calculator { 
    height: 320px; 
    width: 235px; 
    border-radius: 5px;  
    overflow: hidden;

    display: grid; 
    grid-auto-columns: repeat(4, 25%);   
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>
