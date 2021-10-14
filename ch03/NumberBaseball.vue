<template>
    <div>
        <h1>{{ result }}</h1>
        <form @submit="onSubmitEventHandler" action="">
            <input v-model="value" ref="answer" maxlength="4" type="text" />
            <button type="submit">입력</button>
        </form>
        <div>시도: {{ tries.length }}</div>
        <ul>
            <li v-for="(item, index) in tries" :key="index">
                <div>{{ item.try }}</div>
                <div>{{ item.result }}</div>
            </li>
        </ul>
    </div>
</template>

<script>
const getNumbers = () => {
    const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const array = [];
    for (let index = 0; index < 4; index += 1) {
        const chosen = candidates.splice(Math.floor(Math.random() * (9 - index)), 1)[0];
        array.push(chosen);
    }
    return array;
};
export default {
    data() {
        return {
            answer: getNumbers(),
            tries: [],
            value: "",
            result: "",
        };
    },
    methods: {
        onSubmitEventHandler(e) {
            e.preventDefault();
            if (this.value === this.answer.join("")) {
                this.tries.push({
                    try: this.value,
                    result: "홈런",
                });
                this.result = "홈런";
                alert("게임을 다시 실행합니다.");
                this.tries = [];
                this.answer = getNumbers();
            } else {
                if (this.tries.length >= 9) {
                    this.result = `10넘게 돌려서 실패! 답은 ${this.answer.join(",")}였습니다`;
                    alert("게임을 다시 시작합니다.");
                    this.tries = [];
                    this.answer = getNumbers();
                }

                let strike = 0;
                let ball = 0;
                const answerArray = this.value.split("").map((v) => parseInt(v));
                for (let i = 0; i < 4; i += 1) {
                    if (answerArray[i] === this.answer[i]) {
                        strike++;
                    } else if (this.answer.includes(answerArray[i])) {
                        ball++;
                    }
                }
                this.tries.push({
                    try: this.value,
                    result: `${strike} 스트라이트. ${ball} 볼입니다.`,
                });
            }
            this.value = "";
            this.$refs.answer.focus();
        },
    },
};
</script>

<style></style>
