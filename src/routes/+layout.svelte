<script lang="ts">
  import '../styles/style.scss';
  import { page } from '$app/stores';
  import { onMount } from 'svelte';

  // Pour gérer l'état du menu mobile (ouvert/fermé)
  let isMenuOpen = false;

  function toggleMenu() {
    isMenuOpen = !isMenuOpen;
  }

  // Ferme le menu si on redimensionne l'écran à une taille plus grande
  onMount(() => {
    const mediaQuery = window.matchMedia('(min-width: 768px)');
    function handleResize() {
      if (mediaQuery.matches) {
        isMenuOpen = false;
      }
    }
    mediaQuery.addEventListener('change', handleResize);

    return () => {
      mediaQuery.removeEventListener('change', handleResize);
    };
  });
</script>

<!-- Start of Selection -->

{#if $page.url.pathname !== '/'}
  <header id="header" class="site-header sticky-top">
    <div class="site-header-container">
      <div class="container position-relative">
        <div class="header-wr d-flex align-items-center">
          <div class="brand-header me-md-5">
            <a class="navbar-brand" href="/">
              <img
                class="brand-h-logo img-fluid"
                loading="eager"
                src="logo-header.svg"
                alt="Feden"
              />
            </a>
          </div>

          <!-- Menu principal (pour les grands écrans) -->
          <nav
            class="navbar navbar-expand-md navbar-light d-none d-md-block position-static"
          >
            <ul id="main-nav" class="nav navbar-nav flex-row main-nav">
              <li class="menu-item">
                <a href="/chiffres" class="dropdown-toggle"
                  >Chiffres clés France entière</a
                >
                <ul class="dropdown-menu">
                  <li>
                    <a class="dropdown-item" href="#dataviz_chiffres_chaud"
                      >Part des réseaux de chaleur</a
                    >
                  </li>
                  <li>
                    <a
                      class="dropdown-item"
                      href="#dataviz_reseaux_chaleur_froid"
                      >Réseaux de chaleur et de froid</a
                    >
                  </li>
                  <li>
                    <a class="dropdown-item" href="#taux_energie_renouvelable"
                      >Taux d’énergies renouvelables</a
                    >
                  </li>
                  <li>
                    <a class="dropdown-item" href="#mix_energetique"
                      >Mix énergétique</a
                    >
                  </li>
                  <li>
                    <a class="dropdown-item" href="#empreinte_carbone"
                      >Empreinte carbone</a
                    >
                  </li>
                  <li>
                    <a class="dropdown-item" href="#secteurs_livraison"
                      >Secteurs de livraison</a
                    >
                  </li>
                  <li>
                    <a class="dropdown-item" href="#taille_villes"
                      >Répartition sur la taille des villes</a
                    >
                  </li>
                </ul>
              </li>
              <li class="menu-item">
                <a href="/carte">Chiffres clés par région</a>
              </li>
            </ul>
          </nav>

          <!-- Menu mobile (d-flex) -->
          <div class="d-flex align-items-center ms-auto">
            <div class="aside-nav-wr d-xl-none ms-3">
              <button class="aside-menu-toggler btn" on:click={toggleMenu}>
                <!-- Icône de menu hamburger -->
              </button>

              <!-- Sous-menu pour mobiles -->
              {#if isMenuOpen}
                <ul class="mobile-menu nav flex-column">
                  <li class="menu-item">
                    <a href="/chiffres" class="dropdown-toggle"
                      >Chiffres clés France entière</a
                    >
                    <ul class="dropdown-menu">
                      <li>
                        <a class="dropdown-item" href="#dataviz_chiffres_chaud"
                          >Part des réseaux de chaleur</a
                        >
                      </li>
                      <li>
                        <a
                          class="dropdown-item"
                          href="#dataviz_reseaux_chaleur_froid"
                          >Réseaux de chaleur et de froid</a
                        >
                      </li>
                      <li>
                        <a
                          class="dropdown-item"
                          href="#taux_energie_renouvelable"
                          >Taux d’énergies renouvelables</a
                        >
                      </li>
                      <li>
                        <a class="dropdown-item" href="#mix_energetique"
                          >Mix énergétique</a
                        >
                      </li>
                      <li>
                        <a class="dropdown-item" href="#empreinte_carbone"
                          >Empreinte carbone</a
                        >
                      </li>
                      <li>
                        <a class="dropdown-item" href="#secteurs_livraison"
                          >Secteurs de livraison</a
                        >
                      </li>
                      <li>
                        <a class="dropdown-item" href="#taille_villes"
                          >Répartition sur la taille des villes</a
                        >
                      </li>
                    </ul>
                  </li>
                  <li class="menu-item">
                    <a href="/carte">Chiffres clés par région</a>
                  </li>
                </ul>
              {/if}
            </div>
          </div>
        </div>
      </div>
    </div>
  </header>
{/if}
<slot />

<style>
  /* Style du menu déroulant */
  .navbar-nav {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .menu-item {
    position: relative;
    display: inline-block;
  }

  .menu-item > a {
    display: block;
    padding: 10px;
    text-decoration: none;
    color: #000;
  }

  .dropdown-menu {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    background-color: #fff;
    border: 1px solid #ddd;
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .dropdown-menu .dropdown-item {
    text-decoration: none;
    color: #000;
  }

  .dropdown-item {
    padding-left: 20px;
  }

  .dropdown-menu .dropdown-item:hover {
    background-color: #f1f1f1;
  }

  /* Afficher le menu déroulant au survol */
  .menu-item:hover .dropdown-menu {
    display: block;
  }

  .mobile-menu {
    list-style: none;
    padding: 0;
    margin: 0;
    background-color: #fff;
    position: absolute;
    top: 50px;
    left: 0;
    border: 1px solid #ddd;
    width: auto;
  }

  .mobile-menu .menu-item {
    padding: 10px;
    border-bottom: 1px solid #ddd;
  }

  .mobile-menu .menu-item > a {
    color: #000;
    text-decoration: none;
  }

  .mobile-menu .dropdown-menu {
    list-style: none;
    padding: 30;
    margin: 0;
  }

  .mobile-menu .dropdown-item {
    padding: 5px 10px;
    text-decoration: none;
    color: #000;
  }

  .navbar-toggler-icon {
    display: inline-block;
    width: 1.5em;
    height: 1.5em;
    background-color: #000;
  }
</style>
