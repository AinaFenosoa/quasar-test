<template>
  <q-layout view="hHh lpR fff">
    <!-- Navigation avec menu décalé à gauche -->
    <q-header class="header-bg q-py-md header-with-wave">
      <q-toolbar class="container">
        <div class="row full-width items-center">
          <div class="col-md-3 col-sm-4 col-xs-6">
            <div class="text-h5 text-weight-bold text-primary">MIX<span class="text-white">ALL</span></div>
          </div>
          <div class="col-md-5 col-sm-6 col-xs-12">
            <q-tabs
              class="text-white"
              inline-label
              indicator-color="transparent"
              active-color="white"
              no-caps
            >
              <q-tab name="home" label="Accueil" />
              <q-tab name="products" label="Solutions" />
              <q-tab name="services" label="Services" />
              <q-tab name="about" label="À propos" />
              <q-tab name="contact" label="Contact" />
            </q-tabs>
          </div>
          <div class="col-md-4 col-xs-12 text-right q-gutter-sm">
            <!-- Bouton qui ouvre le popup de connexion -->
            <q-btn color="white" text-color="primary" label="Se connecter" no-caps rounded outline @click="openLoginModal" />
            <q-btn color="primary" label="Créer un compte" no-caps rounded @click="openRegisterModal" />
          </div>
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <!-- Hero Section avec animation -->
      <section class="hero-section">
        <div class="overlay"></div>
        <div class="container">
          <div class="row">
            <div class="col-md-7 col-sm-10 col-xs-12">
              <div class="hero-content q-mt-xl">
                <h1 class="text-h2 text-weight-bold text-white animate-fadeIn">Transformez votre vision en réalité digitale</h1>
                <p class="text-h6 text-white q-mt-md animate-slideUp">Solutions innovantes pour propulser votre entreprise vers de nouveaux horizons</p>
                <div class="q-mt-lg q-gutter-md animate-fadeIn-delay">
                  <q-btn color="primary" size="lg" label="Découvrir nos solutions" no-caps rounded />
                  <q-btn outline color="white" size="lg" label="En savoir plus" no-caps rounded />
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="shape-divider">
          <svg data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 120" preserveAspectRatio="none">
            <path d="M321.39,56.44c58-10.79,114.16-30.13,172-41.86,82.39-16.72,168.19-17.73,250.45-.39C823.78,31,906.67,72,985.66,92.83c70.05,18.48,146.53,26.09,214.34,3V0H0V27.35A600.21,600.21,0,0,0,321.39,56.44Z" class="shape-fill"></path>
          </svg>
        </div>
      </section>

      <!-- Section fonctionnalités -->
      <section class="features-section q-py-xl">
        <div class="container text-center q-mb-xl">
          <h2 class="text-h3 text-weight-bold q-mb-md">Nos solutions d'excellence</h2>
          <p class="text-subtitle1 q-mx-auto" style="max-width: 700px">Découvrez comment nos technologies innovantes peuvent transformer votre entreprise et créer une expérience utilisateur exceptionnelle.</p>
        </div>

        <div class="container">
          <div class="row q-col-gutter-lg">
            <div v-for="(feature, index) in features" :key="index" class="col-md-4 col-sm-6 col-xs-12">
              <q-card class="feature-card q-pa-md" flat bordered>
                <q-card-section>
                  <div class="feature-icon text-primary q-mb-md">
                    <q-icon :name="feature.icon" size="56px" />
                  </div>
                  <h3 class="text-h5 text-weight-bold">{{ feature.title }}</h3>
                  <p class="q-mt-sm">{{ feature.description }}</p>
                </q-card-section>
                <q-card-actions>
                  <q-btn flat color="primary" label="En savoir plus" no-caps />
                </q-card-actions>
              </q-card>
            </div>
          </div>
        </div>
      </section>

      <!-- Section statistiques -->
      <section class="stats-section q-py-xl">
        <div class="container">
          <div class="row q-col-gutter-lg justify-center">
            <div v-for="(stat, index) in stats" :key="index" class="col-md-3 col-sm-6 col-xs-12 text-center">
              <div class="stat-item">
                <div class="stat-value text-h2 text-primary text-weight-bold">{{ stat.value }}</div>
                <div class="stat-label text-subtitle1">{{ stat.label }}</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Section témoignages -->
      <section class="testimonial-section q-py-xl">
        <div class="container">
          <div class="text-center q-mb-xl">
            <h2 class="text-h3 text-weight-bold">Ce que nos clients disent</h2>
            <p class="text-subtitle1 q-mx-auto" style="max-width: 700px">Découvrez les expériences de nos clients qui ont transformé leur entreprise grâce à nos solutions.</p>
          </div>

          <q-carousel
            v-model="testimonialSlide"
            animated
            arrows
            navigation
            infinite
            padding
            height="400px"
            class="testimonial-carousel bg-transparent text-black"
          >
            <q-carousel-slide v-for="(testimonial, index) in testimonials" :key="index" :name="index" class="column no-wrap">
              <div class="row fit justify-center">
                <div class="col-md-8 col-xs-10">
                  <q-card flat class="testimonial-card q-pa-lg">
                    <q-card-section>
                      <div class="text-h6 text-italic">"{{ testimonial.quote }}"</div>
                      <div class="row items-center q-mt-lg">
                        <q-avatar size="56px">
                          <img :src="testimonial.avatar" />
                        </q-avatar>
                        <div class="q-ml-md">
                          <div class="text-weight-bold">{{ testimonial.name }}</div>
                          <div class="text-caption">{{ testimonial.position }}, {{ testimonial.company }}</div>
                        </div>
                      </div>
                    </q-card-section>
                  </q-card>
                </div>
              </div>
            </q-carousel-slide>
          </q-carousel>
        </div>
      </section>

      <!-- Section CTA -->
      <section class="cta-section q-py-xl text-center">
        <div class="container">
          <h2 class="text-h3 text-weight-bold text-white q-mb-md">Prêt à transformer votre entreprise?</h2>
          <p class="text-subtitle1 text-white q-mb-xl q-mx-auto" style="max-width: 700px">
            Rejoignez des centaines d'entreprises qui ont déjà fait le pas vers l'innovation
          </p>
          <q-btn color="white" text-color="primary" size="lg" label="Contactez-nous" no-caps rounded />
        </div>
      </section>
    </q-page-container>

    <!-- Footer avec la même couleur que le header -->
    <q-footer class="header-bg text-white">
      <div class="container q-py-md">
        <div class="row q-col-gutter-md">
          <div class="col-md-4 col-sm-6 col-xs-12">
            <div class="text-h6 text-weight-bold text-primary q-mb-sm">MIX<span class="text-white">ALL</span></div>
            <p class="q-mr-md text-caption">Nous créons des solutions digitales innovantes pour propulser votre entreprise.</p>
            <div class="row q-gutter-sm q-mt-sm">
              <q-btn round flat dense color="white" icon="fab fa-facebook-f" />
              <q-btn round flat dense color="white" icon="fab fa-twitter" />
              <q-btn round flat dense color="white" icon="fab fa-linkedin-in" />
              <q-btn round flat dense color="white" icon="fab fa-instagram" />
            </div>
          </div>
          <div class="col-md-2 col-sm-6 col-xs-12">
            <h6 class="text-weight-bold q-mb-sm text-body2">Solutions</h6>
            <q-list dense padding class="q-py-none">
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">Web Design</q-item-section>
              </q-item>
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">Mobile Apps</q-item-section>
              </q-item>
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">E-commerce</q-item-section>
              </q-item>
            </q-list>
          </div>
          <div class="col-md-2 col-sm-6 col-xs-12">
            <h6 class="text-weight-bold q-mb-sm text-body2">Entreprise</h6>
            <q-list dense padding class="q-py-none">
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">À propos</q-item-section>
              </q-item>
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">Équipe</q-item-section>
              </q-item>
              <q-item clickable v-ripple dense>
                <q-item-section class="text-caption">Contact</q-item-section>
              </q-item>
            </q-list>
          </div>
          <div class="col-md-4 col-sm-6 col-xs-12">
            <h6 class="text-weight-bold q-mb-sm text-body2">Newsletter</h6>
            <p class="q-mb-sm text-caption">Inscrivez-vous pour recevoir nos actualités</p>
            <q-input dark dense outlined v-model="newsletter" placeholder="Votre email" class="q-mb-sm">
              <template v-slot:append>
                <q-btn round dense flat color="primary" icon="send" />
              </template>
            </q-input>
          </div>
        </div>
        <div class="row justify-between q-mt-md">
          <div class="col-12 text-center text-caption">
            © {{ new Date().getFullYear() }} MIXALL. Tous droits réservés.
          </div>
        </div>
      </div>
    </q-footer>

    <!-- Remplacer le q-dialog par notre composant LoginForm -->
    <login-form
      v-model="showLoginModal"
      v-model:email="email"
      v-model:password="password"
      @submit="onSubmit"
      @google-login="loginWithGoogle"
    />

    <!-- Ajouter notre composant RegisterForm -->
    <register-form
      v-model="showRegisterModal"
      v-model:username="username"
      v-model:email="email"
      v-model:password="password"
      v-model:confirmPassword="confirmPassword"
      @submit="onRegisterSubmit"
      @google-login="registerWithGoogle"
    />
  </q-layout>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import LoginForm from 'components/LoginForm.vue';
