<!--template>
  <div class="three-container" ref="container"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import type { GLTF } from 'three/examples/jsm/loaders/GLTFLoader';

const container = ref<HTMLElement | null>(null);
let scene: THREE.Scene;
let camera: THREE.PerspectiveCamera;
let renderer: THREE.WebGLRenderer;
let controls: OrbitControls;
let animationFrame: number;
let objects: THREE.Mesh[] = [];

// Configuration et initialisation de Three.js
const init = () => {
  if (!container.value) return;

  // Créer la scène
  scene = new THREE.Scene();

  // Configurer la caméra
  camera = new THREE.PerspectiveCamera(
    70,
    container.value.clientWidth / container.value.clientHeight,
    0.1,
    1000
  );
  camera.position.z = 5;

  // Configurer le renderer
  renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.setSize(container.value.clientWidth, container.value.clientHeight);
  container.value.appendChild(renderer.domElement);

  // Ajouter des lumières
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(10, 10, 10);
  scene.add(directionalLight);

  // Ajouter des objets 3D (exemple avec des cubes)
  addObjects();

  // Ajouter des contrôles de caméra (exemple)
  controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;

  // Charger un modèle 3D (exemple)
  const loader = new GLTFLoader();
  loader.load('/path/to/model.gltf', (gltf: GLTF) => {
    scene.add(gltf.scene);
  });

  // Gérer le redimensionnement de la fenêtre
  window.addEventListener('resize', onWindowResize);

  // Démarrer l'animation
  animate();
};

// Ajouter des objets 3D à la scène
const addObjects = () => {
  // Créer des cubes avec des couleurs aléatoires
  for (let i = 0; i < 50; i++) {
    const geometry = new THREE.BoxGeometry(0.5, 0.5, 0.5);
    const material = new THREE.MeshPhongMaterial({
      color: new THREE.Color(`hsl(${Math.random() * 360}, 100%, 50%)`),
    });

    const cube = new THREE.Mesh(geometry, material);

    // Position aléatoire
    cube.position.x = (Math.random() - 0.5) * 10;
    cube.position.y = (Math.random() - 0.5) * 10;
    cube.position.z = (Math.random() - 0.5) * 10;

    // Rotation aléatoire
    cube.rotation.x = Math.random() * Math.PI;
    cube.rotation.y = Math.random() * Math.PI;

    // Stocker la vitesse de rotation pour l'animation
    cube.userData = {
      rotationSpeed: {
        x: (Math.random() - 0.5) * 0.02,
        y: (Math.random() - 0.5) * 0.02,
      }
    };

    scene.add(cube);
    objects.push(cube);
  }
};

// Animer les objets 3D
const animate = () => {
  animationFrame = requestAnimationFrame(animate);

  // Animer chaque cube
  objects.forEach(cube => {
    cube.rotation.x += cube.userData.rotationSpeed.x;
    cube.rotation.y += cube.userData.rotationSpeed.y;
  });

  // Mettre à jour les contrôles
  if (controls) {
    controls.update();
  }

  // Rendre la scène
  renderer.render(scene, camera);
};

// Gérer le redimensionnement de la fenêtre
const onWindowResize = () => {
  if (!container.value) return;

  camera.aspect = container.value.clientWidth / container.value.clientHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(container.value.clientWidth, container.value.clientHeight);
};

// Hooks de cycle de vie
onMounted(() => {
  if (container.value) {
    init();
  }
});

onBeforeUnmount(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame);
  }
  window.removeEventListener('resize', onWindowResize);

  // Nettoyer les ressources Three.js
  objects.forEach(object => {
    if (object.geometry) object.geometry.dispose();
    if (object.material) {
      if (Array.isArray(object.material)) {
        object.material.forEach(material => material.dispose());
      } else {
        object.material.dispose();
      }
    }
  });

  if (renderer) {
    renderer.dispose();
  }
});
</script>

<style scoped>
.three-container {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  pointer-events: none;
}
</style-->
