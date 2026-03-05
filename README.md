
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>3D Car Game</title>

<style>
body{
margin:0;
overflow:hidden;
background:black;
font-family:Arial;
}

#score{
position:absolute;
top:20px;
left:20px;
color:white;
font-size:22px;
}

#gameOver{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
color:red;
font-size:40px;
display:none;
}
</style>
</head>

<body>

<div id="score">Score: 0</div>
<div id="gameOver">GAME OVER</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<script>

let scene = new THREE.Scene();

let camera = new THREE.PerspectiveCamera(
75,
window.innerWidth/window.innerHeight,
0.1,
1000
);

let renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth,window.innerHeight);
document.body.appendChild(renderer.domElement);

let light = new THREE.DirectionalLight(0xffffff,1);
light.position.set(0,10,10);
scene.add(light);

let roadGeometry = new THREE.PlaneGeometry(20,200);
let roadMaterial = new THREE.MeshStandardMaterial({color:0x333333});
let road = new THREE.Mesh(roadGeometry,roadMaterial);
road.rotation.x = -Math.PI/2;
scene.add(road);

let carGeometry = new THREE.BoxGeometry(2,1,4);
let carMaterial = new THREE.MeshStandardMaterial({color:0x00ff00});
let playerCar = new THREE.Mesh(carGeometry,carMaterial);
playerCar.position.y = 0.5;
playerCar.position.z = 5;
scene.add(playerCar);

let enemyCars = [];

function createEnemy(){

let enemyMaterial = new THREE.MeshStandardMaterial({color:0xff0000});
let enemy = new THREE.Mesh(carGeometry,enemyMaterial);

enemy.position.x = (Math.random()*10)-5;
enemy.position.y = 0.5;
enemy.position.z = -50;

scene.add(enemy);
enemyCars.push(enemy);

}

setInterval(createEnemy,1500);

camera.position.set(0,8,12);
camera.lookAt(playerCar.position);

let keys = {};
document.addEventListener("keydown",(e)=>keys[e.key]=true);
document.addEventListener("keyup",(e)=>keys[e.key]=false);

let score = 0;
let gameOver = false;

function animate(){

if(gameOver) return;

requestAnimationFrame(animate);

if(keys["ArrowLeft"]) playerCar.position.x -=0.2;
if(keys["ArrowRight"]) playerCar.position.x +=0.2;

enemyCars.forEach(enemy=>{

enemy.position.z +=0.5;

if(enemy.position.z > 10){

enemy.position.z = -50;
enemy.position.x = (Math.random()*10)-5;

score++;
document.getElementById("score").innerText="Score: "+score;

}

let dx = playerCar.position.x - enemy.position.x;
let dz = playerCar.position.z - enemy.position.z;

if(Math.abs(dx)<2 && Math.abs(dz)<3){

gameOver=true;
document.getElementById("gameOver").style.display="block";

}

});

renderer.render(scene,camera);

}

animate();

</script>

</body>
</html>
