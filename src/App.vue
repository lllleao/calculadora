<script setup>
import { onMounted, onUnmounted, reactive } from 'vue'
import Calculadora from './components/Calculadora.vue'
const state = reactive({
  numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 0, ','],
  number01: '',
  number02: '',
  simboloOperacao: '',
  nomeOperacaoAtual: '',
  numeroDaConta: '',
  resultado: 0,
  dPorZero: 'Impossível dividir por 0',
  converterPonto: '',
  number01Virgula: '',
  number02Virgula: '',
  juntarNumVirgula: '',
  novoModo: false
})

const divisaoPorZero = (n1, n2) => {
  const operacaoTeste = parseFloat(n1) / parseFloat(n2)

  if (n2 == 0) {
    return state.dPorZero
  } else {
    return operacaoTeste
  }
}

const operacao = () => {
  const { number01, number02, nomeOperacaoAtual } = state

  switch (nomeOperacaoAtual) {
    case 'soma':
    case '+':
      return parseFloat(number01) + parseFloat(number02)
    case 'subtracao':
    case '-':
      return parseFloat(number01) - parseFloat(number02)
    case 'divisao':
    case '/':
      return divisaoPorZero(number01, number02)
    case 'multiplicacao':
    case '*':
      return parseFloat(number01) * parseFloat(number02)
    case 'porcentagem':
    case '%':
      return (parseFloat(number01) / 100) * parseFloat(number02)
  }
}

// Início dos SetNumbers
const setNumber01 = (numero) => {
  state.number01Virgula = adicionarVirgula(numero.replace('.', ','))
  state.number01 = numero.replace(',', '.')
}

const setNumber02 = (numero) => {
  state.number02Virgula = adicionarVirgula(numero.replace('.', ','))
  state.number02 = numero.replace(',', '.')
}

const seMensagemDividirPorZero = () => {
  if (state.numeroDaConta === state.dPorZero) {
    state.numeroDaConta = ''
  }
}

const apagarPontoDuplicado = () => {
  const existe = -1
  const jaTemPonto = state.numeroDaConta.indexOf(',') !== existe ? true : false
  const doisPontos = state.numeroDaConta.indexOf(',,') !== existe ? true : false

  if (jaTemPonto) {

    if (doisPontos) {
      state.numeroDaConta = state.numeroDaConta.replace(',,', ',')
    }

    const pontos = [...state.numeroDaConta].filter(item => {
      return item === ','
    })

    if (pontos.length > 1) {
      state.numeroDaConta = state.numeroDaConta.slice(0, state.numeroDaConta.lastIndexOf(','))
    }
  }

}

function adicionarVirgula(numero) {
  let inde = numero.indexOf(',')
  state.juntarNumVirgula = inde !== -1 ? numero.slice(inde) : ''
  state.converterPonto = inde !== -1 ? numero.slice(0, inde) : numero

  let resultado = '';

  let tamanho = state.converterPonto.length;
  let diferenca = tamanho % 3;

  for (let i = 0; i < tamanho; i++) {
    if (i > 0 && (i - diferenca) % 3 === 0) {
      resultado += '.';
    }
    resultado += state.converterPonto[i];
  }
  return resultado + state.juntarNumVirgula
}

const setNumbers = (evento) => {
  seMensagemDividirPorZero()
  if (verificaSeEObjeto(evento)) {
    state.numeroDaConta += evento
  } else {
    state.numeroDaConta += evento.target.innerHTML
  }
  apagarPontoDuplicado()


  const seComecaComPonto = state.numeroDaConta[0] === ','
  const seComecaComZero = state.numeroDaConta[0] === '0' && state.numeroDaConta[1] !== ','

  if (seComecaComPonto) {
    state.numeroDaConta = ''
  } else if (seComecaComZero) {
    state.numeroDaConta = '0'
  }

  const listaNomesOp = ['soma', 'subtracao', 'multiplicacao', 'divisao', 'porcentagem', '+', '-', '/', '*', '%']

  const verificacao = listaNomesOp.some(item => {
    return item === state.nomeOperacaoAtual
  })

  if (verificacao) {
    setNumber02(state.numeroDaConta)
  } else {
    setNumber01(state.numeroDaConta)
  }

}

// Fim dos SetNumbers

const resultadoZero = () => {
  const verificacao = Boolean(state.number01 === '0' && !state.number02 && !state.nomeOperacaoAtual)

  if (verificacao) {
    state.numeroDaConta = ''
  } else {
    state.numeroDaConta = state.number01
  }
}

const setOperacao = (item) => {

  let { number01, number02 } = state


  const verificacao = number01.length >= 1 && number02.length === 0 && state.number01 !== state.dPorZero

  if (verificacao) {
    if (verificaSeEObjeto(item)) {
      state.nomeOperacaoAtual = item
      state.simboloOperacao = item
      state.numeroDaConta = ''
    } else {
      state.nomeOperacaoAtual = item.target.dataset.operacao
      state.simboloOperacao = item.target.innerHTML
      state.numeroDaConta = ''
    }
  } else if (!verificacao) {
    // pass
  }

}


