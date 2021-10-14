<template>
    <div>
        <div>당첨 숫자</div>
        <div id="결과창">
            <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"></lotto-ball>
        </div>
        <div>
            <lotto-ball v-if="bonus" :number="bonus"></lotto-ball>
            <button @click="onClickRedo" v-if="redo">한 번 더!</button>
        </div>
    </div>
</template>

<script>
import LottoBall from "./LottoBall.vue";

function getWinNumbers() {
    console.log("getWinNumbers");
    const candidate = Array(45)
        .fill()
        .map((v, i) => i + 1);
    const shuffle = [];
    while (candidate.length > 0) {
        shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
    }
    const bonusNumber = shuffle[shuffle.length - 1];
    const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
    return [...winNumbers, bonusNumber];
}
export default {
    components: {
        "lotto-ball": LottoBall,
    },
    data() {
        return {
            winNumbers: getWinNumbers(),
            winBalls: [],
            bonus: null,
            redo: false,
        };
    },
    methods: {
        onClickRedo() {
            this.winNumbers = getWinNumbers();
            this.winBalls = [];
            this.bonus = null;
            this.redo = false;
            this.showBalls();
        },
        showBalls() {
            for (let i = 0; i < this.winNumbers.length - 1; i++) {
                setTimeout(() => {
                    this.winBalls.push(this.winNumbers[i]);
                }, (i + 1) * 1000);
            }
            setTimeout(() => {
                this.bonus = this.winNumbers[6];
                this.redo = true;
            }, 7000);
        },
    },

    computed: {},
    whatch: {
        winBalls(val, oldVal) {
            if (val.length === 0) {
                this.showBalls();
            }
        },
    },
    mounted() {
        this.showBalls();
    },
};
</script>

<style></style>
