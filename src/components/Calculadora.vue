<script setup>

const props = defineProps(['igual', 'getNumbers', 'show', 'setNumbers', 'setOperacao', 'pegarElemento', 'apagar', 'zerar', 'eventoTeclado', 'novoModo', 'novoModoResposta'])

</script>

<template>
    <main @keydown="props.eventoTeclado" class="main" :class="{ 'main--modo-light': props.novoModoResposta }">
        <div class="calculadora">
            <p class="calculadora__display">{{ props.show }}</p>
            <div class="numero">
                <ul class="numero__list">
                    <li class="numero__list__item" @click="props.setNumbers" v-for="numero in props.getNumbers">
                        {{ numero }}
                    </li>
                    <li class="numero__list__item" @click="props.setOperacao" data-operacao="soma">+</li>
                    <li class="numero__list__item" @click="props.setOperacao" data-operacao="subtracao">-</li>
                    <li class="numero__list__item" @click="props.setOperacao" data-operacao="divisao">/</li>
                    <li class="numero__list__item" @click="props.setOperacao" data-operacao="multiplicacao">x</li>
                    <li class="numero__list__item" @click="props.setOperacao" data-operacao="porcentagem">%</li>
                    <li class="igual numero__list__item" @click="props.igual">
                        =
                    </li>
                    <li class="numero__list__item" @click="props.zerar">C</li>
                    <li class="delete numero__list__item" @click="props.apagar">
                        <i class="fa-solid fa-delete-left"></i>
                    </li>
                </ul>
            </div>
        </div>
        <div class="interruptor">
            <input @change="props.novoModo" class="check" id="check" type="checkbox">

            <label class="sun-moon" for="check">
                <i class="sun fa-solid fa-sun"></i>

                <i class="moon fa-solid fa-moon"></i>
            </label>
        </div>
    </main>

</template>

<style lang="scss">
@import '@/scss/main.scss';
</style>