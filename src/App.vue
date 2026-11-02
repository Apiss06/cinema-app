<template>
  <div class="app-container">
    <!-- Navbar / Header -->
    <header class="navbar">
      <div class="nav-left">
        <h1 class="logo">CINEFLOW</h1>
        <nav class="nav-links">
          <a href="#" class="active">Beranda</a>
          <a href="#" @click.prevent="showCinemaNotice">Bioskop</a>
          <a href="#">Tiket Saya</a>
        </nav>
      </div>

      <div class="nav-right">
        <!-- Lokasi diubah menjadi Darjo Pride -->
        <div class="location-badge">
          <span class="icon">📍</span>
          <span class="location-name">Darjo Pride</span>
        </div>
        <div class="user-avatar">👤</div>
      </div>
    </header>

    <!-- Hero Banner (Featured Movie) -->
    <section 
      class="hero-section" 
      :style="{ backgroundImage: `linear-gradient(to right, rgba(15, 15, 20, 0.95), rgba(15, 15, 20, 0.4)), url(${featured.bgImage})` }"
    >
      <div class="hero-content">
        <span class="badge">{{ featured.subtitle }}</span>
        <h2 class="hero-title">{{ featured.title }}</h2>
        <div class="hero-meta">
          <span>⭐ {{ featured.rating }}</span>
          <span>•</span>
          <span>{{ featured.genre }}</span>
          <span>•</span>
          <span>{{ featured.duration }}</span>
        </div>
        <p class="hero-description">{{ featured.description }}</p>

        <div class="hero-actions">
          <button class="btn-primary" @click="selectMovie(featured)">Beli Tiket</button>
          <button class="btn-secondary">Lihat Trailer</button>
        </div>
      </div>
    </section>

    <!-- Movie Catalog -->
    <section class="catalog-section">
      <h3 class="section-title">Mau nonton apa hari ini?</h3>

      <div class="movie-grid">
        <div 
          v-for="movie in movies" 
          :key="movie.id" 
          class="movie-card"
          @click="selectMovie(movie)"
        >
          <div class="poster-wrapper">
            <img :src="movie.poster" :alt="movie.title" />
            <span class="rating-tag">⭐ {{ movie.rating }}</span>
          </div>
          <div class="movie-info">
            <h4>{{ movie.title }}</h4>
            <p>{{ movie.genre }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Promo Banner -->
    <section class="promo-section">
      <div class="promo-card">
        <div class="promo-text">
          <span class="promo-badge">Nonton Lebih Hemat</span>
          <h3>Nonton lebih hemat, ceritanya tetap hebat.</h3>
          <p>Dapatkan diskon hingga 30% untuk pemesanan tiket pertama Anda.</p>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { featuredMovie, moviesList } from './moviesData.js';

export default {
  name: 'App',
  data() {
    return {
      featured: featuredMovie,
      movies: moviesList
    };
  },
  methods: {
    showCinemaNotice() {
      alert('Halaman bioskop akan segera tersedia.');
    },
    selectMovie(movie) {
      alert(`Anda memilih film: ${movie.title}`);
    }
  }
};
</script>

<style scoped>
/* Reset & Base Dark Theme */
.app-container {
  background-color: #0f0f14;
  color: #ffffff;
  min-height: 100vh;
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  padding-bottom: 40px;
}

/* Navbar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  background-color: rgba(15, 15, 20, 0.9);
  backdrop-filter: blur(10px);
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-left {
  display: flex;
  align-items: center;
  gap: 40px;
}

.logo {
  font-size: 1.4rem;
  font-weight: 800;
  letter-spacing: 2px;
  color: #f59e0b;
}

.nav-links {
  display: flex;
  gap: 24px;
}

.nav-links a {
  color: #9ca3af;
  text-decoration: none;
  font-size: 0.95rem;
  transition: color 0.2s;
}

.nav-links a:hover,
.nav-links a.active {
  color: #ffffff;
  font-weight: 600;
}

.nav-right {
  display: flex;
  align-items: center;
  gap: 20px;
}

.location-badge {
  background-color: #1f2937;
  padding: 8px 14px;
  border-radius: 20px;
  font-size: 0.85rem;
  display: flex;
  align-items: center;
  gap: 6px;
  border: 1px solid #374151;
}

.user-avatar {
  background-color: #374151;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

/* Hero Section */
.hero-section {
  padding: 80px 40px;
  background-size: cover;
  background-position: center;
  min-height: 450px;
  display: flex;
  align-items: center;
}

.hero-content {
  max-width: 600px;
}

.badge {
  color: #f59e0b;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 1.5px;
  text-transform: uppercase;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 900;
  margin: 10px 0;
  line-height: 1.1;
}

.hero-meta {
  display: flex;
  gap: 12px;
  color: #d1d5db;
  font-size: 0.9rem;
  margin-bottom: 16px;
}

.hero-description {
  color: #9ca3af;
  line-height: 1.6;
  margin-bottom: 28px;
}

.hero-actions {
  display: flex;
  gap: 16px;
}

.btn-primary {
  background-color: #f59e0b;
  color: #000000;
  border: none;
  padding: 12px 28px;
  border-radius: 8px;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s, background-color 0.2s;
}

.btn-primary:hover {
  background-color: #d97706;
  transform: translateY(-2px);
}

.btn-secondary {
  background-color: transparent;
  color: #ffffff;
  border: 1px solid #4b5563;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

/* Catalog Section */
.catalog-section {
  padding: 40px;
}

.section-title {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 24px;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 24px;
}

.movie-card {
  background-color: #181824;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
  border: 1px solid #27273a;
}

.movie-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.5);
}

.poster-wrapper {
  position: relative;
  aspect-ratio: 2/3;
}

.poster-wrapper img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.rating-tag {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: rgba(0, 0, 0, 0.75);
  backdrop-filter: blur(4px);
  padding: 4px 8px;
  border-radius: 6px;
  font-size: 0.8rem;
  font-weight: 600;
}

.movie-info {
  padding: 16px;
}

.movie-info h4 {
  font-size: 1rem;
  font-weight: 700;
  margin: 0 0 6px 0;
}

.movie-info p {
  color: #9ca3af;
  font-size: 0.85rem;
  margin: 0;
}

/* Promo Banner */
.promo-section {
  padding: 20px 40px;
}

.promo-card {
  background: linear-gradient(135deg, #1f2937 0%, #111827 100%);
  border: 1px solid #374151;
  border-radius: 16px;
  padding: 32px;
}

.promo-badge {
  color: #f59e0b;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.promo-text h3 {
  font-size: 1.4rem;
  margin: 8px 0;
}

.promo-text p {
  color: #9ca3af;
  margin: 0;
}
</style>