<template>
    <div id="renderWindow"></div>
</template>

<script>
import * as THREE from 'three'

export default {
    mounted() {
        const renderWindow = document.getElementById('renderWindow')
        const camera = new THREE.PerspectiveCamera(80, renderWindow.clientWidth / renderWindow.clientHeight, 1, 10)
        const renderer = new THREE.WebGLRenderer({alpha: true})
        const raycaster = new THREE.Raycaster()
        const geometry = new THREE.BoxGeometry()
        const scene = new THREE.Scene()
        const cubeGroup = new THREE.Group()
        const mousePos = new THREE.Vector2(-10, -10)
        let isDragging, hoveredObj

        renderer.setSize(renderWindow.clientWidth, renderWindow.clientHeight)
        renderWindow.appendChild(renderer.domElement)

        camera.position.z = 6

        for (let ix = 0; ix < 3; ix++) {
            for (let iy = 0; iy < 3; iy++) {
                for (let iz = 0; iz < 3; iz++) {
                    let cube = new THREE.Mesh(
                        geometry, new THREE.MeshBasicMaterial(
                            {color: 0x550000 * (ix+1) + 0x5500 * (iy+1) + 0x55 * (iz+1) + 0xfed3ea}
                        )
                    )

                    cube.position.set(1 - ix, 1 - iy, 1 - iz)
                    cubeGroup.add(cube)
                }
            }
        }

        cubeGroup.rotation.x = Math.PI / 8
        cubeGroup.rotation.y = Math.PI / 4

        scene.add(cubeGroup)
        renderWindow.addEventListener(
            "mousemove", event => {
                mousePos.x = event.x / renderWindow.clientWidth  *  2 - 1
                mousePos.y = event.y / renderWindow.clientHeight * -2 + 1
            }
        )

        function rotateFace(facePositions, direction) {
            const pieceMovementOffset = [6, 2, -2, 4, 0, -4, 2, -2, -6]
            const newPositions = pieceMovementOffset.map(
                (offset, index) => facePositions[direction? 8-(index+offset) : index+offset]
            )
            facePositions.map(
                pos => cubeGroup.children.filter(
                    cube => cube.position.equals(new THREE.Vector3().fromArray(pos))
                )[0]
            ).forEach(
                (cube, index) => {cube.position.fromArray(newPositions[index])}
            )
        }

        renderWindow.addEventListener(
            "mousedown", event => {
                switch (cubeGroup.children.indexOf(hoveredObj)) {
                    case 4:
                        rotateFace(
                            [[1,  1, 1], [1,  1, 0], [1,  1, -1],
                             [1,  0, 1], [1,  0, 0], [1,  0, -1],
                             [1, -1, 1], [1, -1, 0], [1, -1, -1]],
                            event.button == 0
                        )
                        break

                    case 10:
                        rotateFace(
                            [[ 1, 1, -1], [ 1, 1, 0], [ 1, 1, 1],
                             [ 0, 1, -1], [ 0, 1, 0], [ 0, 1, 1],
                             [-1, 1, -1], [-1, 1, 0], [-1, 1, 1]],
                            event.button == 0
                        )
                        break

                    case 12:
                        rotateFace(
                            [[-1,  1, 1], [0,  1, 1], [1,  1, 1],
                             [-1,  0, 1], [0,  0, 1], [1,  0, 1],
                             [-1, -1, 1], [0, -1, 1], [1, -1, 1]],
                            event.button == 0
                        )
                        break

                    case 14:
                        rotateFace(
                            [[1,  1, -1], [0,  1, -1], [-1,  1, -1],
                             [1,  0, -1], [0,  0, -1], [-1,  0, -1],
                             [1, -1, -1], [0, -1, -1], [-1, -1, -1]],
                            event.button == 0
                        )
                        break

                    case 22:
                        rotateFace(
                            [[-1, 1,  1], [-1, 0,  1], [-1, -1,  1],
                             [-1, 1,  0], [-1, 0,  0], [-1, -1,  0],
                             [-1, 1, -1], [-1, 0, -1], [-1, -1, -1]],
                            event.button == 0
                        )
                        break

                    case 16:
                        rotateFace(
                            [[-1, -1,  1], [0, -1,  1], [1, -1,  1],
                             [-1, -1,  0], [0, -1,  0], [1, -1,  0],
                             [-1, -1, -1], [0, -1, -1], [1, -1, -1]],
                            event.button == 0
                        )
                        break

                    default: isDragging = true
                }
            }
        )

        renderWindow.addEventListener("mouseup", () => {isDragging = false})
        renderWindow.addEventListener("mouseleave", () => {isDragging = false})

        renderWindow.addEventListener(
            "mousemove", event => {
                if (isDragging) {
                    cubeGroup.rotation.y += event.movementX / 300
                    cubeGroup.rotation.x += event.movementY / 300
                }
            }
        )

        renderWindow.addEventListener(
            "contextmenu", event => {
                event.preventDefault()
                event.stopPropagation()
                return false
            }
        )

        let anim = () => {
            requestAnimationFrame(anim)

            raycaster.setFromCamera(mousePos, camera)
            let intersect = raycaster.intersectObjects(cubeGroup.children)
            hoveredObj = intersect.length > 0 ? intersect[0].object : undefined

            renderer.render(scene, camera)
        }

        anim()
    }
}
</script>

<style scoped>
#renderWindow {
    position: absolute;
    top: 0; bottom: 0; left: 0; right: 0;
    width:  100%;
    height: 100%;
}
</style>