<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/logo.png">
        <v-timer :endtime="'10 Oct 2021 14:00:00'" />
    </div>
</template>

<script>
import VTimer from './components/VTimer';

export default {
    name: 'App',
    components: {
        VTimer
    }
}
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        font-size: 1.8rem;
        font-weight: 500;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
