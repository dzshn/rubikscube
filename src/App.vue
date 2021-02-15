<template>
    <h1>cube</h1>
    <Cube/>
    <footer>
        <p>
            <a href="https://github.com/dzshn/rubikscube/blob/master/LICENSE">MIT License</a> ─ <a href="https://github.com/dzshn">dzshn</a> | made with <a href="https://vuejs.org">vue.js</a> and <a href="https://threejs.org">three.js</a>
        </p>
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
    color: #aaa;
    overflow: none;
}

h1, a {
    color: #fff
}

a {
    text-decoration: none;
    background: #3a3a3a;
    padding: 3px 5px 1px;
    border-radius: 5px;
}

footer {
    position: absolute;
    bottom: 0;
    width: 100%;
    color: #fff;
    background: #222;
}
</style>