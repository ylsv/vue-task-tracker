<template>
    <header>
        <h1>{{title}}</h1>
        <Button v-show="isHomePage" :text="showAddTask ? 'Close' : 'Add Task' " :color="showAddTask ? 'red' : 'green'" @btn-click="$emit('toggle-add-task')" />
    </header>
</template>

<script>
import Button from './Button.vue'

export default {
    name: 'Header',
    props: {
        title: String,
        showAddTask: Boolean,
    },
    components: {
        Button
    },
    computed: {
        isHomePage() {
           return this.$route.path === '/'
        },
    },
    emits: ['toggle-add-task']
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>