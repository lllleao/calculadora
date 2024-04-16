<script setup>
import { reactive } from 'vue'
import Calculadora from './components/Calculadora.vue'

const state = reactive({
  numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, '.'],
  number01: '',
  number02: '',
  simboloOperacao: '',
  nomeOperacaoAtual: '',
  numeroDaConta: '',
  resultado: 0,
  dPorZero: 'Impossível dividir por 0',
})

const divisaoPorZero = (n1, n2) => {
  const operacaoTeste = parseFloat(n1) / parseFloat(n2)

  if (isNaN(operacaoTeste)) {
    return '...'
  } else if (n2 == 0) {
    return state.dPorZero
  } else {
    return operacaoTeste
  }
}

const operacao = () => {
  const { number01, number02, nomeOperacaoAtual } = state

  switch (nomeOperacaoAtual) {
    case 'soma':
      return parseFloat(number01) + parseFloat(number02)
    case 'subtracao':
      return parseFloat(number01) - parseFloat(number02)
    case 'divisao':
      return divisaoPorZero(number01, number02)
    case 'multiplicacao':
      return parseFloat(number01) * parseFloat(number02)
    case 'porcentagem':
      return (parseFloat(number01) / 100) * parseFloat(number02)
  }
}



const setNumber01 = (numero) => {
  state.number01 = numero
}

const setNumber02 = (numero) => {
  state.number02 = numero
}

const setNumbers = (evento) => {
  state.numeroDaConta += evento.target.innerHTML
  const listaNomesOp = ['soma', 'subtracao', 'multiplicacao', 'divisao', 'porcentagem']
  if (state.numeroDaConta[0] === '.') {
    state.numeroDaConta = ''
  }
  const teste = listaNomesOp.some(item => {
    return item === state.nomeOperacaoAtual
  })

  if (teste) {
    setNumber02(state.numeroDaConta)
  } else {
    setNumber01(state.numeroDaConta)
  }

}

const resultadoZero = () => {
  const verificacao = Boolean(state.number01 === '0' && !state.number02 && !state.nomeOperacaoAtual)
  if (verificacao) {
    state.numeroDaConta = ''
  } else {
    state.numeroDaConta = state.number01
  }
  return state.numeroDaConta
}

const setOperacao = (item) => {
  let { number01, number02 } = state


  const verificacao = number01.length >= 1 && number02.length === 0

  if (verificacao) {
    state.nomeOperacaoAtual = item.target.dataset.operacao
    state.simboloOperacao = item.target.innerHTML
    state.numeroDaConta = ''
  } else if (!verificacao) {
    // pass
  }

}

const show = () => {
  const resultadoTela = `${state.number01} ${state.simboloOperacao} ${state.number02}`
  if (state.number01 === '') {
    return '...'
  } else {
    return resultadoTela
  }

}

const igual = (e) => {
  state.resultado = operacao()
  state.number01 = state.resultado.toString()
  state.number02 = ''
  state.simboloOperacao = ''
  state.nomeOperacaoAtual = ''
  resultadoZero()
}

const apagarElemento = () => {
  const listaDeElementos = []

  if (state.number02.length === 0 && state.simboloOperacao.length === 0) {
    for (let i = 0; i < state.number01.length; i++) {
      listaDeElementos.push(state.number01[i])
    }
    listaDeElementos.pop()

    if (listaDeElementos.length === 0) {
      state.numeroDaConta = ''
      state.number01 = ''
    } else {
      state.numeroDaConta = ''
      state.number01 = ''
      listaDeElementos.forEach(itens => {
        state.number01 += itens
        state.numeroDaConta += itens
      })
    }
  }

  if (state.number02.length !== 0) {
    for (let i = 0; i < state.number02.length; i++) {
      listaDeElementos.push(state.number02[i])
    }
    listaDeElementos.pop()

    if (listaDeElementos.length === 0) {
      state.numeroDaConta = ''
      state.number02 = ''
    } else {
      state.numeroDaConta = ''
      state.number02 = ''
      listaDeElementos.forEach(itens => {
        state.number02 += itens
        state.numeroDaConta += itens
      })
    }
    console.log(state.number02.length)
  } else {
    state.nomeOperacaoAtual = ''
    state.simboloOperacao = ''
  }

}

const apagar = (evento) => {
  apagarElemento()

}


</script>


<template>
  <main class="main">
    <div class="container">
      <Calculadora :get-numbers="state.numbers" :show="show()" :set-numbers="setNumbers" :set-operacao="setOperacao"
        :igual="igual" :apagar="apagar" />
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
