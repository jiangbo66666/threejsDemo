<template>
    <div class="three" id="three" ref="three">

    </div>
  </template>
  
  <script>
    import * as THREE from "three"
    // scene, mesh，cubes 不做代理（不放在this中，不让vue监听），否则会报错
    const SCENE = new THREE.Scene()
    let cubes = []

  export default {
    name: 'threeDemo',
    props: {
        
    },
    data(){
        return{
            scene : null,
            camera : null,
            width : 0,
            height : 0,
            renderer : null,
            mesh : null
        }
    },
    mounted() {
        this.$nextTick(this.init())
    },
    methods: {
        init() {
            let container = this.$refs.three;
            // console.log(container)
            // 设置页面的宽高
            this.height = container.offsetHeight;
            this.width = container.offsetWidth;

            // threejs的几个要素，scene,camera,geometry,light等
            // 设置透视相机
            this.camera = new THREE.PerspectiveCamera( 75, this.width / this.height, 0.1, 1000)
            // 设置相机的位置
            this.camera.position.set( 50, 50, 50)
            this.add(this.camera)

            // 创建自定义的方形和材质
            const geometry = new THREE.BoxGeometry(10,10,10)
            cubes = [
                this.addCubeMesh( geometry, 0x44aa88, 20),
                this.addCubeMesh( geometry, 0x8844aa, 0),
                this.addCubeMesh( geometry, 0xaa8844, -20),
            ]
            // 指定相机看向的方位
            this.camera.lookAt(0,0,0);
            this.addLight()
            
            // 设置渲染器的属性
            this.renderer = new THREE.WebGLRenderer({
                antialias: true
            })
            this.renderer.setClearColor(new THREE.Color('#F5F5DC'), 1);
            this.renderer.setSize(this.width, this.height);
            console.log(document.getElementById("three"))
            
            container.appendChild(this.renderer.domElement);
            this.render();
        },
        add(obj) {
            SCENE.add(obj)
        },
        // 创建自定义方形网格
        addCubeMesh(geometry,color,x){
            const material = new THREE.MeshPhongMaterial({color})
            const cube = new THREE.Mesh(geometry,material)
            cube.position.x = x
            this.add(cube)
            return cube
        },
        // 增加光源
        addLight() {
            const color = 0xFFFFFF;
            const intensity = 1;
            // 平行光源
            const DirectLight = new THREE.DirectionalLight(color, intensity);
            DirectLight.position.set(-1, 2, 4);
            this.add(DirectLight);
        },
        // 渲染
        render(){
            this.renderer.render(SCENE, this.camera);
            cubes.forEach((cube)=>{
                cube.rotation.x += 0.01
                cube.rotation.y += 0.01
            })
            requestAnimationFrame(this.render);
        }

    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped lang="scss">
    .three{
        width: 100vw;
        height: 100vh;
    }
  </style>
  