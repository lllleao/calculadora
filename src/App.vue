<script setup>
import { reactive } from 'vue'
import Calculadora from './components/Calculadora.vue'

const state = reactive({
  number01: '',
  number02: '',
  operation: 'soma',
  dPorZero: 'Impossível dividir por 0'
})

const divisaoPorZero = (n1, n2) => {
  const operacaoTeste = parseFloat(n1) / parseFloat(n2)
  // console.log(operacaoTeste)

  if (isNaN(operacaoTeste)) {
    return '...'
  } else if (n2 == 0) {
    return state.dPorZero
  } else {
    return operacaoTeste
  }
}

const operacao = () => {
  const { number01, number02, operation } = state

  switch (operation) {
    case 'soma':
      return parseFloat(number01) + parseFloat(number02)
    case 'subtracao':
      return parseFloat(number01) - parseFloat(number02)
    case 'divisao':
      return divisaoPorZero(number01, number02)
    case 'multiplicacao':
      return parseFloat(number01) * parseFloat(number02)
  }
}

const fazerConta = (e) => {
  const resultado = operacao()

  if (resultado === state.dPorZero) {
    return state.dPorZero

  } else if (isNaN(resultado)) {
    return '...'

  } else {
    return resultado
  }
}

</script>

<template>
  <main class="main">
    <div class="container">
      <Calculadora :fazer-conta="fazerConta()" :get-number01="item => state.number01 = item.target.value" :get-number02="item => state.number02 = item.target.value" :get-operation="item => state.operation = item.target.value" />
    </div>
  </main>
</template>

<style scoped>
.main {
  height: 100vh;
  width: 100vw;
  background-color: #000;
}
</style>
