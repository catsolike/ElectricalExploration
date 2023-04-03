<template>
    <div class="my-table__wrapper">
        <table class="table-main">
            <tr><th>ID</th><th>T</th><th>A</th></tr>
            <tr v-for="item in items"
                :key="item.id"
            >
                <td> {{ item.id }} </td>
                <td> {{ item.x }} </td>
                <td> {{ item.y }} </td>
            </tr>
        </table>
    </div>


</template>

<script>

export default {
    name: "my-table",
    props: {
        items: Array,
    }
}
</script>

<style lang="scss" scoped>
$header-height: $base-font-size;
.my-table {

    &__wrapper {
        background: $table-background;
        height: 100%;
    }
}

.table-main {
    border-collapse: collapse;
    width: 100%;
    text-align: center;
}

th {
    position: sticky;
    top: 0;
    background: $th-background;
    padding: 0px 8px;
    height: $header-height;
    border-right: $main-background solid 5px;
}

th:last-child {
    border-right: $main-background solid 0px;
}

td {
    color: white;
    padding: 9px 8px;
    transition: .1s linear;
    border-right: $main-background solid 5px;
}

td:last-child {
    border-right: $main-background solid 0px;
}

tr:hover td {
    color: $table-title-border-line;
    background: $table-string-background-hover;
}

</style>
