<script lang="ts">
  import '../styles/style.scss';
  import { navigating } from '$app/stores';
  import { expoOut } from 'svelte/easing';
  import { slide } from 'svelte/transition';
</script>

<header id="header" class="site-header sticky-top">
  <div class="site-header-container">
    <div class="container position-relative">
      <div class="header-wr d-flex align-items-center">
        <div class="brand-header me-md-5">
          <a class="navbar-brand" href="https://fedene.fr">
            <img
              class="brand-h-logo img-fluid"
              loading="eager"
              src="https://fedene.fr/wp-content/uploads/2023/11/logo-header.svg"
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
              id="mn-menu-item-103"
              class="menu-item menu-item-type-custom menu-item-object-custom menu-item-has-children menu-item-103 nav-item"
            ></li>
            <li
              id="mn-menu-item-103"
              class="menu-item menu-item-type-custom menu-item-object-custom menu-item-has-children is-mega-menu is-mega-fedene-menu dropdown menu-item-103 nav-item"
            >
              <a
                href="#"
                aria-haspopup="true"
                aria-expanded="false"
                class="dropdown-toggle nav-link"
                id="66ddad0558ab1-menu-item-dropdown-103"
                data-bs-toggle="dropdown"
              >
                <span itemprop="name"><span>La Fedene</span></span>
              </a>
              <div
                class="mega-dropdown p-0 dropdown-menu"
                aria-labelledby="66ddad0558ab1-menu-item-dropdown-103"
                role="menu"
              >
                <div class="grid g-0">
                  <div
                    class="mfm-headline-col mfm-padding-y g-col-12 g-col-lg-3 position-relative"
                  >
                    <div
                      class="mfm-headline d-flex flex-row flex-lg-column h-100"
                    >
                      <div class="mfm-headline-img mb-lg-3">
                        <img
                          class="mfm-headline-img img-fluid"
                          src="https://fedene.fr/wp-content/uploads/2024/01/Group-447-2.png"
                          alt=""
                        />
                      </div>
                      <div class="mfm-headline-content">
                        <div
                          class="mfm-headline-title fw-semibold text-heading-color has-link-no-underlined"
                        >
                          <a
                            href="https://fedene.fr/les-evenements-2024/"
                            class="mfm-headline-link stretched-link"
                            >Retrouvez tous les évènements de 2024</a
                          >
                        </div>
                      </div>
                    </div>
                  </div>
                  <div
                    class="mfm-content-col mfm-padding-y g-col-12 g-col-lg-9"
                  >
                    <div
                      class="mfm-content-col-row grid g-3 align-items-center mfm-h-100"
                    >
                      <div
                        class="mfm-nav-col g-col-12 g-col-md-5 g-col-xl-4 mt-0 d-flex align-items-center mfm-h-100"
                      >
                        <ul class="list-unstyled">
                          <li
                            class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1048 nav-item"
                          >
                            <a
                              itemprop="url"
                              href="https://fedene.fr/organisation-et-gouvernance/"
                              class="dropdown-item"
                            >
                              <span itemprop="name"
                                ><span>Organisation et gouvernance</span></span
                              >
                            </a>
                          </li>
                          <li
                            class="menu-item menu-item-type-post_type menu-item-object-page menu-item-3899 nav-item"
                          >
                            <a
                              itemprop="url"
                              href="https://fedene.fr/ambition-et-raison-detre/"
                              class="dropdown-item"
                            >
                              <span itemprop="name"
                                ><span>Ambition et raison d’être</span></span
                              >
                            </a>
                          </li>
                          <li
                            class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1049 nav-item"
                          >
                            <a
                              itemprop="url"
                              href="https://fedene.fr/devenir-adherent/"
                              class="dropdown-item"
                            >
                              <span itemprop="name"
                                ><span>Devenir adhérent</span></span
                              >
                            </a>
                          </li>
                          <li
                            class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1050 nav-item"
                          >
                            <a
                              itemprop="url"
                              href="https://fedene.fr/fedene-en-regions/"
                              class="dropdown-item"
                            >
                              <span itemprop="name"
                                ><span>Fedene en régions</span></span
                              >
                            </a>
                          </li>
                          <li
                            class="menu-item menu-item-type-post_type menu-item-object-page menu-item-1811 nav-item"
                          >
                            <a
                              itemprop="url"
                              href="https://fedene.fr/les-acteurs-de-la-transition-energetique/"
                              class="dropdown-item"
                            >
                              <span itemprop="name"
                                ><span>Nos partenaires</span></span
                              >
                            </a>
                          </li>
                        </ul>
                      </div>
                      <div
                        class="mfm-logos-col g-col-12 g-col-md-7 g-col-xl-8 mt-0 d-flex align-items-center mfm-h-100"
                      >
                        <div class="mfm-logos d-flex flex-wrap">
                          <a
                            href="https://fedene.fr/mission/efficacite-energetique/"
                            class="mfm-logo-link"
                          >
                            <img
                              class="mfm-logo-img img-fluid"
                              src="https://fedene.fr/wp-content/uploads/2023/12/fedene-snec-logo-couleur-rgb-1.svg"
                              alt="Efficacité énergétique"
                            />
                          </a>
                          <a
                            href="https://fedene.fr/mission/solutions-numeriques/"
                            class="mfm-logo-link"
                          >
                            <img
                              class="mfm-logo-img img-fluid"
                              src="https://fedene.fr/wp-content/uploads/2023/12/frdene-sypim-logo-couleur-rgb-1.svg"
                              alt="Solutions numériques"
                            />
                          </a>
                          <a
                            href="https://fedene.fr/mission/valorisation-energetique-des-dechets/"
                            class="mfm-logo-link"
                          >
                            <img
                              class="mfm-logo-img img-fluid"
                              src="https://fedene.fr/wp-content/uploads/2023/12/fedene-svdu-logo-couleur-rgb-1.svg"
                              alt="Déchets énergie"
                            />
                          </a>
                          <a
                            href="https://fedene.fr/mission/reseaux-de-chaleur-et-de-froid/"
                            class="mfm-logo-link"
                          >
                            <img
                              class="mfm-logo-img img-fluid"
                              src="https://fedene.fr/wp-content/uploads/2023/12/fedene-sncu-logo-couleur-rgb-1.svg"
                              alt="Réseaux de chaleur &amp; froid"
                            />
                          </a>
                          <a
                            href="https://sypemi.com/"
                            class="mfm-logo-link"
                            target="_blank"
                          >
                            <a
                              href="https://sypemi.com/"
                              class="mfm-logo-link"
                              target="_blank"
                            >
                              <img
                                class="mfm-logo-img img-fluid"
                                src="https://fedene.fr/wp-content/uploads/2023/12/logo-sypemi.svg"
                                alt="Le syndicat des professionnels du Facility Management"
                              />
                            </a>
                            <a
                              href="https://fedene.fr/mission/sn2e-2/"
                              class="mfm-logo-link"
                            >
                              <img
                                class="mfm-logo-img img-fluid"
                                src="https://fedene.fr/wp-content/uploads/2023/12/2020-SN2E_FEDENE-2-e1703864848538.png"
                                alt=""
                              />
                            </a>
                          </a>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </li>

            <li
              id="mn-menu-item-102"
              class="menu-item menu-item-type-custom menu-item-object-custom menu-item-has-children dropdown menu-item-102 nav-item"
            >
              <a
                href="#"
                aria-haspopup="true"
                aria-expanded="false"
                class="dropdown-toggle nav-link"
                id="66ddad055944f-menu-item-dropdown-102"
                data-bs-toggle="dropdown"
                ><span itemprop="name"><span>Missions</span></span></a
              >
              <ul
                class="dropdown-menu"
                aria-labelledby="66ddad055944f-menu-item-dropdown-102"
              >
                <li
                  id="mn-menu-item-1042"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1042 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/emploi-formation/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Emploi &amp; formation</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1043"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1043 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/prevention-et-securite/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Social – Prévention &amp; sécurité</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1047"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1047 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/efficacite-energetique/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Efficacité énergétique</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1044"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1044 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/reseaux-de-chaleur-et-de-froid/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Réseaux de chaleur et de froid</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1045"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1045 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/valorisation-energetique-des-dechets/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Valorisation énergétique des déchets</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-2389"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-2389 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/renouvalblesetrecup/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Énergies renouvelables et de récupération</span
                      ></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1046"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1046 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/solutions-numeriques/"
                    class="dropdown-item"
                    ><span itemprop="name"
                      ><span>Solutions numériques</span></span
                    ></a
                  >
                </li>
                <li
                  id="mn-menu-item-1669"
                  class="menu-item menu-item-type-post_type menu-item-object-mission menu-item-1669 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/mission/innovations/"
                    class="dropdown-item"
                    ><span itemprop="name"><span>Innovations</span></span></a
                  >
                </li>
              </ul>
            </li>
            <li
              id="mn-menu-item-104"
              class="menu-item menu-item-type-post_type menu-item-object-page menu-item-104 nav-item"
            >
              <a
                itemprop="url"
                href="https://fedene.fr/actualites/"
                class="nav-link"
                ><span itemprop="name"><span>Actualités</span></span></a
              >
            </li>
            <li
              id="mn-menu-item-105"
              class="menu-item menu-item-type-post_type menu-item-object-page menu-item-105 nav-item"
            >
              <a
                itemprop="url"
                href="https://fedene.fr/ressources/"
                class="nav-link"
                ><span itemprop="name"><span>Ressources</span></span></a
              >
            </li>
          </ul>
        </nav>
        <div class="d-flex align-items-center ms-auto">
          <nav class="navbar navbar-light ms-auto">
            <div
              class="social-nav social-nav-header d-none d-xl-block me-xl-4 pe-xl-3"
            >
              <div class="social-nav-list list-inline" role="tablist">
                <div class="list-inline-item" role="tab">
                  <a
                    href="https://fr.linkedin.com/company/fedene"
                    title=""
                    aria-label="Suivez-nous"
                    target="_blank"
                  >
                    <i class="fa-brands fa-linkedin" aria-hidden="true"></i>
                  </a>
                </div>
                <div class="list-inline-item" role="tab">
                  <a
                    href="https://twitter.com/_FEDENE_"
                    title=""
                    aria-label="Suivez-nous"
                    target="_blank"
                  >
                    <i class="fa-brands fa-x-twitter" aria-hidden="true"></i>
                  </a>
                </div>
                <div class="list-inline-item" role="tab">
                  <a
                    href="https://www.youtube.com/channel/UCiZp-mfl0aYTWoUUlsCYBQw"
                    title=""
                    aria-label="Suivez-nous"
                    target="_blank"
                  >
                    <i class="fa-brands fa-youtube" aria-hidden="true"></i>
                  </a>
                </div>
              </div>
            </div>
            <div class="d-none d-xl-block">
              <ul
                id="action-nav-66ddad055a458"
                class="nav navbar-nav flex-row main-aside-nav"
                itemscope=""
                itemtype="http://www.schema.org/SiteNavigationElement"
              >
                <li
                  id="an6-menu-item-109"
                  class="is-menu-underline menu-item menu-item-type-post_type menu-item-object-page menu-item-109 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/devenir-adherent/"
                    class="nav-link"
                    ><span itemprop="name"><span>Adhérer</span></span></a
                  >
                </li>
                <li
                  id="an6-menu-item-110"
                  class="menu-item menu-item-type-custom menu-item-object-custom menu-item-110 nav-item"
                >
                  <a
                    target="_blank"
                    rel="noopener noreferrer"
                    itemprop="url"
                    href="https://xrm.eudonet.com/specif/EUDO_06209/Extranet/Connexion.aspx"
                    class="btn btn-outline-primary"
                    ><span itemprop="name"><span>Extranet adhérents</span></span
                    ></a
                  >
                </li>
                <li
                  id="an6-menu-item-111"
                  class="menu-item menu-item-type-post_type menu-item-object-page menu-item-111 nav-item"
                >
                  <a
                    itemprop="url"
                    href="https://fedene.fr/contact/"
                    class="btn btn-primary"
                    ><span itemprop="name"><span>Contact</span></span></a
                  >
                </li>
              </ul>
            </div>
          </nav>
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
<slot />
