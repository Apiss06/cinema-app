<template>
  <div class="app-container">
    <!-- Navbar Desktop (Hanya muncul di laptop/tablet) -->
    <header class="navbar desktop-only">
      <div class="nav-left">
        <h1 class="logo">CINEFLOW</h1>
        <nav class="nav-links">
          <a href="#" class="active">Beranda</a>
          <a href="#" @click.prevent="showNotice('Halaman Bioskop')">Bioskop</a>
          <a href="#" @click.prevent="showNotice('Halaman Promo')">Promo</a>
          <a href="#" @click.prevent="showNotice('Halaman Tiket Saya')">Tiket Saya</a>
          <a href="#" @click.prevent="showNotice('Halaman Contact Us')">Contact Us</a>
        </nav>
      </div>

      <div class="nav-right">
        <div class="location-badge">
          <span class="icon">📍</span>
          <span class="location-name">Darjo Pride</span>
        </div>
        <div class="user-avatar" @click="showNotice('Halaman Login / Profil')">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
      </div>
    </header>

    <!-- Navbar Mobile (Hanya muncul di HP dengan badge lokasi dan ikon profil) -->
    <header class="navbar mobile-only">
      <h1 class="logo">CINEFLOW</h1>
      <div class="nav-right">
        <div class="location-badge">
          <span>📍 Darjo Pride</span>
        </div>
        <div class="user-avatar" @click="showNotice('Halaman Login / Profil')">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
            <circle cx="12" cy="7" r="4"></circle>
          </svg>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section 
      class="hero-section" 
      :style="{ backgroundImage: `linear-gradient(to right, rgba(15, 15, 20, 0.95) 30%, rgba(15, 15, 20, 0.4) 100%), url(${featured.bgImage})` }"
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

    <!-- Catalog Section -->
    <section class="catalog-section">
      <h3 class="section-title">Mau nonton apa hari ini?</h3>

      <!-- Filter Genre -->
      <div class="genre-filters">
        <button 
          v-for="genre in genres" 
          :key="genre"
          :class="['genre-btn', { active: selectedGenre === genre }]"
          @click="selectedGenre = genre"
        >
          {{ genre }}
        </button>
      </div>

      <!-- Movie Grid -->
      <div class="movie-grid">
        <div 
          v-for="movie in filteredMovies" 
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

    <!-- Promo Section (Desain Dipercantik & Elegan) -->
    <section class="promo-section">
      <div class="promo-card">
        <div class="promo-decoration"></div>
        <div class="promo-text">
          <span class="promo-badge">⚡ Penawaran Spesial</span>
          <h3>Nonton lebih hemat, ceritanya tetap hebat.</h3>
          <p>Dapatkan diskon eksklusif hingga 30% untuk pemesanan tiket bioskop pertama Anda menggunakan metode pembayaran pilihan.</p>
        </div>
        <button class="promo-btn" @click="showNotice('Klaim Promo')">Klaim Promo</button>
      </div>
    </section>

    <!-- Bottom Navigation Bar Khusus HP -->
    <nav class="bottom-nav mobile-only">
      <a href="#" class="active" @click.prevent="showNotice('Beranda')">🏠<span>Beranda</span></a>
      <a href="#" @click.prevent="showNotice('Bioskop')">🎬<span>Bioskop</span></a>
      <a href="#" @click.prevent="showNotice('Promo')">🏷️<span>Promo</span></a>
      <a href="#" @click.prevent="showNotice('Tiket Saya')">🎟️<span>Tiket</span></a>
      <a href="#" @click.prevent="showNotice('Contact Us')">📞<span>Kontak</span></a>
    </nav>
  </div>
</template>

<script>
import { featuredMovie, moviesList } from './moviesData.js';

export default {
  name: 'App',
  data() {
    return {
      featured: featuredMovie,
      movies: moviesList,
      genres: ['Semua', 'Drama', 'Sci-Fi', 'Horror', 'Action', 'Romance'],
      selectedGenre: 'Semua'
    };
  },
  computed: {
    filteredMovies() {
      if (this.selectedGenre === 'Semua') {
        return this.movies;
      }
      return this.movies.filter(movie => movie.genre.includes(this.selectedGenre));
    }
  },
  methods: {
    showNotice(pageName) {
      alert(`${pageName} akan segera tersedia.`);
    },
    selectMovie(movie) {
      alert(`Anda memilih film: ${movie.title}`);
    }
  }
};
</script>

<style scoped>
.app-container {
  background-color: #0f0f14;
  color: #ffffff;
  min-height: 100vh;
  width: 100%;
  padding-bottom: 60px;
}

.mobile-only {
  display: none !important;
}

.desktop-only {
  display: flex !important;
}

.navbar {
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 20px 40px;
  background-color: rgba(15, 15, 20, 0.95);
  backdrop-filter: blur(10px);
  position: sticky;
  top: 0;
  z-index: 100;
}

.nav-left {
  display: flex;
  align-items: center;
  gap: 30px;
}

.logo {
  font-size: 1.4rem;
  font-weight: 800;
  letter-spacing: 2px;
  color: #f59e0b;
}

.nav-links {
  display: flex;
  gap: 20px;
}

