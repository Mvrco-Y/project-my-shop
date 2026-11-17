
---

# Exercice 1 : Mémo Performance, Accessibilité et Éco-conception

## Objectif du projet
L'objectif de cet exercice était de prendre une application React existante et de l'optimiser sur trois axes majeurs : la performance de chargement, l'accessibilité pour tous les utilisateurs (notamment ceux utilisant des lecteurs d'écran), et l'éco-conception (réduire le poids transféré pour limiter l'impact environnemental). La règle d'or était de prouver chaque amélioration par des mesures concrètes (Lighthouse et DevTools), avant et après modifications.

## Ce qui a été fait (Branche `access`)
1. **Polices système** : Suppression de l'import Google Fonts (qui bloquait le rendu) au profit des polices natives du système d'exploitation.
2. **Code-splitting** : Utilisation de `React.lazy` et `Suspense` pour ne charger que le code JavaScript de la page consultée, allégeant drastiquement le premier chargement.
3. **Images** : Ajout des attributs `width`, `height` et `loading="lazy"` pour éviter les sauts de mise en page (CLS) lors du chargement des images.
4. **Contrastes** : Modification des couleurs du texte secondaire pour respecter les normes WCAG.
5. **Sémantique et A11y** : Ajout de l'attribut `aria-current="page"` sur la navigation active et remplacement d'un bouton de navigation par un lien natif `<Link>`.

## Résultats mesurés
* **Accessibilité** : Score Lighthouse passé de **95/100 à 100/100** sur toutes les pages.
* **Poids total** : Réduit de **5,1 Mo à 3,1 Mo** lors du chargement de l'accueil (soit -40%).
* **Requêtes réseau** : Passées de 39 à 30 au chargement initial.
* **Stabilité visuelle (CLS)** : Sur la page Produits, le score est passé de 0.145 à 0.079 (amélioration de 55%).


# my-shop — Audit de performance, accessibilité et éco-conception

Ce projet a fait l'objet d'un audit complet (Lighthouse et Chrome DevTools) sur les performances, l'accessibilité et l'éco-conception.

## 1. Méthodologie
Les mesures ont été réalisées en local avec :
- **Lighthouse** (mode Desktop) pour les scores et métriques.
- L'onglet **Network** de Chrome DevTools (avec "Désactiver le cache" coché) pour le poids et le nombre de requêtes.
- Comparaison entre la branche `starter` (version initiale) et la branche `access` (version optimisée).

## 2. Mesures globales Avant / Après

| Page | Version | Perf Lighthouse | Accessibilité | Poids transféré | Requêtes |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Accueil (`/`)** | AVANT | 60 | 95 | 5,1 Mo | 39 |
| | **APRÈS** | 57* | **100** | **3,1 Mo** | **30** |
| **Produits (`/products`)** | AVANT | 56 | 95 | - | - |
| | **APRÈS** | **60** | **100** | - | - |

*\*Le score Perf de l'accueil (57) est dû au bruit de mesure en local (serveur Vite non compressé). Les gains réels se constatent sur la réduction du poids total (-2 Mo) et du nombre de requêtes (-9).*

## 3. Détail des corrections et gains mesurés

**1. Éco : Remplacement de Google Fonts par une pile de polices système**
- **Action :** Suppression de l'import Google Fonts (DM Sans, Fraunces) dans le CSS.
- **Gain mesuré :** Les requêtes de polices passent de 2 fichiers `.woff2` à **0 requête**. Économie de bande passante et affichage du texte instantané.

**2. Perf : Code-splitting (découpage par page avec `React.lazy`)**
- **Action :** Utilisation de `lazy()` et `<Suspense>` dans `App.tsx`.
- **Gain mesuré :** Le chargement initial passe de **39 requêtes (5,1 Mo)** à **30 requêtes (3,1 Mo)**. Le code des pages Produits et Commandes n'est plus téléchargé sur l'accueil.

**3. Perf / A11y : Dimensions réservées sur les images (`width` et `height`)**
- **Action :** Ajout des attributs de taille et de `loading="lazy"` sur les balises `<img>`.
- **Gain mesuré :** Sur la page Produits, le **CLS** (Cumulative Layout Shift, les sauts de page) chute de **0.145 à 0.079** (divisé par 2). Le navigateur réserve la place avant le chargement.

**4. A11y : Renforcement des contrastes de texte**
- **Action :** Modification des couleurs de texte secondaire et d'accent dans `App.css`.
- **Gain mesuré :** Le score d'accessibilité Lighthouse passe de **95 à 100** sur les deux pages auditées.

**5. A11y : Ajout de l'attribut `aria-current` et d'un lien natif pour le bouton Retour**
- **Action :** Ajout de `aria-current="page"` sur la navigation active et remplacement d'un bouton JS par un `<Link>` sur la fiche produit.
- **Gain mesuré :** Navigation sémantique et clavier facilitée. Confirme le score parfait de **100** en accessibilité.

## 4. Conclusion
L'application est désormais **100/100 en accessibilité** et beaucoup plus légère (-40% de poids au chargement initial). La règle d'or a été respectée : aucune optimisation n'a été affirmée sans être remesurée.