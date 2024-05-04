# projetencadre gestion de stock
Realisé par OUYHIA AYOUB, OUAREZI YASSINE, MEZOUAR ABDERRAHMAN 
2APG6 EMSI MARRAKECH
## Rapport de Projet : Système de Gestion de Stock en C

### Objectif du Projet
Le projet vise à développer une application console en C pour la gestion de stock, permettant aux utilisateurs de gérer un inventaire de produits via une interface utilisateur en ligne de commande. L'application supporte la saisie, modification, suppression, recherche, tri, et enregistrement des données produit dans un fichier CSV.

### Conception du Système
L'application utilise une structure de données de type liste chaînée pour stocker les informations des produits dynamiquement. Chaque produit est représenté par un nœud dans la liste contenant divers attributs comme l'ID du produit, nom, description, utilisateur, prix, quantité, seuil d'alerte, et les dates d'entrée et de sortie de stock.

#### Structures de Données
- **Produit**: Structure contenant les détails du produit et un pointeur vers le produit suivant dans la liste chaînée.

#### Fonctionnalités Principales
- **Ajout de Produit**: Ajouter un nouveau produit à la liste.
- **Suppression de Produit**: Supprimer un produit existant de la liste.
- **Modification de Produit**: Mettre à jour les détails d'un produit existant.
- **Affichage des Produits**: Afficher tous les produits dans la liste.
- **Recherche de Produits**: Rechercher des produits par nom ou utilisateur.
- **Tri des Produits**: Trier les produits par nom ou prix unitaire.
- **Sauvegarde et Chargement**: Sauvegarder et charger les produits depuis un fichier CSV.

### Répartition des Tâches

#### Ouyhia Ayoub : Conception de Base et Gestion des Fichiers
- **Tâches**:
  - Conception des structures de données.
  - Implémentation de la fonction `chargerProduits` pour lire les produits depuis un fichier CSV.
  - Implémentation de la fonction `sauvegarderProduits` pour écrire les produits dans un fichier CSV.
- **Objectifs**:
  - Assurer que les produits peuvent être chargés et sauvegardés efficacement.
  - Maintenir l'intégrité des données lors des opérations de fichier.

#### Ouarhezi Yassine : Gestion des Produits et Recherche
- **Tâches**:
  - Implémentation des fonctions `ajouterProduit`, `supprimerProduit`, et `modifierProduit`.
  - Création des fonctions de recherche `rechercherParNom` et `rechercherParUtilisateur`.
- **Objectifs**:
  - Permettre une gestion fluide des produits dans l'inventaire.
  - Fournir des méthodes efficaces pour trouver des produits spécifiques.

#### Mezouar Abderrahman : Interface Utilisateur et Fonctions de Tri
- **Tâches**:
  - Développement de l'interface utilisateur dans la fonction `main`.
  - Implémentation des fonctions de tri `trierProduitsParNom` et `trierProduitsParPrix`.
- **Objectifs**:
  - Créer une interface conviviale pour interagir avec le système.
  - Assurer que les produits peuvent être triés correctement selon différents critères.

### Notes importantes:

1. **Gestion du Tableau pour le Tri**:
    - il faut probablement ajuster votre structure de données ou la méthode de tri. `qsort` ne peut pas être utilisé directement sur des listes chaînées comme suggéré dans l'exemple de code ci-dessus; cela nécessite un tableau ou un autre moyen de gestion des données. Il faut soit convertir votre liste chaînée en tableau pour le tri, soit implémenter un tri manuel pour les listes chaînées.

2. **Validation de l'Entrée**:
    - Assurons-nous e valider l'entrée utilisateur pour éviter les problèmes de sécurité comme les injections de format de chaîne.

3. **MAX_PRODUCTS**:
    - `MAX_PRODUCTS` dans `qsort` est utilisé hypothétiquement ici. Vous devrez déterminer le nombre de produits ou changer la méthode de tri en fonction de votre structure de données.

