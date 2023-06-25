<template>
    <student v-for="(student, index) in students" :key="index" :student="student" />
</template>

<script>
import Student from './Student.vue'
export default {
    props: ['students'],
    components: {
        Student
    }
}
</script>
<style scoped>

</style>