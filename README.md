# NuxtSEO

# Mise en Place d'un SEO Performant avec Nuxt

# Table des Matières

   1.1 [Introduction](#introduction)
   1.2 [La Sitemap](#la-sitemap)
   1.3 [Gestion des Méta-Tags](#gestion-des-méta-tags)
   1.4 [Données Structurées](#données-structurées)
   1.5 [Balises du Corps](#balises-du-corps)
   1.6 [Intégration de CSS Externe](#intégration-de-css-externe)
  
## Introduction
  
L'optimisation pour les moteurs de recherche (SEO) est essentielle pour maximiser la visibilité d'un site web dans les résultats des moteurs de recherche. L'utilisation du framework Nuxt offre de nombreuses fonctionnalités et techniques pour améliorer le SEO.
<div id='sitemap'/>  
  
## La Sitemap

La Sitemap facilite l'exploration et l'indexation du site par les moteurs de recherche en fournissant une liste organisée des URL, y compris la date de dernière modification et la priorité d'indexation.

```
//Pour utiliser axios dans le projet :https://github.com/axios/axios,  https://www.the-koi.com/projects/nuxt-3-how-to-use-axios/

export default {
  sitemap: {
    path: '/sitemap.xml', // Chemin où sera généré le fichier sitemap.xml
    hostname: 'https://www.site.com', // URL de base du site
    routes() {
      // Générer dynamiquement la liste des URL du site
      return axios.get('https://api.vsite.com/pages') // Exemple d'appel API
        .then((response) => {
          const pages = response.data;
          return pages.map((page) => ({
            url: page.url,
            changefreq: 'daily', // Fréquence de changement de la page
            priority: 0.7, // Priorité d'indexation (entre 0 et 1)
            lastmodISO: page.lastModified, // Date de dernière modification
          }));
        })
        .catch((error) => {
          console.error('Erreur lors de la récupération des URL :', error);
        });
    },
  },
}
```

Dans cet exemple, j'utilise Axios pour récupérer dynamiquement les données des pages de notre site depuis une API. J'ajoute ensuite la liste des URL en spécifiant des détails tels que la fréquence de changement, la priorité d'indexation et la date de dernière modification pour chaque page.
Il faut s'assurer d'adapter ce code en fonction de la structure de site et de la manière dont est stocké les URL. La mise à jour de cette Sitemap en cas de modifications fréquentes du contenu contribue à un SEO efficace.


# Gestion des Méta-Tags 

La gestion des méta-tags permet de personnaliser dynamiquement le titre, la description et d'autres informations pertinentes pour chaque page, améliorant ainsi la visibilité dans les résultats de recherche.

```
<template>
  <div>
    <h1>Titre de la page</h1>
  </div>
</template>

<script>
export default {
  meta: [
    { hid: 'og:title', property: 'og:title', content: 'Titre de la page' },
    { hid: 'twitter:title', name: 'twitter:title', content: 'Titre de la page' },
    { hid: 'description', name: 'description', content: 'Description de la page' },
    { name: 'keywords', content: 'mots-clés, SEO' },
    { name: 'author', content: 'Nom de l'auteur' },
    // On peut ajouter d'autres balises méta au besoin
  ],
};
</script>
```
Dans cet exemple, je personalise dynamiquement le titre de la page et je définie des méta-tags tels que la description, les mots-clés et l'auteur pour améliorer le SEO et la visibilité dans les résultats de recherche. Chaque page nécessite une définition manuelle de ces méta-tags pour garantir un SEO optimal.


# Données Structurées :

Les données structurées améliorent la compréhension du contenu par les moteurs de recherche, ce qui peut entraîner l'affichage d'extraits enrichis dans les résultats de recherche, améliorant ainsi la visibilité.

```
<script setup>
const eventData = {
  name: 'Nom de l'événement',
  startDate: '2023-10-30',
  location: 'Lieu de l'événement',
};
</script>

<script>
export default {
  head() {
    return {
      structuredData: {
        '@context': 'http://schema.org',
        '@type': 'Event',
        name: eventData.name,
        startDate: eventData.startDate,
        location: eventData.location,
      },
    };
  },
};
</script>
```
Dans cet exemple, je définie des données structurées pour un événement en utilisant le format JSON-LD, qui suit le schéma de balisage de schema.org. Les données structurées aident les moteurs de recherche à comprendre que l'objet en question est un événement.
Les données structurées peuvent également être utilisées pour d'autres types d'objets. Chaque type d'objet aura des balises structurées spécifiques à définir. Cependant, il est important de noter que la définition de données structurées nécessite une connaissance préalable des schémas de balisage appropriés.


# Balises du Corps :

Les balises du corps permettent d'inclure des scripts et des balises à la fin du corps de la page, améliorant ainsi la performance de chargement. Elles sont utiles pour intégrer des scripts tiers et des outils de suivi.

```
<template>
  <div>
    <h1>Ma Page</h1>
    <!-- contenu de page -->
  </div>
</template>
<script setup>
const thirdPartyScriptURL = 'https://third-party-script.com';
</script>

<script>
export default {
  head() {
    return {
      script: [
        {
          src: thirdPartyScriptURL,
          async: true, // Définir async si le script peut être chargé de manière asynchrone
          defer: true, // Définir defer si le script peut être différé pour améliorer la performance
          crossorigin: 'anonymous', // Pour les scripts externes, favorisez la sécurité
          type: 'text/javascript', // Type du script (peut être omis)
          tagPosition: 'bodyClose', // Ajout du script à la fin du corps
        },
      ],
    };
  },
};
</script>
```

Dans cet exemple, j'utilise `tagPosition: 'bodyClose'` pour indiquer que le script `thirdPartyScriptURL` doit être placé à la fin du corps de la page. Cela améliore la performance de chargement de la page, car le script est chargé après le contenu principal.

Cependant, il est essentiel d'utiliser cette technique avec précaution. Trop de scripts tiers ou mal placés peuvent entraîner des problèmes de performance, car ils peuvent bloquer le rendu de la page ou ralentir le chargement. Il est recommandé de ne charger que les scripts nécessaires et de s'assurer qu'ils sont correctement configurés pour ne pas perturber l'expérience de l'utilisateur.
En utilisant ces techniques dans un projet Nuxt, vous pouvez optimiser votre SEO et améliorer la visibilité de votre site web dans les résultats de recherche.

  
# Intégration de CSS Externe :

L'intégration de CSS externe permet d'appliquer des styles externes à votre site, comme les polices Google Fonts, améliorant l'apparence de votre site.

```
<template>
  <div>
    <h1>Ma Page avec des Polices Google Fonts</h1>
    <p>Ceci est un exemple d'utilisation de polices Google Fonts.</p>
  </div>
</template>

<script setup lang="ts">
useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com',
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: '',
    }
  ]
})
</script>
```

Dans cet exemple, j'active les polices Google Fonts en utilisant le composant `useHead`. J'ajoute deux balises `link` :

- La première balise `link` avec `rel: 'preconnect'` établit une connexion préalable au serveur de polices pour améliorer la performance du chargement.
- La deuxième balise `link` avec `rel: 'stylesheet'` charge les polices Google Fonts.

L'attribut `crossorigin: ''` indique que le chargement est anonyme.

L'intégration de CSS externe améliore la conception du site en lui ajoutant des styles externes tout en maintenant un bon niveau de performance.
