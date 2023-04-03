<template>
    <div class="delete-form__main"
    >
        <p>Удалить элемент?</p>
    
        <my-btn class="delete-form__btns_confirm"
                @click="deleteElement"
        >Подтвердить</my-btn>
        <my-btn class="delete-form__btns_cancel"
                @click="closeWindow"
        >Отменить</my-btn>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'delete-project-form',
    props: {
        elementId: String,
        link: String,
        typeElement: String
    },
    methods: {
        async deleteElement() {
            try {
                await axios.get(`${this.link}/${this.typeElement}_remove`,
                    {
                        headers: { 'content-type': 'application/javascript' },
                        params: {
                            id: this.elementId,
                        },
                        withCredentials: true
                    });
                this.closeWindow();
            } catch (e) {
                console.log(e)
            }
        },
        closeWindow() {
            this.$emit('update:visibility', false)
        }
    }
}
</script>

<style lang="scss" scoped>
.delete-form__main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    width: 100%;
    height: 100%;
}

.delete-form__btns {
    &_confirm {
        margin: 20px 0 10px 0;
        width: 50%;
    }
    
    &_cancel {
        width: 50%;
    }
}


</style>