const show = () => {
  const resultadoTela = `${state.number01Virgula} ${state.simboloOperacao} ${state.number02Virgula}`
  if (state.number01Virgula === '') {
    return '...'
  } else {
    return resultadoTela
  }

}

const igual = (e) => {
  const verificacao = Boolean(state.number02)
  state.resultado = operacao()
  if (verificacao) {
    state.number01 = state.resultado.toString()
    state.number02 = ''

    const verificacaoDPorZero = state.resultado === state.dPorZero ? state.resultado : adicionarVirgula(state.number01.replace('.', ','))

    state.number01Virgula = verificacaoDPorZero
    state.number02Virgula = ''
    state.simboloOperacao = ''
    state.nomeOperacaoAtual = ''
    resultadoZero()
  }
}

const apagarElementoComVirgula = () => {
  let listaDeElementosComVirgula = []

  const verificacao = state.number02Virgula.length === 0 && state.simboloOperacao.length === 0
  if (verificacao) {

    listaDeElementosComVirgula = [...state.number01Virgula]
    listaDeElementosComVirgula.pop()


    if (listaDeElementosComVirgula.length === 0) {
      state.number01Virgula = ''
    } else {
      state.number01Virgula = ''

      listaDeElementosComVirgula.forEach(itens => {
        state.number01Virgula += itens
      })
      state.number01Virgula = adicionarVirgula(state.number01Virgula.replace(/\./g, ''))

    }
  } else if (state.number02Virgula.length !== 0) {
    listaDeElementosComVirgula = [...state.number02Virgula]
    listaDeElementosComVirgula.pop()

    if (listaDeElementosComVirgula.length === 0) {
      state.number02Virgula = ''
    } else {
      state.number02Virgula = ''

      listaDeElementosComVirgula.forEach(itens => {
        state.number02Virgula += itens
      })
      state.number02Virgula = adicionarVirgula(state.number02Virgula.replace(/\./g, ''))
    }
  } else {
    state.nomeOperacaoAtual = state.simboloOperacao = ''
    state.numeroDaConta = state.number01
  }
}

const apagarElemento = () => {
  let listaDeElementos = []
  const verificacao = state.number02.length === 0 && state.simboloOperacao.length === 0
  const impossivelDPorZero = state.number01 === state.dPorZero
  if (verificacao) {

    if (impossivelDPorZero) {
      state.number01 = ''
    } else {
      listaDeElementos = [...state.number01]
      listaDeElementos.pop()


      if (listaDeElementos.length === 0) {
        state.numeroDaConta = state.number01 = ''
      } else {
        state.numeroDaConta = state.number01 = ''

        listaDeElementos.forEach(itens => {
          state.numeroDaConta = state.number01 += itens
        })

      }
    }
  } else if (state.number02.length !== 0) {
    listaDeElementos = [...state.number02]
    listaDeElementos.pop()

    if (listaDeElementos.length === 0) {
      state.numeroDaConta = state.number02 = ''
    } else {
      state.numeroDaConta = state.number02 = ''

      listaDeElementos.forEach(itens => {
        state.number02 = state.numeroDaConta += itens
      })
    }
  }
}

const apagar = () => {
  apagarElemento()
  apagarElementoComVirgula()
}

const zerar = () => {
  state.numeroDaConta = ''
  state.number01 = ''
  state.number02 = ''
  state.number01Virgula = ''
  state.number02Virgula = ''
  state.simboloOperacao = ''
  state.nomeOperacaoAtual = ''
}

const verificaSeEObjeto = item => {
  return typeof item !== 'object'
}

const eventoTeclado = evento => {
  const teclado = evento.key
  const verificacaoNumeros = '1234567890,'.includes(teclado)
  const verificacaoOperacao = '+-/*%'.includes(teclado)
  const verificacaoIgual = teclado === 'Enter'
  const verificacaoDelete = teclado === 'Backspace'
  const verificacaoSeta = teclado === 'ArrowRight' || teclado === 'ArrowLeft'

  if (verificacaoNumeros) {
    setNumbers(teclado)
  } else if (verificacaoOperacao) {
    setOperacao(teclado)
  } else if (verificacaoIgual) {
    igual()
  } else if (verificacaoDelete) {
    apagar()
  } else if (verificacaoSeta) {
    state.novoModo = teclado === 'ArrowRight'
  }
}


onMounted(() => {
  document.querySelector('body').addEventListener('keydown', eventoTeclado)
  document.querySelector('label').addEventListener('click', () => {
    state.novoModo = state.novoModo === true ? false : true
  })
})

onUnmounted(() => {
  document.querySelector('body').removeEventListener('keydown', eventoTeclado)
  document.querySelector('label').addEventListener('click', () => {
    state.novoModo = state.novoModo === true ? false : true
  })
})

</script>


<template>
  <Calculadora :get-numbers="state.numbers" :show="show()" :set-numbers="setNumbers" :set-operacao="setOperacao"
    :igual="igual" :apagar="apagar" :zerar="zerar" :evento-teclado="eventoTeclado" :novo-modo-resposta="state.novoModo" />
</template>


<style scoped></style>
