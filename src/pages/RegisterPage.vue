<template>
  <q-page class="q-pa-md page-container">
    <div class="form-container">
      <div class="form-header">
        <img src="https://cdn.quasar.dev/img/mountains.jpg" alt="Login Image" class="form-image" />
      </div>
      <div class="form-content">
        <q-form>
          <q-input v-model="login" label="Login" required />
          <q-input v-model="email" label="Email" type="email" required />
          <q-input v-model="password" :type="isPwd ? 'password' : 'text'" label="Mot de passe" required>
            <template v-slot:append>
              <q-icon
                :name="isPwd ? 'visibility_off' : 'visibility'"
                class="cursor-pointer"
                @click="isPwd = !isPwd"
              />
            </template>
          </q-input>
          <q-input v-model="confirmPassword" :type="isConfirmPwd ? 'password' : 'text'" label="Confirmation mot de passe" required>
            <template v-slot:append>
              <q-icon
                :name="isConfirmPwd ? 'visibility_off' : 'visibility'"
                class="cursor-pointer"
                @click="isConfirmPwd = !isConfirmPwd"
              />
            </template>
          </q-input>
          <div class="form-actions">
            <q-btn type="submit" label="Créer un compte" color="primary" class="q-mt-md" />
          </div>
        </q-form>
        <!-- Updated clickable link -->
        <div class="form-footer">
          <router-link to="/auth/login" class="form-link">
            Vous avez deja un compte?
          </router-link>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const login = ref('');
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const isPwd = ref(true);
const isConfirmPwd = ref(true);
</script>

<style scoped>
.page-container {
  display: flex;
  justify-content: center;
  background: linear-gradient(rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.5)), url('https://cdn.quasar.dev/img/mountains.jpg');
  background-size: cover;
  align-items: center;
  height: 90vh;
  overflow: hidden;
}

.form-container {
  max-width: 400px;
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}

.form-header {
  width: 100%;
}

.form-image {
  width: 100%;
  height: auto;
}

.form-content {
  padding: 0 25px 30px 25px;
}

.form-actions {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

.form-footer {
  margin-top: 20px;
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
