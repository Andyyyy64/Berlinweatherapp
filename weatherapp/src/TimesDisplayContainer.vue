<script>
export default {
    props: {
        Data:[]
    }
}
</script>

<template>
    <div>
        <ul v-for="(List, index) in Data" :key="index">
            <li>
                時間:{{ List.time }} 温度:{{ List.temp }} {{ List.temp >= 15 ? "熱い" : "寒い" }}
            </li>
        </ul>
    </div>
</template>