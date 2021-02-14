<template>
    <div>
        <div id="renderWindow"></div>
    </div>
    <p>{{hoveredFace}}, {{hoveredFace != 'none' ? cubeGroup.children[hoveredFace].position : 0}}</p>
</template>

<script>
import * as THREE from 'three'

export default {
    data() {
        return {
            scene: Object,
            hoveredFace: 'none',
            cubeGroup: Object,
            isMouseDown: false
        }
    },
    mounted() {
        const renderWindow = document.getElementById('renderWindow')
        const camera = new THREE.PerspectiveCamera(80, renderWindow.clientWidth / renderWindow.clientHeight, 0.1, 100)
        const renderer = new THREE.WebGLRenderer({alpha: true})
        const raycaster = new THREE.Raycaster()
        const geometry = new THREE.BoxGeometry()
        const scene = new THREE.Scene()
        const cubeGroup = new THREE.Group()
        const mousePos = new THREE.Vector2(-10, -10)
        let isDragging = false

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

                    cube.position.x = 1 - ix * 1
                    cube.position.y = 1 - iy * 1
                    cube.position.z = 1 - iz * 1
                    cubeGroup.add(cube)
                }
            }
        }

        cubeGroup.rotation.x = Math.PI / 8
        cubeGroup.rotation.y = Math.PI / 4

        scene.add(cubeGroup)
        renderWindow.addEventListener(
            "mousemove", event => {
                mousePos.x =   (event.x - renderWindow.getBoundingClientRect().x) / renderWindow.clientWidth   * 2 - 1
                mousePos.y = -((event.y - renderWindow.getBoundingClientRect().y) / renderWindow.clientHeight) * 2 + 1
            }
        )

        function rotateFace(facePositions, direction) {
            const pieceMovementOffset = [6, 2, -2, 4, 0, -4, 2, -2, -6]
            const objByOldPos = facePositions.map(
                pos => {
                    return cubeGroup.children.filter(
                        cube => {
                            return cube.position.equals(
                                new THREE.Vector3(pos[0], pos[1], pos[2])
                            )
                        }
                    )[0]
                }
            )
            const newPositions = pieceMovementOffset.map(
                (offset, index) => {
                    return facePositions[direction? 8-(index+offset) : index+offset]
                }
            )

            objByOldPos.forEach(
                (cube, index) => {
                    let newPos = newPositions[index]
                    cube.position.set(newPos[0], newPos[1], newPos[2])
                }
            )
        }

        renderWindow.addEventListener(
            "mousedown", event => {
                switch (this.hoveredFace) {
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

                    case 22:
                        rotateFace(
                            [[-1,  1, -1], [-1,  1, 0], [-1,  1, 1],
                             [-1,  0, -1], [-1,  0, 0], [-1,  0, 1],
                             [-1, -1, -1], [-1, -1, 0], [-1, -1, 1]],
                            event.button == 0
                        )
                        break

                    default:
                        isDragging = true
                        break
                }
            }
        )

        renderWindow.addEventListener("mouseup", () => {isDragging = false})
        renderWindow.addEventListener("mouseleave", () => {isDragging = false})

        renderWindow.addEventListener(
            "mousemove", event => {
                if (isDragging) {
                    cubeGroup.rotation.y += event.movementX / 500
                    cubeGroup.rotation.x += event.movementY / 500
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
            this.hoveredFace = intersect.length > 0 ? cubeGroup.children.indexOf(intersect[0].object) : 'none'

            renderer.render(scene, camera)
        }

        anim()

        this.cubeGroup = cubeGroup
        this.scene = scene
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