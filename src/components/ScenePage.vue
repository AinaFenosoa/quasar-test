<template>
  <q-page class="login-page">
    <div class="model-container" ref="container"></div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import type { GLTF } from 'three/examples/jsm/loaders/GLTFLoader.js';

const container = ref<HTMLElement | null>(null);
let scene: THREE.Scene;
let camera: THREE.PerspectiveCamera;
let renderer: THREE.WebGLRenderer;
let controls: OrbitControls;
let animationFrame: number;
let model: THREE.Group | null = null;

// Configuration et initialisation de Three.js
const init = () => {
  if (!container.value) return;

  // Créer la scène
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0xfdaaaa);

  // Configurer la caméra
  camera = new THREE.PerspectiveCamera(
    50,
    container.value.clientWidth / container.value.clientHeight,
    0.1,
    1000,
  );
  camera.position.z = 5;
  camera.position.y = 1;

  // Configurer le renderer
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(container.value.clientWidth, container.value.clientHeight);

  // Utiliser outputColorSpace au lieu de outputEncoding (Three.js >=r152)
  renderer.outputColorSpace = THREE.SRGBColorSpace;

  container.value.appendChild(renderer.domElement);

  // Ajouter des lumières
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.7);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
  directionalLight.position.set(1, 1, 1);
  scene.add(directionalLight);

  // Ajouter des aides visuelles pour débogage
  const axesHelper = new THREE.AxesHelper(5);
  scene.add(axesHelper);

  const gridHelper = new THREE.GridHelper(10, 10);
  scene.add(gridHelper);

  // Ajouter des contrôles de caméra
  controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;
  controls.dampingFactor = 0.05;
  controls.minDistance = 3;
  controls.maxDistance = 10;
  controls.autoRotate = true;
  controls.autoRotateSpeed = 1;

  // Charger le modèle 3D
  const loader = new GLTFLoader();
  loader.load('voiture.glb',
    (gltf: GLTF) => {
      console.log('Modèle chargé avec succès!', gltf);
      model = gltf.scene;

      if (model) {
        // Centrer et ajuster le modèle
        const box = new THREE.Box3().setFromObject(model);
        const center = box.getCenter(new THREE.Vector3());
        const size = box.getSize(new THREE.Vector3());

        // Centrer le modèle
        model.position.x = -center.x;
        model.position.y = -center.y;
        model.position.z = -center.z;

        // Ajuster l'échelle si nécessaire
        const maxDim = Math.max(size.x, size.y, size.z);
        if (maxDim > 2) {
          const scale = 2 / maxDim;
          model.scale.set(scale, scale, scale);
        }

        scene.add(model);
      }
    },
    // Fonction de progression du chargement
    (xhr: { loaded: number; total: number }) => {
      console.log((xhr.loaded / xhr.total) * 100 + '% chargé');
    },
    // Fonction d'erreur - changer le type à unknown comme attendu par GLTFLoader
    (error: unknown) => {
      console.error("Une erreur s'est produite lors du chargement du modèle:", error);
    },
  );

  // Gérer le redimensionnement de la fenêtre
  window.addEventListener('resize', onWindowResize);

  // Démarrer l'animation
  animate();
};

// Animer la scène
const animate = () => {
  animationFrame = requestAnimationFrame(animate);

  if (controls) {
    controls.update();
  }

  // Vérifier l'état du modèle
  if (model) {
    console.log('Modèle présent dans la scène');
    model.rotation.y += 0.005;
  } else {
    console.log('Modèle non chargé');
  }

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
  init();
});

onBeforeUnmount(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame);
  }
  window.removeEventListener('resize', onWindowResize);

  // Nettoyer les ressources Three.js
  if (renderer) {
    renderer.dispose();

    if (container.value) {
      const canvas = renderer.domElement;
      if (canvas.parentElement) {
        canvas.parentElement.removeChild(canvas);
      }
    }
  }
});
</script>

<style scoped>
.login-page {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

.model-container {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
