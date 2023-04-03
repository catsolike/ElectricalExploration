<template>
    <div class="first-grid">
        <div class="selects">
            <my-select class="selects__project"
                        :placeholder-text="'Выбрать проект...'"
                        v-model="selectedProjectId"
                        :options="projects"
            ></my-select>
            <my-select class="selects__area"
                        :placeholder-text="'Выбрать площадь...'"
                        v-model="selectedAreaId"
                        :options="areas"
            ></my-select>
            <my-select class="selects__line"
                        :placeholder-text="'Выбрать маршрут...'"
                        v-model="selectedLineId"
                        :options="lines"
            ></my-select>
        </div>

        <div class="status-form"
            v-if="this.analyticRole"
        >
            <p class="status-form__status-text">Статус: {{ statusData }}</p>
            <my-btn class="status-form__confirm-btn">Подтвердить</my-btn>
            <my-btn class="status-form__cancel-btn">Отменить</my-btn>
        </div>

        <div class="math-form"
            v-if="this.mathRole"
        >
            <my-btn class="math-form__get-max-btn"
                    @click="openMax"
            >Получить высшее значение </my-btn>
            <my-btn class="math-form__get-average-a-btn">Получить среднее значение A</my-btn>
            <my-btn class="math-form__get-average-route-btn">Получить среднее значение по всему маршруту</my-btn>
            
            <form action="graphic" target="_blank">
                <my-btn class="math-form__open-graphic-btn"
                >Вывести график по точке</my-btn>
            </form>
        </div>

        <div class="table-wrapper">
            <my-table class="table-wrapper__table"
                        :items="points"
            ></my-table>
        </div>


        <my-dialog v-model:show="dialogVisible"
        >
            <p>Высшее значение: {{ getMax() }}</p>
        </my-dialog>
    </div>
</template>

<script>
import axios from 'axios';
import MyTable from '@/components/MyTable.vue';

export default {
    name: 'analytic-math-page',
    data() {
        return {
            selectedProjectId: null,
            selectedAreaId: null,
            selectedLineId: null,
            projects: [],
            areas: [ {name:'Сначала выберите проект'} ],
            lines: [{ name: 'Сначала выберите площадь' }],
            points: [],

            analyticRole: false,
            mathRole: true,

            uncheckedStatus: 'не проверен',
            checkedStatus: 'проверен',
            statusData: null,

            dialogVisible: false,
        }
    },
    props: {
        serverLink: String
    },
    components: {
        MyTable
    },
    mounted() {
        this.fetchProjects()
    },
    watch: {
        selectedProjectId() {
            this.fetchAreas(this.selectedProjectId)
        },
        selectedAreaId() {
            this.fetchLines(this.selectedAreaId)
        },
        selectedLineId() {
            this.fetchPoints(this.selectedLineId)
        },
    },
    methods: {
        async fetchProjects()
        {
            try {
                const response = await axios.get(`${this.serverLink}/projects`,
                    { headers: { 'content-type': 'application/javascript' } })
                    this.projects = response.data;
            } catch (e) {
                alert(e)
            }
        },
        async fetchAreas(projectId) {
            try {
                const response = await axios.get(`${this.serverLink}/areas`,
                    {
                        headers: { 'content-type': 'application/javascript' },
                        params: {
                            project_id: projectId,
                        }
                    })
                    this.areas = response.data;
            } catch (e) {
                console.log(e)
            }
        },
        async fetchLines(areaId) {
            try {
                const response = await axios.get(`${this.serverLink}/lines`,
                    {
                        headers: { 'content-type': 'application/javascript' },
                        params: {
                            area_id: areaId,
                        }
                    })
                    this.lines = response.data;
            } catch (e) {
                console.log(e)
            }
        },
        async fetchPoints(lineId) {
            try {
                const response = await axios.get(`${this.serverLink}/points`,
                    {
                        headers: { 'content-type': 'application/javascript' },
                        params: {
                            line_id: lineId,
                        }
                    })
                this.points = response.data;
            } catch (e) {
                console.log(e)
            }
        },
        openMax() {
            this.dialogVisible = true;
        },
        getMax() {
            if (this.points.length > 0)
                return Math.max(...this.points.map((point) => point.x))
            else return 'маршрут не выбран'
        },
        getAverageA() {
            
        },
        getAverageLine() {
            
        },
    }
}
</script>

<style lang="scss" scoped>
$table-width: calc(100% - 3vw);
.first-grid {
    display: inline-grid;
    grid-template-columns: 30vw 70vw;
    grid-template-rows: 1fr 1fr;

    gap: 1vh 1vw;
    padding: 1vh 1vw 1vh 1vw;
}

.selects {
    grid-column: 1;
    grid-row: 1;
		&__project {
		}

		&__area {
            margin: 20px 0px 0px 0px;
		}
        
		&__line {
            margin: 20px 0px 0px 0px;
        }
}

.status-form {
    grid-column: 1;
    grid-row: 2;
    display: flex;
    flex-direction: column;
		&__status-text {
		}

		&__confirm-btn {
            width: 50%;
            margin: 20px auto;
		}
        
		&__cancel-btn {
            margin: 0 auto;
            width: 50%;
		}
}

.math-form {
    grid-column: 1;
    grid-row: 2;
    display: flex;
    flex-direction: column;

    &__get-max-btn {
        width: 100%;
        margin: 0px auto;
    }

    &__get-average-a-btn {
        width: 100%;
        margin: 20px auto;
    }

    &__get-average-route-btn {
        width: 100%;
        margin: 0px auto;
    }

    &__open-graphic-btn {
        width: 100%;
        margin: 20px auto;
    }
}


.table-wrapper {
    grid-column: 2;
    grid-row-start: 1;
    grid-row-end: 3;
    overflow: auto;
    
    border-radius: 30px;

    height: 100%;
    width: $table-width;

		&__table {
            border-radius: 30px;
		}
}

</style>