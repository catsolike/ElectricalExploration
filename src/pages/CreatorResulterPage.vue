<template>
    <div class="second-grid">
        <div class="project_creator">
            <my-select class="project_creator__select"
                        :placeholder-text="'Выбрать проект...'"
                        v-model="selectedProjectId"
                        :options="projects"
                        ></my-select>

            <my-btn class="project_creator__add-btn"
                        v-if="this.creatorRole"
                        @click="this.openAddProject"
                        >Добавить проект</my-btn>

            <my-btn class="project_creator__delete-btn"
                        v-model="selectedProjectId"
                        v-if="this.creatorRole"
                        @click="this.openDeleteProject"
                        >Удалить проект</my-btn>
        </div>

        <div class="area_creator">
            <my-select  class="area_creator__select"
                        :placeholder-text="'Выбрать площадь...'"
                        :options="areas"
                        v-model="selectedAreaId"
            ></my-select>
            <my-btn class="area_creator__add-btn"
                        v-model="selectedAreaId"
                        v-if="this.creatorRole"
                        @click="this.openAddArea"
                        >Добавить площадь</my-btn>
            <my-btn class="area_creator__delete-btn"
                        v-model="selectedAreaId"
                        v-if="this.creatorRole"
                        @click="this.openDeleteArea"
                        >Удалить площадь</my-btn>
        </div>

        <div class="line_creator">
            <my-select  class="line_creator__select"
                        :placeholder-text="'Выбрать путь...'"
                        :options="lines"
                        v-model="selectedLineId"
            ></my-select>
            <my-btn class="line_creator__add-btn"
                    v-model="selectedLineId"
                    v-if="this.creatorRole"
                    @click="this.openAddLine"
                    >Добавить путь</my-btn>
            <my-btn class="line_creator__delete-btn"
                    v-model="selectedLineId"
                    v-if="this.creatorRole"
                        @click="this.openDeleteLine"
            >Удалить путь</my-btn>
        </div>

        <div class="point_creator">
            <my-select  class="point_creator__select"
                        :placeholder-text="'Выбрать точку...'"
                        v-model="selectedPointId"
                        :options="points"
            ></my-select>
            <my-btn class="point_creator__add-btn"
                        v-model="selectedPointId"
                        v-if="this.creatorRole"
                        @click="this.openAddPoint"
                        >Добавить точку</my-btn>
            <my-btn class="point_creator__delete-btn"
                        v-model="selectedPointId"
                        v-if="this.creatorRole"
                        @click="this.openDeletePoint"
                        >Удалить точку</my-btn>
        </div>

        <about-form :class="infoDataGrid"
                    :projectId="selectedProjectId"
                    :areaId="selectedAreaId"
                    :lineId="selectedLineId"
                    :pointId="selectedPointId"
        ></about-form>

        <div class="table-wrapper">
            <my-table   class="table-wrapper__table"
                        :items="points"
            ></my-table>
        </div>


        <my-dialog class="project__dialog"
                v-model:show="projectAdding"
        >
            <add-project-form :link="serverLink"
                                v-model:visibility="projectAdding"
                ></add-project-form>
        </my-dialog>

        <my-dialog class="project__dialog"
                v-model:show="projectDeleting"
        >
            <delete-form :link="serverLink"
                         :elementId="selectedProjectId"
                         :typeElement="'project'"
                         v-model:visibility="projectDeleting"
            ></delete-form>
        </my-dialog>

        <my-dialog class="area__dialog"
                v-model:show="areaAdding"
        >
            <add-area-form :link="serverLink"
                            :projects="projects"
                            v-model:visibility="areaAdding"
            ></add-area-form>
        </my-dialog>
        
        <my-dialog class="area__dialog"
                v-model:show="areaDeleting"
        >
            <delete-form :link="serverLink"
                         :elementId="selectedAreaId"
                         :typeElement="'area'"
                         v-model:visibility="areaDeleting"
            />
        </my-dialog>

        <my-dialog class="line__dialog"
                v-model:show="lineAdding"
        >
        </my-dialog>
        <my-dialog class="line__dialog"
                v-model:show="lineDeleting"
        >
            <delete-form :link="serverLink"
                         :elementId="selectedLineId"
                         :typeElement="'line'"
                         v-model:visibility="lineDeleting"
            />
        </my-dialog>

        <my-dialog class="point__dialog"
                v-model:show="pointAdding"
        >
        </my-dialog>
        <my-dialog class="point__dialog"
                v-model:show="pointDeleting"
        >
            <delete-form :link="serverLink"
                         :elementId="selectedPointId"
                         :typeElement="'point'"
                         v-model:visibility="pointDeleting"
            />
        </my-dialog>
    </div>
</template>

