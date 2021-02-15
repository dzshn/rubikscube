<template>
    <h1>cube</h1>
    <div>
        <h3>how do I use this?</h3>
        <p>Left-click the middle pieces to rotate clockwise, Right-click to rotate counterclockwise, drag anywhere else to rotate the view</p>
        <h3>why.</h3>
        <p>good question</p>
    </div>
    <Cube/>
    <footer>
        <p>
            <a href="https://github.com/dzshn/rubikscube/blob/main/LICENSE" target="_blank" rel="noopener noreferrer">MIT License</a> ─ <a href="https://github.com/dzshn" target="_blank" rel="noopener noreferrer">dzshn</a> | made with <a href="https://vuejs.org" target="_blank" rel="noopener noreferrer">vue.js</a> and <a href="https://threejs.org" target="_blank" rel="noopener noreferrer">three.js</a>
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

h1, h3, a {
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

#app > div {
    position: absolute;
    width: 230px;
    right: 10px;
    top: 10px;
}

</style>