import RegisterForm from 'components/RegisterForm.vue';

// Define types for form data
interface RegisterData {
  username: string;
  email: string;
  password: string;
  confirmPassword: string;
}

// Variables pour le popup de connexion
const showLoginModal = ref(false);
const email = ref('');
const password = ref('');

// Variables pour le popup d'inscription
const showRegisterModal = ref(false);
const username = ref('');
const confirmPassword = ref('');

// Variables pour les carrousels
const testimonialSlide = ref(0);
const newsletter = ref('');

// Fonction pour ouvrir le popup de connexion
const openLoginModal = () => {
  showLoginModal.value = true;
};

// Fonction pour ouvrir le popup d'inscription
const openRegisterModal = () => {
  showRegisterModal.value = true;
};

// Fonction pour gérer la soumission du formulaire
const onSubmit = () => {
  console.log('Tentative de connexion avec:', email.value, password.value);
  // Ici vous mettriez votre logique de connexion réelle
  // Puis fermeture du popup en cas de succès
  // showLoginModal.value = false;
};

// Fonction pour la connexion avec Google
const loginWithGoogle = () => {
  console.log('Tentative de connexion avec Google');
  // Implémentez ici l'authentification Google
};

// Fonction pour gérer l'inscription
const onRegisterSubmit = (data: RegisterData) => {
  console.log('Tentative d\'inscription avec:', data);
  // Ici vous mettriez votre logique d'inscription réelle
  // Puis fermeture du popup en cas de succès
  // showRegisterModal.value = false;
};

