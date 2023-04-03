<template>
    <analytic-math-page
                        v-if="this.analyticRole || this.mathRole"
                        :serverLink="defaultLink"
    ></analytic-math-page>

    <creator-resulter-page
                            v-else-if="this.creatorRole || this.resulterRole"
                            :serverLink="defaultLink"
    ></creator-resulter-page>

    <div v-else class="unlogin"
    >Войдите, чтобы использовать сайт</div>
</template>

<script>
import AnalyticMathPage from '@/pages/AnalyticMathPage.vue';
import CreatorResulterPage from '@/pages/CreatorResulterPage.vue'
export default {
    components: {
        AnalyticMathPage,
        CreatorResulterPage
    },
    data() {
        return {
            defaultLink: "https://shikidy.mrsmori.moe",
            analyticRole: false,
            mathRole: true,
            creatorRole: false,
            resulterRole: false,
        }
    }
}
</script>

<style lang="scss" scoped>

</style>