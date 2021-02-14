<template>
    <h1>cube</h1>
    <Cube/>
    <footer>
        <p>@dzshn 2021 | made with vue.js and three.js</p>
    </footer>
</template>

<script>
import Cube from "./components/Cube"

export default {
    name: 'App',
    components: {
        Cube
    }
}
</script>

<style>
body {
    margin: 0;
    background: #2a2a2a;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #fff;
    overflow: none;
}

footer {
    position: absolute;
    bottom: 0;
    width: 100%;
    color: #fff;
    background: #222;
}
</style>