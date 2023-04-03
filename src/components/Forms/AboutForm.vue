<template>
    <div class="info-texts">
        <p>Номер проекта {{ projectId }}</p>
        <p>Номер площади {{ areaId }} </p>
        <p>Номер пути {{ lineId }} </p>
        <p>Номер точки {{ pointId }} </p>
        <p>Результат точки A | T {{ restultPoint }}</p>
    </div>
</template>

<script>

export default {
    name: 'about-form',
    props: {
        projectId: String,
        areaId: String,
        lineId: String,
        pointId: String,
        restultPoint: String,
    }
}
</script>

<style lang="scss" scoped>
.info-texts > *{
    margin: 5px;
}
</style>