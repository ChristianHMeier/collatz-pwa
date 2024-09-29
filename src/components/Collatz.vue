<script setup>
import { ref, onMounted } from 'vue'
// import './../assets/js/BaseNumber.min.js'

const count = ref('0')

const text = ref('')


const byFourFound = ref(false)

onMounted(() => {
        const baseNumberScript = document.createElement("script");
        baseNumberScript.setAttribute(
            "src",
            "https://cdn.jsdelivr.net/gh/AlexSp3/Basenumber.js@main/BaseNumber.min.js"
        );
        baseNumberScript.setAttribute(
            'language',
            'javascript'
        )

        baseNumberScript.defer = true

        document.head.appendChild(baseNumberScript)
    })


async function calculateCollatz(startNumber, firstIteration) {
  if (startNumber <= 0) {
    text.value = 'Only positive numbers!'
    return
  }

  let isByFour = false
  // clean values on button click
  if (firstIteration) {
    text.value = ''
    byFourFound.value = false
  }
  
  try {
    let baseNumber = BigInt(startNumber)
    
    // if possible, spot first pure multiple of 4, from where the trivial cycle is always reached after an odd number
    try {
      isByFour =  Base(Base(baseNumber).log(4n).valueOf()).isInt()
    } catch (error) {
      console.info("Number '" + baseNumber + "' exceeds the threshold for safe integers in JS, logarithmic verification is not possible")
      isByFour = false
    }

    if (isByFour && !byFourFound.value) {
      text.value += baseNumber.toString() + " <-First pure multiple of 4!\n"
      byFourFound.value = true
    } else {
      text.value += baseNumber.toString() + "\n"
    }

    // avoid infinite loop when 1 is reached
    if (baseNumber === 1n) {
      return
    }

    // the proper function of the Collatz Inference
    if (baseNumber % 2n !== 0n) {
      calculateCollatz(3n*baseNumber +1n, false)
    } else {
      calculateCollatz(baseNumber / 2n, false);
    }
  } catch (e) {
    text.value = 'Not a number!'
    return
  }

}
</script>

<template>
  <div class="row align-items-center">
    <div class="offset-1 offset-md-3 offset-lg-4 col-4 col-md-3 col-lg-2 mb-3">
    <label for="inputNumber">Starting Number</label>
</div>
  <div class="col-6 col-lg-2 mb-3">
    <input type="text" step="1" id="inputNumber" min="1" v-model="count" />
  </div>
<div class="col-12 mb-3 text-center">
    <button type="button" class="btn btn-primary" @click="calculateCollatz(count, true)">Calculate!</button>
  </div>
  <div id="bottom" class="col-12 mb-3 px-5">
    <textarea class="w-100" v-model="text"></textarea>
  </div>
</div>
</template>

<style scoped>
.row {
  height: 90vh;
}
#bottom {
  height: 100%;
  max-height: 70vh;
}
textarea {
  height: 100%;
}
</style>
