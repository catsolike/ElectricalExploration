<template>
    <analytic-math-page
                        v-if="this.role === this.analyticRole || this.role === this.mathRole"
                        :serverLink="serverLink"
    ></analytic-math-page>

    <creator-resulter-page
                            v-else-if="this.role === this.creatorRole || this.role === this.resulterRole "
                            :serverLink="serverLink"
    ></creator-resulter-page>

    <div v-else class="unlogin"
    >Войдите, чтобы использовать сайт</div>
</template>

<script>
import axios from 'axios';
import AnalyticMathPage from '@/pages/AnalyticMathPage.vue';
import CreatorResulterPage from '@/pages/CreatorResulterPage.vue'
export default {
    components: {
        AnalyticMathPage,
        CreatorResulterPage
    },
    data() {
        return {
            serverLink: "https://shikidy.mrsmori.moe",

            analyticRole: "analitic",
            mathRole: "math",
            creatorRole: "creator",
            resulterRole: "resulter",
            role: null
        }
    },
    mounted() {
        this.checkLRole()
    },
    methods: {
        async checkLRole() {
            try {
                const response = await axios.get(`${this.serverLink}/role`,
                    
                    {
                        headers: { 'content-type': 'application/javascript' },
                        withCredentials: true
                    })
                this.role = response.data.role;
            } catch (e) {
                alert(e)
            }
        }
    }
}
</script>

<style lang="scss" scoped>

</style>