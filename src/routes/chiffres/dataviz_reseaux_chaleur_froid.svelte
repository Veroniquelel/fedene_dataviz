<script>
  import { onMount } from 'svelte';
  import { onDestroy } from 'svelte';

  export let currentStep2;

  let start = 1; // Valeur initiale
  let heightPercentage = 0;
  let imgHeight = 0; // Hauteur de l'image
  let imgWidth = 0; // Hauteur de l'image
  let imgTopOffset = 0; // Position de l'image par rapport au haut de la page

  let margins = { top: 0, bottom: 0, left: 0, right: 0 };

  const updateMargins = () => {
    const img = document.querySelector('#reseau');
    const rect = img.getBoundingClientRect();
    margins.top = rect.height * 0.1;
    margins.bottom = rect.bottom * 0.1;
    margins.left = rect.left;
    margins.right = window.innerWidth - rect.right;
    imgWidth = img ? img.clientWidth : 0; // Récupération de la largeur de l'image avec vérification de null
    console.log(imgWidth);
    console.log(margins);
  };

  // Fonction pour ajuster la hauteur de la div en fonction du scroll
  const updateHeightPercentage = () => {
    const scrollPos = window.scrollY;

    if (scrollPos >= imgTopOffset && imgHeight > 0) {
      // Calculer le pourcentage uniquement lorsque le scroll dépasse l'image
      const relativeScroll = scrollPos - imgTopOffset;
      heightPercentage = Math.min(100, (relativeScroll / imgHeight) * 100);
    } else if (scrollPos < imgTopOffset) {
      // Réinitialiser la hauteur quand on revient au-dessus de l'image
      heightPercentage = 0;
    }

    console.log(heightPercentage);
  };

  onMount(() => {
    updateMargins(); // Calcul initial des marges

    window.addEventListener('resize', updateMargins); // Met à jour les marges lors du redimensionnement

    const img = document.querySelector('#reseau'); // Sélection de l'image

    // Fonction pour mettre à jour la hauteur de l'image et sa position
    const setImageDimensions = () => {
      imgHeight = img.clientHeight; // Récupération de la hauteur de l'image
      imgTopOffset = img.getBoundingClientRect().top + window.scrollY; // Position de l'image par rapport au haut de la page
      updateHeightPercentage(); // Mise à jour immédiate lors du chargement
    };

    // On appelle la fonction immédiatement au montage
    setImageDimensions();

    // Event listener pour recalculer si la fenêtre est redimensionnée
    window.addEventListener('resize', setImageDimensions);

    // Ajout de l'événement scroll
    window.addEventListener('scroll', updateHeightPercentage);

    // Nettoyage des listeners lors du démontage du composant
    return () => {
      window.removeEventListener('scroll', updateHeightPercentage);
      window.removeEventListener('resize', setImageDimensions);
    };
  });
</script>

<div
  style="display: flex; top:0; justify-content: center; align-items: center; position: relative; width: 100%; height: auto;"
>
  <!-- Image SVG responsive -->
  <img
    src="reseau_froid.svg"
    alt="Réseau froid"
    id="reseau"
    style="max-width: 100%; height: auto; object-fit: contain;"
  />

  <!-- Div rouge responsive -->
  <div
    style="background-color: #ED776E; position: absolute; top: {margins.top}px;   left: {margins.left}px; width: {imgWidth /
      2}px; z-index: -3; height: {heightPercentage + start}%;"
  ></div>

  <!-- Div bleue responsive -->
  <div
    style="background-color: #249DE1; position: absolute; top: {margins.top}px;  right: {margins.right}px; width: {imgWidth /
      2}px; z-index: -3; height: {heightPercentage + start}%;"
  ></div>
</div>