// Fonction pour l'inscription avec Google
const registerWithGoogle = () => {
  console.log('Tentative d\'inscription avec Google');
  // Implémentez ici l'authentification Google pour l'inscription
};

// Données dynamiques
const features = ref([
  {
    icon: 'devices',
    title: 'Design Responsive',
    description: 'Interfaces adaptées à tous les appareils pour une expérience utilisateur optimale'
  },
  {
    icon: 'speed',
    title: 'Hautes Performances',
    description: 'Solutions optimisées pour des temps de chargement rapides et une efficacité maximale'
  },
  {
    icon: 'security',
    title: 'Sécurité Avancée',
    description: 'Protection de vos données avec les dernières technologies de sécurité'
  },
  {
    icon: 'analytics',
    title: 'Analyses Détaillées',
    description: 'Suivez vos performances avec des tableaux de bord analytiques personnalisés'
  },
  {
    icon: 'integration_instructions',
    title: 'Intégration Facile',
    description: 'Compatible avec vos systèmes existants pour une transition en douceur'
  },
  {
    icon: 'support_agent',
    title: 'Support 24/7',
    description: 'Notre équipe d\'experts est disponible pour vous aider à tout moment'
  }
]);

const stats = ref([
  { value: '250+', label: 'Clients satisfaits' },
  { value: '95%', label: 'Taux de satisfaction' },
  { value: '1500+', label: 'Projets livrés' },
  { value: '24/7', label: 'Support client' }
]);

