<template>
    <form @submit.prevent>
        <div class="inputs">
            <div class="element">
                Способ ввода:
                <my-select v-model="methodOfIntroducing" :options="methodOfIntroducingOptions"/>
            </div>
            <div class="element" v-if="methodOfIntroducing === 1 || methodOfIntroducing === 2 || methodOfIntroducing === 3">
                Количество точек:
                <my-select v-model="numberOfPoints" :options="numberOfPointsOptions" @change="changePoints"/>
            </div>
            <div class="element" v-if="methodOfIntroducing === 2">
                Файл:
                <my-input type="file" @change="changeFile"/>
            </div>
            <div class="element" v-if="methodOfIntroducing === 3">
                Функция:
                <my-select v-model="f" :options="fOptions" @change="changePoints"/>
                <div class="row">
                    <div class="interval">
                        a:
                        <my-input v-model='a' type="number" @change="changePoints"/>
                    </div>
                    <div class="interval">
                        b:
                        <my-input v-model='b' type="number" @change="changePoints"/>
                    </div>
                </div>
            </div>
            <div class="element" v-if="methodOfIntroducing === 1 || methodOfIntroducing === 2 || methodOfIntroducing === 3">
                <div class="row">
                    <div class="point">
                        <div>
                            x₁:
                            <my-input v-model='point1[0]' type="number"/>
                        </div>
                        <div>
                            y₁:
                            <my-input v-model="point1[1]" type="number"/>
                        </div>
                    </div>
                    <div class="point">
                        <div>
                            x₇:
                            <my-input v-model='point7[0]' type="number"/>
                        </div>
                        <div>
                            y₇:
                            <my-input v-model="point7[1]" type="number"/>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="point">
                        <div>
                            x₂:
                            <my-input v-model='point2[0]' type="number"/>
                        </div>
                        <div>
                            y₂:
                            <my-input v-model="point2[1]" type="number"/>
                        </div>
                    </div>
                    <div class="point">
                        <div>
                            x₈:
                            <my-input v-model='point8[0]' type="number"/>
                        </div>
                        <div>
                            y₈:
                            <my-input v-model="point8[1]" type="number"/>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="point">
                        <div>
                            x₃:
                            <my-input v-model='point3[0]' type="number"/>
                        </div>
                        <div>
                            y₃:
                            <my-input v-model="point3[1]" type="number"/>
                        </div>
                    </div>
                    <div v-if="numberOfPoints > 8" class="point">
                        <div>
                            x₉:
                            <my-input v-model='point9[0]' type="number"/>
                        </div>
                        <div>
                            y₉:
                            <my-input v-model="point9[1]" type="number"/>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="point">
                        <div>
                            x₄:
                            <my-input v-model='point4[0]' type="number"/>
                        </div>
                        <div>
                            y₄:
                            <my-input v-model="point4[1]" type="number"/>
                        </div>
                    </div>
                    <div v-if="numberOfPoints > 9" class="point">
                        <div>
                            x₁₀:
                            <my-input v-model='point10[0]' type="number"/>
                        </div>
                        <div>
                            y₁₀:
                            <my-input v-model="point10[1]" type="number"/>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="point">
                        <div>
                            x₅:
                            <my-input v-model='point5[0]' type="number"/>
                        </div>
                        <div>
                            y₅:
                            <my-input v-model="point5[1]" type="number"/>
                        </div>
                    </div>
                    <div v-if="numberOfPoints > 10" class="point">
                        <div>
                            x₁₁:
                            <my-input v-model='point11[0]' type="number"/>
                        </div>
                        <div>
                            y₁₁:
                            <my-input v-model="point11[1]" type="number"/>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="point">
                        <div>
                            x₆:
                            <my-input v-model='point6[0]' type="number"/>
                        </div>
                        <div>
                            y₆:
                            <my-input v-model="point6[1]" type="number"/>
                        </div>
                    </div>
                    <div v-if="numberOfPoints > 11" class="point">
                        <div>
                            x₁₂:
                            <my-input v-model='point12[0]' type="number"/>
                        </div>
                        <div>
                            y₁₂:
                            <my-input v-model="point12[1]" type="number"/>
                        </div>
                    </div>
                </div>
            </div>
            <div class="element" v-if="methodOfIntroducing === 1 || methodOfIntroducing === 2 || methodOfIntroducing === 3">
                <div>
                    x:
                    <my-input v-model='x' type="number"/>
                </div>
            </div>
        </div>
        <div class="commandButton" v-if="methodOfIntroducing === 1 || methodOfIntroducing === 2 || methodOfIntroducing === 3">
            <my-button type="button" @click="reset">Сбросить</my-button>
            <my-button type="button" @click="solve">Отправить</my-button>
        </div>
        <div class="emptiness">
        </div>
    </form>
