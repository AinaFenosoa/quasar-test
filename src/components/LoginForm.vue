<template>
  <q-dialog v-model="showDialog" persistent>
    <q-card class="form-container" style="width: 400px">
      <!-- Bouton de fermeture en X en haut à droite -->
      <q-btn
        round
        flat
        dense
        icon="close"
        class="close-btn"
        @click="closeDialog"
      />

      <!-- En-tête avec image -->
      <div class="form-header">
        <img src="https://cdn.quasar.dev/img/mountains.jpg" alt="Login Image" class="form-image" />
      </div>

      <!-- Contenu du formulaire -->
      <div class="form-content">
        <q-form @submit="submitForm" class="form-inputs">
          <q-input
            v-model="emailModel"
            label="Email"
            type="email"
            lazy-rules="ondemand"
            :rules="[ val => val && val.length > 0 || 'Veuillez saisir votre email',
                      val => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(val) || 'Email invalide' ]"
          />

          <q-input
            v-model="passwordModel"
            label="Mot de passe"
            :type="isPwd ? 'password' : 'text'"
            lazy-rules="ondemand"
            :rules="[ val => val && val.length > 0 || 'Veuillez saisir votre mot de passe' ]"
          >
            <template v-slot:append>
              <q-icon
                :name="isPwd ? 'visibility_off' : 'visibility'"
                class="cursor-pointer"
                @click="isPwd = !isPwd"
              />
            </template>
          </q-input>

          <div class="form-actions q-mt-xs">
            <q-btn
              label="Connexion"
              type="submit"
              color="primary"
              no-caps
            />
          </div>

          <div class="text-center q-py-xs">ou</div>

          <q-btn
            class="full-width bg-red-7 text-white"
            icon="fab fa-google"
            label="Se connecter avec Google"
            no-caps
            @click="googleLogin"
          />

          <div class="form-footer q-mt-xs">
            <a href="#" class="form-link">Vous n'avez pas encore de compte?</a>
          </div>
        </q-form>
      </div>
    </q-card>
  </q-dialog>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

// Props
const props = defineProps({
  modelValue: {
    type: Boolean,
    default: false
  },
  email: {
    type: String,
    default: ''
  },
  password: {
    type: String,
    default: ''
  }
});

// Émetteur d'événements
const emit = defineEmits(['update:modelValue', 'update:email', 'update:password', 'submit', 'google-login']);

// Variables locales
const isPwd = ref(true);

// Computed properties avec two-way binding
const showDialog = computed({
  get: () => props.modelValue,
  set: (value) => emit('update:modelValue', value)
});

const emailModel = computed({
  get: () => props.email,
  set: (value) => emit('update:email', value)
});

const passwordModel = computed({
  get: () => props.password,
  set: (value) => emit('update:password', value)
});

// Méthodes
const closeDialog = () => {
  emit('update:modelValue', false);
};

const submitForm = () => {
  emit('submit', { email: emailModel.value, password: passwordModel.value });
};

const googleLogin = () => {
  emit('google-login');
};
</script>

<style lang="scss" scoped>
// Styles pour le popup de connexion
.form-container {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  position: relative;
}

.close-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  z-index: 10;
  background: rgba(255, 255, 255, 0.7);
  color: #555;
}

.form-header {
  width: 100%;
}

.form-image {
  width: 100%;
  height: auto;
  max-height: 150px;
  object-fit: cover;
}

.form-content {
  padding: 0 25px 20px 25px;
}

.form-inputs {
  .q-input {
    margin-bottom: 10px;
    padding: 0px;
  }
}

.form-actions {
  display: flex;
  justify-content: center;
  margin-top: 15px;
}

.form-footer {
  margin-top: 15px;
  text-align: center;
}

.form-link {
  cursor: pointer;
  color: #575757;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.form-link:hover {
  color: #1e1d1d;
}
</style>
