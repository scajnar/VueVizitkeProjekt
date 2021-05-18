<template>
<div id="vizitka"></div>
</template>



<script>
/* eslint-disable no-undef */
//import vizitka from '/3d.vue'
export default {
  name: 'App',
  mounted() {
    const backgroundColor = 0x000000;

    /*////////////////////////////////////////*/

    var renderCalls = [];
    function render () {
        requestAnimationFrame( render );
        renderCalls.forEach((callback)=>{ callback(); });
    }
    render();

    /*////////////////////////////////////////*/

    var scene = new THREE.Scene();

    var camera = new THREE.PerspectiveCamera( 80, window.innerWidth / window.innerHeight, 0.1, 800 );
    camera.position.set(5,5,5);

    var renderer = new THREE.WebGLRenderer( { antialias: true } );

    renderer.setPixelRatio( window.devicePixelRatio );
    //renderer.setPixelRatio(1.00);

    renderer.setSize( window.innerWidth, window.innerHeight );
    //renderer.setSize( 100, 100 );
    renderer.setClearColor( backgroundColor );//0x );

    renderer.toneMapping = THREE.LinearToneMapping;
    renderer.toneMappingExposure = Math.pow( 0.94, 5.0 );
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFShadowMap;

    renderer.outputEncoding = THREE.sRGBEncoding

    window.addEventListener( 'resize', function () {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize( window.innerWidth, window.innerHeight );
    }, false );

    document.body.appendChild( renderer.domElement);

    function renderScene(){ renderer.render( scene, camera ); }
    renderCalls.push(renderScene);

    /* ////////////////////////////////////////////////////////////////////////// */

    var controls = new THREE.OrbitControls( camera );

    controls.rotateSpeed = 0.3;
    controls.zoomSpeed = 0.9;

    controls.minDistance = 3;
    controls.maxDistance = 20;

    controls.minPolarAngle = 0; // radians
    controls.maxPolarAngle = Math.PI /2; // radians

    controls.enableDamping = true;
    controls.dampingFactor = 0.05;

    renderCalls.push(function(){
        controls.update()
    });


    /* ////////////////////////////////////////////////////////////////////////// */


    // var light = new THREE.PointLight( 0xffffcc, 20, 200 );
    // light.position.set( 4, 30, -20 );
    // scene.add( light );
  
    var light2 = new THREE.AmbientLight( 0xFFFFFF, 2, 100 );
    light2.position.set( 30, -10, 30 );
    scene.add( light2 );

    /* ////////////////////////////////////////////////////////////////////////// */



    var loader = new THREE.GLTFLoader();
    loader.crossOrigin = true;
    loader.load( 'final.gltf', function ( data ) {

        
        var object = data.scene;
            object.position.set(0, -10, -0.75);
    //     object.rotation.set(Math.PI / -2, 0, 0);

    //     TweenLite.from( object.rotation, 1.3, {
    //       y: Math.PI * 2,
    //       ease: 'Power3.easeOut'
    //     });

        //object.position.y = - 95;
        scene.add( object );
        //, onProgress, onError );
    });

    console.log('test')
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.spodnji{
  border-top:200px;
}
canvas { display: block; }
html { overflow: hidden; background: #000; }
</style>
