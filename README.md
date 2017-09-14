<!DOCTYPE html>
<html lang="en">
	<head>
		<title>three.js webgl - io blender - vertex colors</title>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
		<style>
			body {
				color: #eee;
				font-family:Monospace;
				font-size:13px;
				text-align:center;
				background-color: #000;
				margin: 0px;
				padding: 0px;
				overflow: hidden;
			}
			#info {
				position: absolute;
				top: 0px; width: 100%;
				padding: 5px;
			}
			a {
				color: #0080ff;
			}
		</style>
	</head>
	<body>

		<div id="container"></div>
		<div id="info"><a href="http://threejs.org" target="_blank" rel="noopener">three.js</a> webgl - io blender - vertex colors</div>

		<script src="../build/three.js"></script>

		<script src="js/Detector.js"></script>
		<script src="js/libs/stats.min.js"></script>

		<script>
			if ( ! Detector.webgl ) Detector.addGetWebGLMessage();
			var container, stats;
			var camera, scene, renderer;
			var mesh, mesh2, mesh3, light;
			var mouseX = 0, mouseY = 0;
			var windowHalfX = window.innerWidth / 2;
			var windowHalfY = window.innerHeight / 2;
			init();
			animate();
			function init() {
				container = document.getElementById( 'container' );
				camera = new THREE.PerspectiveCamera( 40, window.innerWidth / window.innerHeight, 1, 10000 );
				camera.position.z = 1800;
				scene = new THREE.Scene();
				light = new THREE.DirectionalLight( 0xffffff );
				light.position.set( 0, 0, 1 ).normalize();
				scene.add( light );
				var loader = new THREE.JSONLoader();
				loader.load( "obj/cubecolors/cubecolors.js", createScene1 );
				loader.load( "obj/cubecolors/cube_fvc.js", createScene2 );
				renderer = new THREE.WebGLRenderer( { antialias: true } );
				renderer.setPixelRatio( window.devicePixelRatio );
				renderer.setSize( window.innerWidth, window.innerHeight );
				container.appendChild( renderer.domElement );
				stats = new Stats();
				container.appendChild( stats.dom );
				document.addEventListener( 'mousemove', onDocumentMouseMove, false );
				//
				window.addEventListener( 'resize', onWindowResize, false );
			}
			function onWindowResize() {
				windowHalfX = window.innerWidth / 2;
				windowHalfY = window.innerHeight / 2;
				camera.aspect = window.innerWidth / window.innerHeight;
				camera.updateProjectionMatrix();
				renderer.setSize( window.innerWidth, window.innerHeight );
			}
			function createScene1( geometry, materials ) {
				mesh = new THREE.Mesh( geometry, materials );
				mesh.position.x = 400;
				mesh.scale.x = mesh.scale.y = mesh.scale.z = 250;
				scene.add( mesh );
			}
			function createScene2( geometry, materials ) {
				mesh2 = new THREE.Mesh( geometry, materials );
				mesh2.position.x = - 400;
				mesh2.scale.x = mesh2.scale.y = mesh2.scale.z = 250;
				scene.add( mesh2 );
			}
			function onDocumentMouseMove( event ) {
				mouseX = ( event.clientX - windowHalfX );
				mouseY = ( event.clientY - windowHalfY );
			}
			//
			function animate() {
				requestAnimationFrame( animate );
				render();
				stats.update();
			}
			function render() {
				camera.position.x += ( mouseX - camera.position.x ) * 0.05;
				camera.position.y += ( - mouseY - camera.position.y ) * 0.05;
				camera.lookAt( scene.position );
				if ( mesh ) {
					mesh.rotation.x += 0.01;
					mesh.rotation.y += 0.01;
				}
				if ( mesh2 ) {
					mesh2.rotation.x += 0.01;
					mesh2.rotation.y += 0.01;
				}
				renderer.render( scene, camera );
			}
		</script>

	</body>
</html>
