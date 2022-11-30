<!--  -->
<template>
  <div class="sunEarth" id="sunEarth" ref="sunEarth">

  </div>
</template>

<script>
import * as THREE from "three"
const SCENE = new THREE.Scene()
let spheres = []
export default {
  components: {},
  data() {
    return {
      height: null,
      width: null,
      camera: null,
      renderer: null
    }
  },
  computed: {

  },
  watch: {},
  methods: {
    init() {
      // 创建太阳系系统
      const solarSystem = new THREE.Object3D();
      SCENE.add(solarSystem);
      spheres.push(solarSystem);

      let container = this.$refs.sunEarth;
      // 设置页面的宽高
      this.height = container.offsetHeight;
      this.width = container.offsetWidth;

      const radius = 1
      const widthSegments = 100
      const heightSegments = 100
      const SphereGeometry = new THREE.SphereGeometry(radius, widthSegments, heightSegments)
      const sunMaterial = new THREE.MeshPhongMaterial({ emissive: 0xffff00 })
      const sunMesh = new THREE.Mesh(SphereGeometry, sunMaterial)
      sunMesh.scale.set(5, 5, 5); // 扩大太阳的大小
      solarSystem.add(sunMesh);
      spheres.push(sunMesh)

      const earthMaterial = new THREE.MeshPhongMaterial({
        color: 0x2233ff,
        emissive: 0x112244,
      });
      // 创建地月系统
      const earthSystem = new THREE.Object3D();
      earthSystem.position.x = 20
      spheres.push(earthSystem);
      solarSystem.add(earthSystem)
      // 创建月球系统，将月球系统放入地月系统中
      const moonSystem = new THREE.Object3D();
      earthSystem.add(moonSystem)
      moonSystem.position.x = 2
      const moonMaterial = new THREE.MeshPhongMaterial({ color: 0x888888, emissive: 0x222222 })
      const moonMesh = new THREE.Mesh(SphereGeometry, moonMaterial);
      moonMesh.scale.set(.5, .5, .5)
      moonSystem.add(moonMesh);
      spheres.push(moonMesh);

      const earthMesh = new THREE.Mesh(SphereGeometry, earthMaterial);
      earthSystem.add(earthMesh)

      spheres.push(earthMesh);

      {
        const color = 0xffffff;
        const intensity = 3;
        // 中心添加点光源
        const light = new THREE.PointLight(color, intensity);
        SCENE.add(light);
      }
      const camera = new THREE.PerspectiveCamera(75, this.width / this.height, 0.1, 1000);
      this.camera = camera;
      camera.position.set(0, 50, 0);
      // camera.up.set(0, 0, 1);
      // camera.getWorldPosition.z = -50
      camera.lookAt(0, 0, 0);
      // 设置渲染器的属性
      this.renderer = new THREE.WebGLRenderer({
        antialias: true
      })

      // this.renderer.setClearColor(new THREE.Color('#000000'), 1);
      // 设置渲染页面的颗粒度，设置成和页面相同的像素。
      console.log(this.width, this.height)
      this.renderer.setSize(this.width, this.height);
      console.log(document.getElementById("sunEarth"))

      container.appendChild(this.renderer.domElement);
      // 辅助轴创建方法
      // spheres.forEach((node) => {
      //   // 创建辅助轴x (红色) 和 z (蓝色) y (绿色)
      //   const axes = new THREE.AxesHelper();
      //   axes.material.depthTest = false;
      //   axes.renderOrder = 1;
      //   node.add(axes);
      // });
      this.render()
    },
    // 渲染
    render() {
      this.renderer.render(SCENE, this.camera);
      // cubes.forEach((cube) => {
      // 	cube.rotation.x += 0.01
      // 	cube.rotation.y += 0.01
      // })
      spheres.forEach((obj) => {
        obj.rotation.y += 0.01;
      });
      requestAnimationFrame(this.render);
    }
  },
  created() {
  },
  mounted() {
    this.$nextTick(this.init())
  },
}
</script>
<style lang="scss" scoped>
.sunEarth {
  width: 100vw;
  height: 100vh;
}
</style>