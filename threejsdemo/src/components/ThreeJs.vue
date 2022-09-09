<template>
    <div class="three" id="three" ref="three">

    </div>
  </template>
  
  <script>
    import * as THREE from "three"
    // scene, mesh 不做代理，否则会报错
    const SCENE = new THREE.Scene()
    let mesh = null

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
            this.camera.position.set( 200, 200, 200)
            this.add(this.camera)
            // 不能看向scene否则会导致scene与几何体的位置发生重叠
            // this.camera.lookAt(SCENE.position);

            this.addBox()

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
        // 创建网格（几何图形和材质的结合体）
        addBox() {
            const geometry = new THREE.BoxGeometry( 100, 100, 100)
            const material = new THREE.MeshPhongMaterial({color:0xff0000})
            mesh = new THREE.Mesh(geometry,material)
            this.add(mesh)
            // 将相机指向盒子的位置，不会出现错误
            this.camera.lookAt(mesh.position);
        },
        // 增加光源
        addLight() {
            const light = new THREE.AmbientLight(0xffffff, 0.5)
            // 平行光源
            const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
            directionalLight.position.set(200, 600, 200);
            this.add(directionalLight);
            this.add(light)
        },
        // 渲染
        render(){
            this.renderer.render(SCENE, this.camera);
            mesh.rotation.x += 0.01;
            mesh.rotation.y += 0.01;
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
  