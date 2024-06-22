<template>
    <div class="card-box">
        <div class="card">
            <img src="" alt="">
            <h3>تست</h3>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const name = ref('ali');
const family = ref('reza');

let test = [name.value, family.value]
let result = test.every((item) => { return item.length > 0 })
console.log(result)




</script>

<style scoped>
.card-box {
    display: flex;
    flex-wrap: wrap;
}

.card {
    width: 100%;
    max-width: 10rem;
    padding: 0.5rem;
    border-radius: 8px;
    box-shadow: 0px 0px 8px 0px rgba(117, 117, 117, 0.75);
}

.card img {
    aspect-ratio: 16 / 9;
    border-radius: 6px;

}

.card h3 {
    text-align: end;
    margin: 0.8rem 0;
}
</style>