<script>
import axios from 'axios';
import MyTable from '@/components/MyTable.vue';
import AboutForm from '@/components/Forms/AboutForm.vue';

import DeleteForm from '@/components/Forms/DeleteForm.vue';
import AddProjectForm from "@/components/Forms/AddForms/AddProjectForm"
import AddAreaForm from "@/components/Forms/AddForms/AddAreaForm"

export default {
    name: 'creator-resulter-page',
    components: {
        MyTable,
        AboutForm,
        DeleteForm,
        AddProjectForm,
        AddAreaForm,
    },
    data() {
        return {
            projectAdding: false,
            areaAdding: false,
            lineAdding: false,
            pointAdding: false,

            projectDeleting: false,
            areaDeleting: false,
            lineDeleting: false,
            pointDeleting: false,

            selectedProjectId: null,
            selectedAreaId: null,
            selectedLineId: null,
            selectedPointId: null,
            projects: [],
            areas: [ {name:'Сначала выберите проект'} ],
            lines: [ { name: 'Сначала выберите площадь' } ],
            points: [ { name: 'Сначала выберите путь' } ],

            creatorRole: true
        }
    },
    props: {
        serverLink: String
    },
    computed: {
        infoDataGrid() {
            return {
                'info-data_creator': this.creatorRole,
                'info-data_resulter': !this.creatorRole
            }
        },
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
        // selectedLineId() {
        //     this.fetchPoints(this.selectedLineId)
        // },
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
        openAddProject() {
            this.projectAdding = true
        },
        openAddArea() {
            this.areaAdding = true
        },
        openAddLine() {
            this.lineAdding = true
        },
        openAddPoint() {
            this.pointAdding = true
        },

        openDeleteProject() {
            if (this.selectedProjectId !== null) {
                this.projectDeleting = true;
            }
            else alert('Выберите проект')
        },
        openDeleteArea() {
            if (this.selectedAreaId !== null) {
                this.areaDeleting = true;
            }
            else alert('Выберите площадь')
        },
        openDeleteLine() {
            if (this.selectedLineId !== null) {
                this.lineDeleting = true;
            }
            else alert('Выберите путь')
        },
        openDeletePoint() {
            if (this.selectedPointId !== null) {
                this.pointDeleting = true;
            }
            else alert('Выберите точку')
        },
    }
}
</script>

<style lang="scss" scoped>
$table-width: calc(100% - 3vw);
.second-grid {
    display: inline-grid;
    grid-template-columns: 20vw 20vw 1fr;
    grid-template-rows: 1fr 1fr 3fr;

    gap: 1vh 1vw;
    padding: 1vh 1vw 1vh 1vw;
}

.project {

    &_creator {
        grid-column: 1;
        grid-row: 1;

        display: flex;
        flex-direction: column;

        &__select {
            margin: 0px 0px 10px 0px;
        }

        &__dialog {
            background: red;
            position: relative;
            z-index: 10;
        }
        
        &__add-btn {
            height: $base-ui-height;
            margin: 0px 0px 10px 0px;
        }
        
        &__delete-btn {
            height: $base-ui-height;
        }
    }
}

.area {

    &_creator {
        grid-column: 2;
        grid-row: 1;

        display: flex;
        flex-direction: column;

        &__select {
            margin: 0px 0px 10px 0px;
        }
        
        &__add-btn {
            height: $base-ui-height;
            margin: 0px 0px 10px 0px;
        }
        
        &__delete-btn {
            height: $base-ui-height;
        }
    }
}

.line {

    &_creator {
        grid-column: 1;
        grid-row: 2;
        display: flex;
        flex-direction: column;

        &__select {
            margin: 0px 0px 10px 0px;
        }
        
        &__add-btn {
            height: $base-ui-height;
            margin: 0px 0px 10px 0px;
        }
        
        &__delete-btn {
            height: $base-ui-height;
        }
    }
}

.point {

    &_creator {
        grid-column: 2;
        grid-row: 2;

        display: flex;
        flex-direction: column;

        &__select {
            margin: 0px 0px 10px 0px;
        }
        
        &__add-btn {
            height: $base-ui-height;
            margin: 0px 0px 10px 0px;
        }
        
        &__delete-btn {
            height: $base-ui-height;
        }
    }
}

.info-data_creator {
    grid-column: 3;
    grid-row-start: 1;
    grid-row-end: 2;
}
.info-data_resulter {
    grid-column-start: 1;
    grid-column-end: 3;
    grid-row: 3;
}

.table-wrapper {
    grid-column: 3;
    grid-row-start: 2;
    grid-row-end: 4;
    overflow: auto;
    
    border-radius: 30px;

    height: 100%;
    width: $table-width;

		&__table {
            border-radius: 30px;
		}
}

</style>