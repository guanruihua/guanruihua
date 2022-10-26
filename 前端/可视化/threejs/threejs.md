# Three

> <https://threejs.org/docs/>
>

## React 使用

```js
import React from 'react'
import * as THREE from 'three'

class Index extends React.Component {
  mdom: any = React.createRef()
  componentDidMount() {
    const scene = new THREE.Scene()
    const camera = new THREE.PerspectiveCamera(
      50,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    )

    const renderer = new THREE.WebGLRenderer()
    renderer.setSize(window.innerWidth-100, window.innerHeight-100)
    this.mdom.current.appendChild(renderer.domElement)

    // document.body.appendChild(renderer.domElement)

    const geometry = new THREE.BoxGeometry()
    const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 })
    const cube = new THREE.Mesh(geometry, material)
    scene.add(cube)

    camera.position.z = 5

    function animate() {
      requestAnimationFrame(animate)

      cube.rotation.x += 0.01
      cube.rotation.y += 0.01

      renderer.render(scene, camera)
    }

    animate()
  }

  render(): React.ReactNode {
    // return <></>
    return <div ref={this.mdom} id='WebGL-output'></div>
  }
}

export default Index

```
