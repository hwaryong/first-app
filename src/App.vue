<script setup>
import { reactive } from 'vue';
const input = reactive({
    amount: null,
    yir: null,
    duration: null
})

const output = reactive({
    months: []
})

const formData = [
    { name: 'amount', placeholder: '대출액 (원)'  },
    { name: 'yir', placeholder: '연이율 (%)'  },
    { name: 'duration', placeholder: '상환 기간 (개월)'  },
];

function calc() {
    // 최적값을 구해서 output.months에 대입
    let x = Math.floor(input.amount / input.duration)
    let done = false
    while (done == false) {
        console.log('trying', x)
        let result = test(x)
        if (result) {
            output.months = result
            return true
        }
        else {
            x = x + 1
        }
    }
}

function test(x) {
    let array = []
    let b = input.amount
    let mir = input.yir / 100 / 12
    for (let j = 1; j < input.duration; j++) {
        let i = Math.floor(b * mir)
        b = b + i - x
        array.push({ x, i, b })
    }
    console.log(array)
    // 성공
    if (b <= x) {
        let i = Math.floor(b * mir)
        array.push({ x: b + i, i, b: 0 })
        return array
    }
    else return false
    // 최적값을 구해주는 함수
    /*
    100원 / 월 2% / 4개월
    [
        { x: 26, i: 4, b: 78 },
        { x: 26, i: 3, b: 55 },
        { x: 26, i: 2, b: 31 },
        { x: 31}, ==> 실패!!
    ]
    */
}
</script>



<template>
    <!-- 전체 컨테이너 -->
    <div class="p-4">
        <!-- 제목 -->
        <div class="text-4xl font-bold mb-8">대출 이자 계산기</div>
        <!-- 조건 -->
        <div class="flex flex-col gap-4">
            <input
                v-for="data of formData"
                :name="data.name"
                :placeholder="data.placeholder"
                class="p-4 border border-black"
                v-model="input[data.name]"
            >
            <button @click="calc">계산</button>
        </div>

        <div class="my-4">
            <div
                v-for="month of output.months"
                class="grid grid-cols-4 gap-4 border-b border-black p-2"
            >
                <div>{{ month.x }}</div>
                <div>{{ month.x - month.i }}</div>
                <div>{{ month.i }}</div>
                <div>{{ month.b }}</div>
            </div>

        </div>
    </div>
</template>

<style>
</style>