<template>
  <div class="flex h-screen pb-96 flex-col p-2 place-content-center items-center bg-gray-400" >
    <input
    @input="changInputValue"
    @keyup.enter="onCalculatClick"
    id="input"
    v-model="displayValue"
    class="m-2 text-xl text-center  bg-yellow-700 "
    style="height: 70px; width: 305px"
    type="text"/>
   
    <div class="flex flex-row">

      <div class="grid grid-cols-1 gap-1 bg-green" style="width: fit-content">
        <keys :displayValue="displayValue" 
              :displayValueString="displayValueString" 
              :displayValueString2="displayValueString2" 
              :calculated="calculated" 
              :isNextString="isNextString" 
              @update="onNumClick"
              @setdot="onDotClick"
              @clear="onClerClick"/>
      </div>  

      <div class="grid grid-cols-1 gap-1 bg-green ml-4" style="width: fit-content">
        <oparator @update="onOparatorClick"/>
        <button @click="onCalculatClick" class="text-green sm:py-3 bg-orange-600 hover:bg-orange-900"> = </button>
      </div>

    </div>
  </div>
</template>


<script lang="ts" setup>
const displayValue = ref<string>([]);
const displayValueString = ref<string>([]);
const displayValueString2 = ref<string[]>([]);

const calculated = ref(false)
const isNextString = ref(false)
const isFromKeyboard = ref(false)


function onNumClick(value: String) {
  isFromKeyboard.value = false

  if (!isNextString.value && displayValueString.value == '' && value == '0' && displayValueString.value.slice(-1) != '.') {
    return
  } else if (isNextString.value && displayValueString2.value == '' && value == '0' && displayValueString2.value.slice(-1) != '.') {
    return
  }
  
  if (calculated.value) {
    if (isNextString.value == false) {
      if (!displayValueString.value && displayValueString.value == '') {
         displayValueString.value = value
      } else {
         displayValueString.value += value
      }
    } else {
      if (!displayValueString.value && displayValueString2.value == '') {
         displayValueString2.value = value
      } else {
         displayValueString2.value += value
      }
    }
  } else  {
    if (isNextString.value == false) {
      if (!displayValueString.value && displayValueString2.value == '') {
         displayValueString.value = value
      } else {
         displayValueString.value += value
      }
    } else {
      if (!displayValueString.value && displayValueString2.value == '') {
         displayValueString2.value = value
      } else {
         displayValueString2.value += value
      }
    }
  } 
  displayValue.value =  `${displayValueString.value}${displayValueString2.value}`
  focusInput()
}

function onOparatorClick(oparator: any) {
  if (displayValueString.value == '' && displayValue.value == '' && displayValue.value != '0') {
    return
  }
  if (displayValueString.value.slice(-1) == '.') {
    displayValueString.value = displayValueString.value.slice(0, -1)
  }

  if (!calculated.value) {
    if 
      (displayValueString.value.slice(-1) == '+' ||
      displayValueString.value.slice(-1) == '-' ||
      displayValueString.value.slice(-1) == '*' ||
      displayValueString.value.slice(-1) == '/' ) {
       

      if (isNextString.value == false) {
        displayValueString.value = displayValueString.value.slice(0, -1)
        displayValueString.value += oparator
        isNextString.value = true

      } else {
       if (displayValueString2.value == '') {
          displayValueString.value = displayValueString.value.slice(0, -1)
          displayValueString.value +=  oparator

        } else {
          onCalculatClick() 
          onOparatorClick(oparator)
        }
      }
    } else {
      displayValueString.value +=  oparator
      isNextString.value = true
    }

  } else {
    if (isFromKeyboard.value) {
      displayValueString.value += displayValue.value

    } else {
      displayValueString.value = displayValue.value 
      displayValueString.value +=  oparator
      
    }
    isNextString.value = true
  }
  displayValue.value =  `${displayValueString.value}${displayValueString2.value}`
  calculated.value = false
  focusInput()
}

function onCalculatClick() {
  const num1 = Number(displayValueString.value.slice(0, -1))
  const oparator = displayValueString.value.slice(-1)
  const num2 = Number(displayValueString2.value)
  
  if (!calculated.value) {
    if (oparator == '+') {
      displayValue.value = Number((num1+num2).toString())

    } else if (oparator == '-') {
      displayValue.value = Number((num1-num2).toString())

    } else if (oparator == '*') {
      displayValue.value = Number((num1*num2).toString())

    } else if (oparator == '/') {
      displayValue.value = Number((num1/num2).toString())
    } 
  
    isNextString.value = false
    calculated.value = true
    displayValueString.value = ''
    displayValueString2.value = ''  
  }
  focusInput()
}

function onDotClick() {
  if (isNextString.value == false) {
    if (!displayValueString.value.includes('.')) { 
      if (displayValueString.value && displayValueString.value != '') {
        displayValueString.value +=   '.'
      } else {
        displayValueString.value +=   '0.'
      }
    }   
  } else  {
    if (!displayValueString2.value.includes('.')) { 
      if (displayValueString2.value && displayValueString2.value  != '') {
          displayValueString2.value +=   '.'
        } else {
          displayValueString2.value +=   '0.'
        }
    }
  }
  displayValue.value =  `${displayValueString.value}${displayValueString2.value}`
  focusInput()
  isFromKeyboard.value = false
}
 
function onClerClick() {
  displayValue.value = ''
  displayValueString.value = ''
  displayValueString2.value = ''
  calculated.value = false
  isNextString.value = false
}

function focusInput() {
  const nextInput = document.getElementById('input');
  if (nextInput) {
    nextInput.focus();
  }
}

function changInputValue(event: any) {
  isFromKeyboard.value = true
  if (event.target.value.slice(-1) == '+' ||
      event.target.value.slice(-1) == '-' ||
      event.target.value.slice(-1) == '*' ||
      event.target.value.slice(-1) == '/' ) {
      
      onOparatorClick(event.target.value.slice(-1))

  } else if
      (event.target.value.slice(-1) == '0' ||
      event.target.value.slice(-1) == '1' ||
      event.target.value.slice(-1) == '2' ||
      event.target.value.slice(-1) == '3' ||
      event.target.value.slice(-1) == '4' ||
      event.target.value.slice(-1) == '5' ||
      event.target.value.slice(-1) == '6' ||
      event.target.value.slice(-1) == '7' ||
      event.target.value.slice(-1) == '8' ||
      event.target.value.slice(-1) == '9' ){

      onNumClick(event.target.value.slice(-1))
      
  } else if (event.target.value.slice(-1) == '.') {
      onDotClick()

  } else {
      displayValue.value = ''
  }
}

onMounted(async() => {
  focusInput()
})

</script>