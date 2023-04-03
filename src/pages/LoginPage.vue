<template>
    <div class="login-grid">
        <div class="login-form">
            <my-text-input class="login-form__login-input"
                        placeholder="Имя пользователя"
                        v-model:modelValue="login"
                        />

            <my-text-input class="login-form__password-input"
                        placeholder="Пароль"
                        v-model:modelValue="password"
            />

            <my-select  class="login-form__role-select"
                        :placeholderText="'Роль'"
            ></my-select>
            <my-btn class="login-form__btn"
                    @click="checkLogin"
            > {{ loginText }} </my-btn>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'login-page',
    data() {
        return {
            placeholderRoleSelect: 'Роль',
            loginText: 'Вход',
            login: '',
            password: '',
        }
    },
    methods: {
        checkLogin() {
        //     axios.get('https://api.mrsmori.moe/login', {'headers' : {'content-type': 'application/javascript'},
        //     'params': { login: this.login, password: this.password },
        // })
        //         .then((response) => {
        //             if (response.data.is_error === false)
        //             {
                        this.$router.push('/')
        //             }
        //             // console.log(response.data)
        //         })
        //         .then((error) => {
        //             console.log(error)
        //         })
        }
    }
}
</script>

<style lang="scss" scoped>
.login-grid {
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
}
.login-form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 0 auto;
    grid-row: 2;
    width: 20vw;

		&__login-input {
            margin: 0px 0px 0px 0px;
		}
        
		&__password-input {
            margin: 15px 0px 0px 0px;
		}

		&__role-select {
            margin: 15px 0px 0px 0px;
            width: 10vw;
		}
        
		&__btn {
            margin: 20px 0px 0px 0px;
            width: 10vw;
		}
}

</style>