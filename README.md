# NuxtSEO

# Mise en Place d'un SEO Performant avec Nuxt

## Introduction
L'optimisation pour les moteurs de recherche (SEO) est essentielle pour maximiser la visibilité de votre site web dans les résultats des moteurs de recherche. L'utilisation du framework Nuxt.js offre de nombreuses fonctionnalités et techniques pour améliorer votre SEO. Dans cet article, nous expliquerons comment mettre en place un SEO performant avec Nuxt et les avantages et inconvénients de chaque méthode.

## La Sitemap
Facilite la tâche des moteurs de recherche pour explorer et indexer votre site.  
Indique l'URL de chaque page, la date de dernière modification et la priorité d'indexation.  
Doit être maintenue à jour en cas de modifications fréquentes du contenu.

```
export default {
  sitemap: {
    path: '/sitemap.xml',
    hostname: 'https://www.votresite.com',
    routes() {
      // Générez ici la liste de toutes les URL de votre site
    },
  },
}
```
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
