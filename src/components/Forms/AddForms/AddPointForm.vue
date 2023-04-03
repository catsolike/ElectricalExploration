<template>
    <div class="add-project__wrapper"
    >
        <my-text-input placeholder="Введите название проекта"
                        v-model="projectName"
        >
        </my-text-input>
    
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
    name: 'add-point-form',
    data() {
        return {
            projectName: null,
        }
    },
    props: {
        link: String,
        visibility: Boolean
    },
    methods: {
        async sendProject() {
            if (this.projectName !== null && this.projectName !== '' && this.projectName.indexOf(' ') < 0 && this.projectName.length < 20) {
                try {
                    await axios.get(`${this.link}/project_add`,
                        {
                            headers: { 'content-type': 'application/javascript' },
                            params: {
                                name: this.projectName,
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
.add-project__wrapper{
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