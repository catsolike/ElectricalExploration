<template>
    <div class="add-area__main"
    >
        <my-text-input placeholder="Введите название площади"
                        v-model="areaName"
        >
        </my-text-input>
    
        <my-select class="add-area__select__project"
                    :placeholder-text="'Выбрать проект...'"
                    v-model="selectedProjectId"
                    :options="projects"
        ></my-select>

        <my-btn class="add-project__btns_confirm"
                @click="sendProject"
        >Подтвердить</my-btn>
        <my-btn class="add-project__btns_cancel"
                @click="closeWindow"
        >Отменить</my-btn>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'add-area-form',
    data() {
        return {
            areaName: null,
            selectedProjectId: null,
        }
    },
    props: {
        link: String,
        visibility: Boolean,
        projects: Array,
    },
    methods: {
        async sendProject() {
            if (this.areaName !== null && this.areaName !== '' && this.areaName.indexOf(' ') < 0 && this.areaName.length < 20) {
                try {
                    await axios.get(`${this.link}/create_area`,
                        {
                            headers: { 'content-type': 'application/javascript' },
                            params: {
                                project_id: this.selectedProjectId,
                                name: this.areaName,
                            }
                        });
                    this.closeWindow();
                } catch (e) {
                    console.log(e)
                }
            }
        },
        closeWindow() {
            this.$emit('update:visibility', false)
        }
    }
}
</script>

<style lang="scss" scoped>
.add-area__main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    
    width: 100%;
    height: 100%;
}

.add-project__btns {
    &_confirm {
        margin: 20px 0 10px 0;
        width: 50%;
    }
    
    &_cancel {
        width: 50%;
    }
}


</style>