.nav-links a {
  color: #9ca3af;
  text-decoration: none;
  font-size: 0.9rem;
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
  gap: 15px;
}

.location-badge {
  background-color: #1f2937;
  padding: 6px 14px;
  border-radius: 20px;
  font-size: 0.85rem;
  display: flex;
  align-items: center;
  gap: 6px;
  border: 1px solid #374151;
}

.user-avatar {
  background-color: #1f2937;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border: 1px solid #374151;
  color: #f59e0b;
  transition: background-color 0.2s;
}

.user-avatar:hover {
  background-color: #374151;
}

.hero-section {
  width: 100%;
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
  margin-bottom: 24px;
}

.hero-actions {
  display: flex;
  gap: 14px;
}

.btn-primary {
  background-color: #f59e0b;
  color: #000000;
  border: none;
  padding: 12px 26px;
  border-radius: 8px;
  font-weight: 700;
  cursor: pointer;
  transition: opacity 0.2s;
}

.btn-primary:hover {
  opacity: 0.9;
}

.btn-secondary {
  background-color: transparent;
  color: #ffffff;
  border: 1px solid #4b5563;
  padding: 12px 22px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
}

.catalog-section {
  width: 100%;
  padding: 40px;
}

.section-title {
  font-size: 1.4rem;
  font-weight: 700;
  margin-bottom: 16px;
}

.genre-filters {
  display: flex;
  gap: 10px;
  margin-bottom: 24px;
  overflow-x: auto;
  padding-bottom: 8px;
}

.genre-btn {
  background-color: #181824;
  color: #9ca3af;
  border: 1px solid #27273a;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 0.85rem;
  cursor: pointer;
  white-space: nowrap;
}

.genre-btn.active {
  background-color: #f59e0b;
  color: #000000;
  border-color: #f59e0b;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 20px;
}

.movie-card {
  background-color: #181824;
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  border: 1px solid #27273a;
  transition: transform 0.2s;
}

.movie-card:hover {
  transform: translateY(-4px);
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
  top: 8px;
  right: 8px;
  background-color: rgba(0, 0, 0, 0.75);
  padding: 4px 8px;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
}

.movie-info {
  padding: 12px;
}

/* ================= PROMO SECTION STYLING (DIJADIKAN LEBIH MENARIK) ================= */
.promo-section {
  width: 100%;
  padding: 20px 40px 40px 40px;
}

.promo-card {
  position: relative;
  background: linear-gradient(135deg, #1f2937 0%, #111827 100%);
  border: 1px solid #374151;
  border-radius: 16px;
  padding: 32px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  overflow: hidden;
  box-shadow: 0 10px 30px -10px rgba(0, 0, 0, 0.5);
}

.promo-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 5px;
  height: 100%;
  background-color: #f59e0b;
}

.promo-text {
  max-width: 700px;
}

.promo-badge {
  display: inline-block;
  background-color: rgba(245, 158, 11, 0.15);
  color: #f59e0b;
  font-size: 0.75rem;
  font-weight: 700;
  text-transform: uppercase;
  padding: 6px 12px;
  border-radius: 6px;
  margin-bottom: 12px;
  letter-spacing: 1px;
  border: 1px solid rgba(245, 158, 11, 0.3);
}

.promo-text h3 {
  font-size: 1.6rem;
  font-weight: 800;
  margin-bottom: 8px;
  color: #ffffff;
}

.promo-text p {
  color: #9ca3af;
  font-size: 0.95rem;
  line-height: 1.6;
}

.promo-btn {
  background-color: #f59e0b;
  color: #000000;
  border: none;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 700;
  font-size: 0.9rem;
  cursor: pointer;
  transition: transform 0.2s, opacity 0.2s;
  white-space: nowrap;
}

.promo-btn:hover {
  opacity: 0.9;
  transform: scale(1.03);
}

/* ================= RESPONSIVE DESIGN (MOBILE) ================= */
@media (max-width: 768px) {
  .desktop-only {
    display: none !important;
  }

  .mobile-only {
    display: flex !important;
  }

  .navbar {
    padding: 12px 16px;
  }

  .hero-section {
    padding: 40px 16px;
    min-height: 350px;
  }

  .hero-title {
    font-size: 2.2rem;
  }

  .catalog-section, .promo-section {
    padding: 16px;
  }

  .movie-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
  }

  .promo-card {
    flex-direction: column;
    align-items: flex-start;
    gap: 20px;
    padding: 24px;
  }

  .promo-btn {
    width: 100%;
  }

  /* Bottom Navigation Bar di HP */
  .bottom-nav {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    background-color: rgba(15, 15, 20, 0.98);
    backdrop-filter: blur(10px);
    border-top: 1px solid #27273a;
    display: flex;
    justify-content: space-around;
    padding: 10px 0;
    z-index: 1000;
  }

  .bottom-nav a {
    color: #9ca3af;
    text-decoration: none;
    font-size: 1.2rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 4px;
  }

  .bottom-nav a span {
    font-size: 0.65rem;
  }

  .bottom-nav a.active,
  .bottom-nav a:hover {
    color: #f59e0b;
  }
}
</style>