<template>
  <q-page class="login-page">
    <div class="debug-panel" v-if="debugMessage">{{ debugMessage }}</div>

    <TresCanvas v-if="isClient">
      <!-- Configuration de la scène -->
      <TresPerspectiveCamera
        :position="[0, 1, 5]"
        :fov="50"
        :near="0.1"
        :far="1000"
      />

      <!-- Éclairage -->
      <TresAmbientLight :intensity="0.7" />
      <TresDirectionalLight :position="[1, 1, 1]" :intensity="0.8" />

      <!-- Aides visuelles pour le développement -->
      <TresAxesHelper :args="[5]" />
      <TresGridHelper :args="[10, 10]" />

      <!-- Cube de test -->
      <TresMesh :position="[2, 0, 0]">
        <TresBoxGeometry :args="[1, 1, 1]" />
        <TresMeshStandardMaterial color="#ff0000" />
      </TresMesh>

      <!-- Modèle 3D -->
      <Suspense>
        <UseGltf
          :path="currentPath"
          @load="onModelLoaded"
          @error="onModelError"
        />
      </Suspense>

      <!-- Contrôles de caméra -->
      <OrbitControls :enableDamping="true" :autoRotate="true" />
    </TresCanvas>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { TresCanvas } from '@tresjs/core';
import { OrbitControls, useGLTF } from '@tresjs/cientos';
import * as THREE from 'three';

// Pour éviter les erreurs de rendu SSR
const isClient = ref(false);
const debugMessage = ref('Initialisation...');

// Le composant UseGltf est la version TresJS de useGLTF
const UseGltf = useGLTF;

// Types pour les callbacks
interface GLTF {
  scene: THREE.Group;
  scenes: THREE.Group[];
  animations: THREE.AnimationClip[];
  cameras: THREE.Camera[];
  asset: {
    version: string;
    [key: string]: any;
  };
}

// Chemins possibles pour le modèle
const paths = [
  '/assets/voiture.glb',
  'assets/voiture.glb',
  './assets/voiture.glb',
  '../assets/voiture.glb'
];
const currentPath = ref(paths[0]);
let pathIndex = 0;

// Gestionnaire de chargement réussi
const onModelLoaded = (gltf: GLTF) => {
  debugMessage.value = `Modèle chargé avec succès depuis: ${currentPath.value}`;

  // Accéder au modèle si nécessaire
  const model = gltf.scene;

  // Analyser le modèle
  const box = new THREE.Box3().setFromObject(model);
  const size = box.getSize(new THREE.Vector3());
  const center = box.getCenter(new THREE.Vector3());

  debugMessage.value += `\nTaille: x=${size.x.toFixed(2)}, y=${size.y.toFixed(2)}, z=${size.z.toFixed(2)}`;
  debugMessage.value += `\nCentre: x=${center.x.toFixed(2)}, y=${center.y.toFixed(2)}, z=${center.z.toFixed(2)}`;

  // Centrer et redimensionner si nécessaire
  // Note: Dans TresJS, vous pouvez aussi le faire de manière déclarative
  const maxDim = Math.max(size.x, size.y, size.z);
  if (maxDim > 0) {
    const scale = 2 / maxDim;
    model.scale.set(scale, scale, scale);
    model.position.set(-center.x, -center.y, -center.z);
  }
};

// Gestionnaire d'erreur
const onModelError = (error: Error) => {
  debugMessage.value = `Erreur de chargement pour ${currentPath.value}: ${error.message || error}`;

  // Essayer le chemin suivant
  pathIndex++;
  if (pathIndex < paths.length) {
    debugMessage.value += `\nEssai du chemin suivant: ${paths[pathIndex]}`;
    currentPath.value = paths[pathIndex];
  } else {
    debugMessage.value += `\nTous les chemins ont échoué.`;
  }
};

// Initialisation côté client uniquement
onMounted(() => {
  isClient.value = true;
  debugMessage.value = 'TresJS initialisé, tentative de chargement du modèle...';
});
</script>

<style scoped>
.login-page {
  width: 100%;
  height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #fdaaaa 100%);
  position: relative;
}

.debug-panel {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px;
  border-radius: 5px;
  font-family: monospace;
  z-index: 100;
  max-width: 80%;
  white-space: pre-wrap;
}
</style>
