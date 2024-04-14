<script setup>
const props = defineProps(['fazerConta', 'getNumber01', 'getNumber02', 'getOperation'])
</script>

<template>
    <div class="row">
        <div class="col-md-5 ms-auto me-auto mt-5 d-flex justify-content-center">
            <div class="calculadora p-3 rounded">
                <p class="mostrador text-center fs-3 bg-white rounded"> {{ props.fazerConta }} </p>

                <div class="numeros fs-3 ms-auto me-auto d-flex justify-content-center">
                    <input @keyup="props.getNumber01" class="w-25" type="number">
                    <select id="numeros" class="me-2 ms-2" @change="props.getOperation">
                        <option value="soma">
                            +
                        </option>
                        <option value="subtracao">
                            -
                        </option>
                        <option value="divisao">
                            /
                        </option>
                        <option value="multiplicacao">
                            x
                        </option>
                    </select>
                    <input @keyup="props.getNumber02" class="w-25" type="number">
                </div>

            </div>
        </div>
    </div>
</template>

<style scoped>
.calculadora {
    background-color: #3c6c88;
}
</style>