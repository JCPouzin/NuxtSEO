# NuxtSEO

# Mise en Place d'un SEO Performant avec Nuxt

## Introduction
L'optimisation pour les moteurs de recherche (SEO) est essentielle pour maximiser la visibilité de votre site web dans les résultats des moteurs de recherche. L'utilisation du framework Nuxt.js offre de nombreuses fonctionnalités et techniques pour améliorer votre SEO. Dans cet article, nous expliquerons comment mettre en place un SEO performant avec Nuxt et les avantages et inconvénients de chaque méthode.

## La Sitemap
**La Sitemap dans un Projet Nuxt pour un SEO Performant :**

La Sitemap facilite l'exploration et l'indexation du site par les moteurs de recherche en fournissant une liste organisée des URL, y compris la date de dernière modification et la priorité d'indexation.

```
//Pour utiliser axios dans votre projet : https://www.the-koi.com/projects/nuxt-3-how-to-use-axios/

export default {
  sitemap: {
    path: '/sitemap.xml', // Chemin où sera généré le fichier sitemap.xml
    hostname: 'https://www.votresite.com', // URL de base de votre site
    routes() {
      // Générez dynamiquement la liste des URL de votre site
      return axios.get('https://api.votresite.com/pages') // Exemple d'appel API
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

Dans cet exemple, nous utilisons Axios pour récupérer dynamiquement les données des pages de notre site depuis une API. Nous générons ensuite la liste des URL en spécifiant des détails tels que la fréquence de changement, la priorité d'indexation et la date de dernière modification pour chaque page.

Assurez-vous d'adapter ce code en fonction de votre propre structure de site et de la manière dont vous stockez vos URL. Le maintien à jour de cette Sitemap en cas de modifications fréquentes du contenu contribue à un SEO efficace.

# Métadonnées et Balises Meta
Aident les moteurs de recherche à comprendre le contenu de chaque page.  
Permettent de spécifier le titre, la description et d'autres informations importantes.  
Doivent être définies manuellement pour chaque page.
```
<template>
  <div>
    <h1>Titre de la page</h1>
  </div>
</template>

<script>
export default {
  head() {
    return {
      title: 'Titre de la page',
      meta: [
        { hid: 'description', name: 'description', content: 'Description de la page' },
        // Autres balises meta
      ],
    };
  },
};
</script>
```
# Données Structurées
Améliorent la compréhension du contenu par les moteurs de recherche.  
Peuvent entraîner l'affichage d'extraits enrichis dans les résultats de recherche.  
Nécessite de définir des balises structurées supplémentaires.

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
# Utilisation de `titleTemplate` pour un Titre Dynamique
Permet de personnaliser dynamiquement le titre de chaque page.  
Améliore la convivialité et la pertinence du titre.  
Requiert une configuration personnalisée.
```
<script setup>
const pageTitle = 'Titre de la page';
</script>

<script>
export default {
  head() {
    return {
      titleTemplate: (chunk) => {
        return chunk ? `${chunk} - ${pageTitle}` : pageTitle;
      },
    };
  },
};
</script>
```
# Balises du Corps
Permet d'inclure des scripts et des balises à la fin du corps pour une meilleure performance de chargement.  
Utile pour les scripts tiers et le suivi.  
Doit être utilisé avec précaution pour éviter des problèmes de performance.

```
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
          tagPosition: 'bodyClose',
        },
      ],
    };
  },
};
</script>
```
En utilisant ces techniques dans un projet Nuxt, vous pouvez optimiser votre SEO et améliorer la visibilité de votre site web dans les résultats de recherche. 