const testimonials = ref([
  {
    quote: 'Grâce à MIXALL, nous avons pu augmenter nos ventes en ligne de 200% en seulement six mois. Leur expertise en design UX et en développement e-commerce a été un véritable game-changer pour notre entreprise.',
    name: 'Sophie Martin',
    position: 'Directrice Marketing',
    company: 'FashionTrends',
    avatar: 'https://cdn.quasar.dev/img/avatar5.jpg'
  },
  {
    quote: 'L\'application mobile développée par MIXALL a révolutionné notre façon de communiquer avec nos clients. Simple, intuitive et performante, c\'est exactement ce dont nous avions besoin.',
    name: 'Thomas Dubois',
    position: 'PDG',
    company: 'UrbanMobility',
    avatar: 'https://cdn.quasar.dev/img/avatar3.jpg'
  },
  {
    quote: 'La refonte de notre site web par MIXALL a complètement transformé notre image de marque. Nous avons vu une augmentation significative du temps passé sur notre site et du taux de conversion.',
    name: 'Léa Rousseau',
    position: 'Directrice Générale',
    company: 'EcoSolutions',
    avatar: 'https://cdn.quasar.dev/img/avatar2.jpg'
  }
]);
</script>

<style lang="scss" scoped>
// Variables générales
$primary-color: #2196f3;
$secondary-color: #1a237e;
$dark-color: #121212;
$light-color: #f5f5f5;
$header-bg-color: #1a1a2e; // Couleur personnalisée pour header et footer

// Container pour maintenir le contenu centré
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 16px;
}

// Header transparent qui devient plein au défilement
.q-header {
  transition: background-color 0.3s ease;
}

// Styles pour le header et footer
.header-bg {
  background-color: $header-bg-color;
}

// Section Hero avec image de fond et overlay
.hero-section {
  position: relative;
  height: 100vh;
  min-height: 600px;
  background-image: url('https://images.unsplash.com/photo-1557804506-669a67965ba0?ixlib=rb-1.2.1&auto=format&fit=crop&w=1567&q=80');
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  overflow: hidden;

  .overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, rgba($secondary-color, 0.9) 0%, rgba($dark-color, 0.85) 100%);
  }

  .hero-content {
    position: relative;
    z-index: 10;
  }
}

// Séparateur de forme
.shape-divider {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
  line-height: 0;

  svg {
    position: relative;
    display: block;
    width: calc(100% + 1.3px);
    height: 80px;

    .shape-fill {
      fill: $light-color;
    }
  }
}

// Section fonctionnalités
.features-section {
  background-color: $light-color;

  .feature-card {
    height: 100%;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    border-radius: 12px;

    &:hover {
      transform: translateY(-10px);
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.1);
    }

    .feature-icon {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 80px;
      height: 80px;
      border-radius: 50%;
      background-color: rgba($primary-color, 0.1);
    }
  }
}

// Section statistiques
.stats-section {
  background-color: white;

  .stat-item {
    padding: 20px;

    .stat-value {
      font-size: 3rem;
      margin-bottom: 10px;
    }
  }
}

// Section témoignages
.testimonial-section {
  background-color: $light-color;

  .testimonial-card {
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
    height: 100%;
  }
}

// Section CTA
.cta-section {
  background: linear-gradient(135deg, $primary-color 0%, $secondary-color 100%);
}

// Animations
.animate-fadeIn {
  animation: fadeIn 1s ease;
}

.animate-fadeIn-delay {
  animation: fadeIn 1s ease 0.5s forwards;
  opacity: 0;
}

.animate-slideUp {
  animation: slideUp 0.8s ease 0.3s forwards;
  opacity: 0;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
