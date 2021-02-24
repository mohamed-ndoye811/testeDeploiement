<script>
  import { gsap, Power4 } from "gsap";

  //---[ IMPORT DES PAGES ]---
  import Equipe from "./pages/Equipe.svelte";
  import InfoImportante from "./components/InfoImportante.svelte";
  import Accueil from "./pages/Accueil.svelte";
  import PlanningsSelection from "./pages/PlanningsSelection.svelte";
  import Bsv from "./pages/Bsv.svelte";
  import Administration from "./pages/Administration.svelte";
  import Activites from "./pages/Activites.svelte";
  import CompteurVisite from "./components/CompteurVisite.svelte";

  // Definission variable de page selectionnée
  let pageSelected = "";

  // On la définie en "/" pour être sur l'acceuil lors du chargement du site
  window.addEventListener("load", function () {
    pageSelected = "/";
  });

  // Variable de la page actuelle affichée
  let actualPage;

  // Fonction de changement de page, qui fait disparaitre celle présente pour laisser apparaitre la nouvelle
  function pageSwitch(nextPage) {
    // Animation de disparition
    gsap.to(actualPage, {
      autoAlpha: 0,
      duration: 0.6,
      ease: Power4.easeOut,
    });

    // Réapparition après un délai
    setTimeout(function () {
      gsap.to(actualPage, {
        autoAlpha: 1,
        duration: 0.6,
        ease: Power4.easeOut,
      });
      pageSelected = nextPage;
    }, 200);
  }

  // Définition des variables pour la recherche des statuts et du réglement
  let pannelVisible = false;
  let adminChoix = "";

  // Gestion du changement de page pour l'administration
  function redirectionAdmin(choix) {
    pageSwitch("/Administration");
    setTimeout(function () {
      adminChoix = choix.toLowerCase();
    }, 200);

    pannelVisible = false;
  }
</script>

<header>
  <img
    src="./img/LSR_LOGO/LSR83_logo.png"
    alt=""
    id="headerLogo"
    on:click={() => pageSwitch("/")}
  />
  <ul class="nav">
    <li class="nav__link" on:click={() => pageSwitch("/Equipe")}>ÉQUIPE</li>
    <li class="nav__link" on:click={() => pageSwitch("/Activites")}>
      ACTIVITÉS
    </li>
    <li class="nav__link" on:click={() => pageSwitch("/BSV")}>BSV</li>
    <li class="nav__link" on:click={() => pageSwitch("/Planning")}>
      PLANNINGS
    </li>
    <li
      class="nav__link"
      on:click={() => {
        pannelVisible = !pannelVisible;
      }}
    >
      ADMINISTRATION
    </li>
  </ul>

  <div class="adminPannel" class:pannelVisible>
    <p on:click={() => redirectionAdmin("Statuts")}>STATUTS</p>
    <p on:click={() => redirectionAdmin("Reglement")}>REGLEMENT</p>
  </div>
</header>

<InfoImportante />

<div class="container" bind:this={actualPage}>
  {#if pageSelected === "/"}
    <Accueil />
  {:else if pageSelected === "/Equipe"}
    <Equipe />
  {:else if pageSelected === "/Activites"}
    <Activites />
  {:else if pageSelected === "/BSV"}
    <Bsv />
  {:else if pageSelected === "/Planning"}
    <PlanningsSelection />
  {:else if pageSelected === "/Administration"}
    <Administration textToShow={adminChoix} />
  {/if}
</div>

<div class:hidden={pageSelected != "/"}>
  <CompteurVisite />
</div>

<style type="text/scss">
  @import "./globals.scss";

  

  .container {
    max-width: 75%;
    height: 100%;
    position: relative;
    top: 15px;
    left: 50%;
    transform: translateX(-50%);
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 75%;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
    z-index: 2;

    & #headerLogo {
      height: 4vh;
      cursor: pointer;
    }

    .nav {
      list-style-type: none;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      width: 80%;

      &__link {
        color: $secondary-color-100;
        text-decoration: none;
        font-weight: 600;
        font-size: 1.9vw;
        cursor: pointer;
      }
    }

    .adminPannel {
      visibility: hidden;
      background-color: $secondary-color-100;
      color: $primary-color;
      position: absolute;
      right: 0;
      top: 70%;
      z-index: 10;
      width: 100x;
      height: 20px;
      box-shadow: 0 0 8px rgba(0, 0, 0, 0.5);
      padding: 20px;
      border-radius: 10px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;

      p {
        font-size: 2.5vw;
        font-weight: 600;
        cursor: pointer;
      }
    }

    .pannelVisible {
      visibility: visible;
    }
  }

  .hidden {
    visibility: hidden;
  }

  @media (min-width: 768px) {
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      max-width: 75%;
      position: relative;
      left: 50%;
      transform: translateX(-50%);
      z-index: 2;

      & #headerLogo {
        height: 8vh;
        cursor: pointer;
      }
    }
  }
</style>
