<template>
  <object3d :obj="mesh"></object3d>
</template>

<script>
/* global requestAnimationFrame */
import THREE from 'three'
import Object3D from '../components/Object3D'

// http://threejs.org/examples/#webgl_geometry_dynamic
export default {
  mixins: [Object3D],

  created () {
    this.animate = this.animate.bind(this)
    this.clock = new THREE.Clock()
    this.mesh = this.createOcean()
  },

  ready () {
    this.animate()
  },

  methods: {
    createOcean () {
      const geometry = this.geometry = new THREE.PlaneGeometry(10000, 10000, 40, 40)
      geometry.rotateX(-Math.PI / 2)
      for (var i = 0, l = geometry.vertices.length; i < l; i++) {
        geometry.vertices[ i ].y = 10 * Math.sin(i / 2)
      }

      const texture = new THREE.TextureLoader().load(require('./water.jpg'))
      texture.wrapS = texture.wrapT = THREE.RepeatWrapping
      texture.repeat.set(5, 5)
      const material = new THREE.MeshBasicMaterial({ color: 0x0044ff, map: texture })

      const mesh = new THREE.Mesh(geometry, material)
      mesh.name = 'Ocean'
      return mesh
    },

    animate () {
      requestAnimationFrame(this.animate)
      const time = this.clock.getElapsedTime() * 5
      for (var i = 0, l = this.geometry.vertices.length; i < l; i++) {
        this.geometry.vertices[i].y = 10 * Math.sin(i / 5 + (time + i) / 7)
      }
      this.mesh.geometry.verticesNeedUpdate = true
    }
  }
}
</script>
