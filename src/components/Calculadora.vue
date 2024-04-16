<script setup>
const props = defineProps(['igual', 'getNumbers', 'show', 'setNumbers', 'setOperacao', 'pegarElemento', 'apagar'])

</script>

<template>
    <div class="row">
        <div class="col-5 ms-auto me-auto mt-5">
            <div class="calculadora p-5">
                <div class="row">
                    <div class="col">
                        <p class="mb-5 bg-light fs-2 text-center rounded">{{ props.show }}</p>
                    </div>
                </div>
                <div class="row">
                    <div class="numeros text-center">
                        <ul class="list-group">
                            <li @click="props.setNumbers" class="fs-2 list-group-item"
                                v-for="numero in props.getNumbers">
                                {{ numero }}
                            </li>
                            <li @click="props.setOperacao" class="fs-2 list-group-item" data-operacao="soma">+</li>
                            <li @click="props.setOperacao" class="fs-2 list-group-item" data-operacao="subtracao">-</li>
                            <li @click="props.setOperacao" class="fs-2 list-group-item" data-operacao="divisao">/</li>
                            <li @click="props.setOperacao" class="fs-2 list-group-item" data-operacao="multiplicacao">*</li>
                            <li @click="props.setOperacao" class="fs-2 list-group-item" data-operacao="porcentagem">%</li>
                            <li @click="props.igual" class="fs-2 list-group-item">
                                =
                            </li>
                            <li @click="props.apagar" class="fs-2 list-group-item">
                                <<
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.calculadora {
    background-color: #3c6c88;
    border-radius: 20px;

    p {
        border-radius: 20px;
    }
}

.list-group {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: .5rem;
    place-items: center;

    li {
        width: 6rem;
        height: 6rem;
        max-width: 100%;
        max-height: 100%;
        cursor: pointer;
        border-radius: 50px;
        display: flex;
        align-items: center;
        justify-content: center;

    }
}
</style>