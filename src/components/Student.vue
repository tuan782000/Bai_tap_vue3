<template>
    <div
        @click="student.selected = !student.selected"
        :class="{ selected: student.selected }"
    >
        {{student.name}}
    </div>
</template>

<script>
export default {
    props: ["student"],
};
</script>
<style scoped>
div {
    border: 1px solid #ebebeb;
    margin: 4px;
    text-align: center;
}
.selected {
    background-color: greenyellow;
}
</style>