4. **Amélioration de l'Interface Utilisateur**:
    - Ajoutez des instructions claires pour l'utilisateur sur ce qu'il doit entrer et quand il doit entrer des données, et prévoyez une gestion pour les erreurs d'entrée utilisateur.

5. **Mémoire et Gestion de Ressources**:
    - il faut  gérer correctement les ressources, notamment en fermant les fichiers ouverts et en libérant toute la mémoire allouée pour éviter les fuites de mémoire, surtout avant de quitter l'application. 

Tache de dertermination de nombre d’utilisateurs: 

Pour déterminer le nombre de produits dans votre liste chaînée et gérer correctement les opérations telles que le tri qui dépendent de connaître ce nombre, vous aurez besoin d'une fonction pour calculer la taille de la liste. Vous pouvez ensuite utiliser cette information pour des opérations telles que la conversion de la liste en tableau pour le tri ou pour toute autre opération nécessitant la connaissance de la longueur de la liste.

Voici comment vous pourriez procéder :

### 1. Ajout de la fonction pour calculer la taille de la liste


// Fonction pour calculer la taille de la liste chaînée
int tailleListe(Produit *tete) {
    int taille = 0;
    Produit *courant = tete;
    while (courant != NULL) {
        taille++;
        courant = courant->suivant;
    }
    return taille;
}
```

### 2. Utilisation de la taille de la liste pour les opérations

Avec la fonction `tailleListe`, vous pouvez maintenant déterminer combien d'éléments sont présents dans votre liste à tout moment. Voici comment vous pouvez l'utiliser, par exemple, pour convertir votre liste en tableau si vous souhaitez utiliser `qsort` pour trier les produits :

```c
// Fonction pour convertir la liste chaînée en tableau
Produit **listeVersTableau(Produit *tete, int *taille) {
    int n = tailleListe(tete);
    *taille = n;  // Mettre à jour la taille pour la référence externe
    Produit **tableau = malloc(n * sizeof(Produit *));
    if (!tableau) return NULL;  // Gestion d'erreur en cas d'allocation échouée

    Produit *courant = tete;
    for (int i = 0; i < n; i++) {
        tableau[i] = courant;
        courant = courant->suivant;
    }
    return tableau;
}

// Fonction pour libérer le tableau de produits
void libererTableau(Produit **tableau) {
    free(tableau);
}
```

### 3. Exemple d'utilisation du tableau pour trier

Voici comment vous pouvez utiliser le tableau pour trier les produits par nom en utilisant `qsort` :

```c
void trierProduitsParNom(Produit **tete) {
    int taille;
    Produit **tableau = listeVersTableau(*tete, &taille);
    if (tableau) {
        qsort(tableau, taille, sizeof(Produit *), comparerParNom);
        // Réassembler la liste chaînée
        for (int i = 0; i < taille - 1; i++) {
            tableau[i]->suivant = tableau[i + 1];
        }
        tableau[taille - 1]->suivant = NULL;
        *tete = tableau[0];  // Mettre à jour la tête de la liste
        libererTableau(tableau);
    }
}
```

### 4. Notes finales

Ces modifications permettront de manipuler la liste chaînée de manière plus flexible, surtout en ce qui concerne les opérations de tri et toute autre fonction nécessitant de manipuler des données en bloc. Assurerons nous de gérer correctement la mémoire, en particulier avec les allocations dynamiques pour éviter les fuites de mémoire.

### Conclusion
Ce projet de gestion de stock est conçu pour être robuste, flexible et facile à utiliser. Chaque membre de l'équipe a un rôle clé dans la mise en œuvre de différentes facettes de l'application, garantissant ainsi que tous les aspects du système sont couverts. Les fonctionnalités implémentées permettront aux utilisateurs de gérer efficacement leurs stocks avec une variété d'outils utiles, le tout sauvegardé de manière fiable dans un format de fichier accessible.
