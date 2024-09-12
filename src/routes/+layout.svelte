<script lang="ts">
  import '../styles/style.scss';
  import { navigating, page } from '$app/stores';
  import { expoOut } from 'svelte/easing';
  import { slide } from 'svelte/transition';
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
          <nav
            class="navbar navbar-expand-md navbar-light d-none d-md-block position-static"
          >
            <ul
              id="main-nav"
              class="nav navbar-nav flex-row main-nav"
              itemscope=""
              itemtype="http://www.schema.org/SiteNavigationElement"
            >
              <li
                id="mn-menu-item-102"
                class="menu-item menu-item-type-custom menu-item-object-custom menu-item-has-children dropdown nav-item"
              >
                <a
                  href="/chiffres"
                  aria-haspopup="true"
                  aria-expanded="false"
                  class="dropdown-toggle nav-link"
                  id="66de95d8ceabb-menu-item-dropdown-102"
                  data-bs-toggle="dropdown"
                  ><span itemprop="name"
                    ><span>Chiffres clés France entière</span></span
                  ></a
                >
                <!-- Sous-menu -->
                <ul
                  class="dropdown-menu"
                  aria-labelledby="66de95d8ceabb-menu-item-dropdown-102"
                >
                  <li>
                    <a class="dropdown-item" href="/chiffres/1">Item 1</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/2">Item 2</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/3">Item 3</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/4">Item 4</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/5">Item 5</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/6">Item 6</a>
                  </li>
                  <li>
                    <a class="dropdown-item" href="/chiffres/7">Item 7</a>
                  </li>
                </ul>
              </li>

              <li
                id="mn-menu-item-104"
                class="menu-item menu-item-type-post_type menu-item-object-page menu-item-104 nav-item"
              >
                <a itemprop="url" href="/carte" class="nav-link"
                  ><span itemprop="name"
                    ><span>Chiffres clés par région</span></span
                  ></a
                >
              </li>
            </ul>
          </nav>
          <div class="d-flex align-items-center ms-auto">
            <nav class="navbar navbar-light ms-auto"></nav>
            <div class="aside-nav-wr d-xl-none ms-3">
              <button
                class="aside-menu-toggler btn"
                type="button"
                data-bs-toggle="offcanvas"
                data-bs-target="#aside-menu"
                aria-controls="aside-menu"
                aria-label="Navigation"
              >
                <span></span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </header>
{/if}
<slot />