</template>

<script>
import MyButton from "@/components/UI/MyButton.vue";
import MyInput from "@/components/UI/MyInput.vue";
import MySelect from "@/components/UI/MySelect.vue";
import axios from "axios";

export default {
    components: {MySelect, MyInput, MyButton},
    data() {
        return {
            x: 0,
            f: '',
            fOptions: [
                {value: 1, name: 'sin(x)'},
                {value: 2, name: 'x³'},
            ],
            a: 0,
            b: 0,
            methodOfIntroducing: '',
            methodOfIntroducingOptions: [
                {value: 1, name: 'Ввод с экрана'},
                {value: 2, name: 'Ввод с файла'},
                {value: 3, name: 'Выбрать функцию'},
            ],
            numberOfPoints: 8,
            numberOfPointsOptions: [
                {value: 8, name: '8'},
                {value: 9, name: '9'},
                {value: 10, name: '10'},
                {value: 11, name: '11'},
                {value: 12, name: '12'},
            ],
            point1: [],
            point2: [],
            point3: [],
            point4: [],
            point5: [],
            point6: [],
            point7: [],
            point8: [],
            point9: [],
            point10: [],
            point11: [],
            point12: [],
            points: [],
        };
    },
    methods: {
        solve() {
            this.points = []
            this.points[0] = this.point1
            this.points[1] = this.point2
            this.points[2] = this.point3
            this.points[3] = this.point4
            this.points[4] = this.point5
            this.points[5] = this.point6
            this.points[6] = this.point7
            this.points[7] = this.point8
            if (this.numberOfPoints > 8) {
                this.points[8] = this.point9
            }
            if (this.numberOfPoints > 9) {
                this.points[9] = this.point10
            }
            if (this.numberOfPoints > 10) {
                this.points[10] = this.point11
            }
            if (this.numberOfPoints > 11) {
                this.points[11] = this.point12
            }
            let errors = []
            let i = 1;
            // console.log(this.points)
            let left = Number.parseFloat(this.points[0][0])
            let right = Number.parseFloat(this.points[0][0])
            let bottom = Number.parseFloat(this.points[0][1])
            let top = Number.parseFloat(this.points[0][1])
            for(let point in this.points) {
                if(this.points[point][0] !== '' && this.points[point][1] !== '') {
                    if(Number.parseFloat(this.points[point][0]) < left) {
                        left = Number.parseFloat(this.points[point][0]);
                    }
                    if(Number.parseFloat(this.points[point][0])> right) {
                        right = Number.parseFloat(this.points[point][0]);
                    }
                    if (Number.parseFloat(this.points[point][1]) < bottom) {
                        bottom = Number.parseFloat(this.points[point][1])
                    }
                    if (Number.parseFloat(this.points[point][1]) > top) {
                        top = Number.parseFloat(this.points[point][1])
                    }
                } else {
                    if (this.points[point][0] === '') {
                        errors.push("Введите значение x" + i)
                    }
                    if (this.points[point][1] === '') {
                        errors.push("Введите значение y" + i)
                    }
                }
                i++;
            }
            for (let j = 1; j < this.numberOfPoints; j++) {
                if (Number.parseFloat(this.points[j][0]) <= Number.parseFloat(this.points[j - 1][0])){
                    errors.push("Проверьте, что все значения x идут в порядке возрастания")
                    break
                }
            }
            if (this.x < left || this.x > right){
                errors.push("Проверьте, что все значение x входит в интервал")
            }
            if(errors.length > 0) {
                this.$emit("error", errors);
                return;
            }
            axios
                .post(
                    "http://localhost:8081/api/interpolate",
                    {
                        points: this.points,
                        x: this.x,
                    },
                ).then((res) => {
                this.$emit("solve", res.data, left - (right - left)/8, right + (right - left)/8,
                    bottom - (top - bottom)/8, top + (top - bottom)/8, this.points);
            }).catch((error) => {
                this.$emit("error", error.response.data);
            });
        },

        updateValues(numberOfPoints, points) {
            this.numberOfPoints = numberOfPoints;
            this.point1 = points[0]
            this.point2 = points[1]
            this.point3 = points[2]
            this.point4 = points[3]
            this.point5 = points[4]
            this.point6 = points[5]
            this.point7 = points[6]
            this.point8 = points[7]
            if (this.numberOfPoints > 8) {
                this.point9 = points[8]
            }
            if (this.numberOfPoints > 9) {
                this.point10 = points[9]
            }
            if (this.numberOfPoints > 10) {
                this.point11 = points[10]
            }
            if (this.numberOfPoints > 11) {
                this.point12 = points[11]
            }
        },
        changeFile(event) {
            if (event.target.files[0].type === "text/plain") {
                let reader = new FileReader();
                reader.readAsText(event.target.files[0], 'windows-1251')
                let results;
                reader.onload = () => {
                    results = reader.result.split("\r\n");
                    if (results.length < 9 || results.length > 13) {
                        let errors = ["Неверное количество строк в файле(ожидается от 10 до 14 строк)."]
                        this.$emit("error", errors);
                    } else {
                        if (!isNaN(parseFloat(results[0]))) {
                            let numberOfPoints = parseFloat(results[0])
                            if (results.length !== numberOfPoints + 1) {
                                let errors = ["Неверное количество строк в файле."]
                                this.$emit("error", errors);
                            } else {
                                let points = []
                                for (let i = 1; i < numberOfPoints + 1; i++) {
                                    results[i] = results[i].trim()
                                    results[i] = results[i].replaceAll(',', '.')
                                    let pointString = results[i].split(" ")
                                    if (pointString.length === 2) {
                                        if (!isNaN(parseFloat(pointString[0])) &&
                                            !isNaN(parseFloat(pointString[1]))) {
                                            let point = [pointString[0],
                                                pointString[1]]
                                            points.push(point);
                                        } else {
                                            console.log(pointString[0])
                                            console.log(pointString[1])
                                            let errors = ["Значения точек должны быть числами"]
                                            this.$emit("error", errors);
                                        }
                                    } else {
                                        let errors = ["В строках с точками ожидается 2 числа"]
                                        this.$emit("error", errors);
                                    }
                                }
                                this.updateValues(numberOfPoints, points);
                            }
                        } else {
                            let errors = ["Значение 'Количество точек' должно быть числом"]
                            this.$emit("error", errors);
                        }
                    }
                };
                reader.onerror = function () {
                    this.$emit("error", "Произошла ошибка чтения из файла");
                };
            } else {
                this.$emit("error", "Произошла ошибка чтения из файла");
            }
        },
        changePoints() {
            if (this.methodOfIntroducing === 3 && this.b > this.a && this.f !== '') {
                let points = [];
                let h = (this.b - this.a) / (this.numberOfPoints - 1);
                for (let i = 0; i < this.numberOfPoints; i++) {
                    let point = []
                    point[0] = (this.a * 1 + i * h).toFixed(4);
                    if (this.f === 1){
                        point[1] = Math.sin(point[0]).toFixed(4);
                    } else {
                        point[1] = Math.pow(point[0], 3).toFixed(4);
                    }
                    points[i] = point
                    this.updateValues(this.numberOfPoints, points);
                }
            }
        },
        reset() {
            this.x = 0
            this.f = ''
            this.a = 0
            this.b = 0
            this.methodOfIntroducing = ''
            this.numberOfPoints = 8
            this.point1 = []
            this.point2 = []
            this.point3 = []
            this.point4 = []
            this.point5 = []
            this.point6 = []
            this.point7 = []
            this.point8 = []
            this.point9 = []
            this.point10 = []
            this.point11 = []
            this.point12 = []
            this.points = []
            this.$emit("reset");
        },
    },
};
</script>

<style scoped>

.commandButton {
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
}
.element {
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    margin: 10px;
}

.point {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 48%;
}
.interval {
    width: 48%;
}
.point div{
    width: 49%;
}
.row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.point div input {
    background-color: #828670;
}

</style>
