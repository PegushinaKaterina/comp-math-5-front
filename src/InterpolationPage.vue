<template>
    <div class="app column">
        <div class="header">
            <h2>Интерполяция функции</h2>
        </div>
        <div class="main column">
            <div class="row">
                <div class="forms appElement column">
                    <div v-if="errors.length > 0" class="column appElement">
                        <div v-for="message in errors">
                            {{message}}
                        </div>
                    </div>
                    <div v-else>

                    </div>
                    <approximation-form class="form"
                                        @solve="solve"
                                        @error="error"
                    />
                </div>
                <div class="plots appElement column">
                    <plot :fnLagrangeProp="fLagrange" :fnNewtonProp="fNewton" :leftProp="left" :rightProp="right" :bottomProp="bottom" :topProp="top" :pointsProp="points"/>
                </div>
            </div>
            <div class="row">

                <div class="result appElement column">
                    Значение в заданной точке:<br>
                    по Лагранжу:<br>
                    {{l}}<br>
                    по Ньютону:<br>
                    {{n}}<br><br>
                    Конечные разности:<br>
                    <div v-for="finiteDifferencesRow, index in finiteDifferences" class="row">
                        <div style="margin: 10px">
                            <pre>{{table[index]}}</pre>
                        </div>
                        <div v-for="finiteDifference in finiteDifferencesRow" >
                            <div style="margin: 10px">
                                <pre>{{finiteDifference}}</pre>
                            </div>
                        </div>
                        <br>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import ApproximationForm from "@/components/InterpolationForm.vue";
import Plot from "@/components/Plot.vue";
import index from "vuex";

export default {
    computed: {
        index() {
            return index
        }
    },
    components: {
        Plot,
        ApproximationForm
    },
    name: "App",
    data() {
        return {
            fLagrange: '',
            fNewton: '',
            l: 0,
            n: 0,
            finiteDifferences: [],
            left: -5,
            right: 5,
            bottom: -5,
            top: 5,
            points: [],
            errors: [],
            table: [],
        };
    },
    methods: {
        solve(data, left, right, bottom, top, points) {
            this.fLagrange = data.lagrangeResponse.f;
            this.fNewton = data.newtonResponse.f
            this.l = data.lagrangeResponse.l;
            this.n = data.newtonResponse.n
            this.finiteDifferences = data.newtonResponse.finiteDifferences
            this.left = left
            this.right = right
            this.bottom = bottom
            this.top = top
            this.points = points
            this.errors = []
            let maxLength = 0
            for (let i = 0; i < this.finiteDifferences.length; i++) {
                for (let j = 0; j < this.finiteDifferences[i].length; j++) {
                    this.finiteDifferences[i][j] = this.finiteDifferences[i][j].toFixed(4)
                    if (this.finiteDifferences[i][j].length > maxLength) {
                        maxLength = this.finiteDifferences[i][j].length
                    }
                }
            }
            console.log(maxLength)
            this.table[0] = 'x  '
            for (let i = 0; i < this.finiteDifferences.length; i++) {
                if (i !== 0 && i < 10) {
                    this.table[i] = 'y' + i + ' '
                } else if (i !== 0) {
                    this.table[i] = 'y' + i
                }

                for (let j = 0; j < this.finiteDifferences[i].length; j++) {
                    while (this.finiteDifferences[i][j].length < maxLength) {
                        this.finiteDifferences[i][j] = " " + this.finiteDifferences[i][j]
                    }
                }
            }
        },
        reset() {
            this.fn = ""
            this.left = -5
            this.right = 5
            this.bottom = -5
            this.top = 5
            this.points = []
            this.errors = []
        },
        error(errors) {
            this.errors = errors;
            console.log(errors)
        },
    },
};
</script>


<style scoped>
*:global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    border-radius: 7px;
    background-color: #d7d4c9;
    font-family: "Courier New";
    font-weight: bolder;
    color: black;
}

.app {
    width: 100%;
    padding: var(--padding-size);
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    --text-size: 15px;
    --text-size-header: 40px;
    --padding-size: 5px;
    --button-size: 110px;
    --button-padding-size: 5px;
    --element-form-text-size: 15px;
}

.main {
    font-size: var(--text-size);
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    margin: 20px;
    padding: 20px;
}

.header {
    width: 100%;
    text-align: center;
    font-size: var(--text-size-header);
    margin: 50px 0px 20px 0px;
}

.result {
    width: 1160px;
    display: flex;
    flex-direction: row;
}

.appElement {
    //height: 650px;
    margin: 20px;
    padding: 20px;
    border: 1.5px solid black;
}

.forms {
    width: 600px;
    //justify-content: space-evenly;
    --button-size: 130px;
    --button-padding-size: 10px;
    --element-form-text-size: 15px;
}
.row {
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
}
.column {
    display: flex;
    flex-direction: column;
}
.plots {
    width: 400px;
}
plot {
    width: 100%;
}
</style>

