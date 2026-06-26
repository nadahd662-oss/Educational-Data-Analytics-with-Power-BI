# 📊 Tableau de Bord d'Analyse Pédagogique & Institutionnelle — Power BI

## 📝 Présentation du Projet
Ce projet consiste en la conception et le développement d'un tableau de bord décisionnel interactif sur **Power BI**, structuré en 3 pages analytiques complémentaires. L'objectif principal est de piloter la performance académique globale, d'analyser la charge et l'évaluation du corps enseignant, et d'optimiser l'allocation des ressources de l'établissement grâce à des insights orientés business.

---

## 🏗️ Architecture du Modèle de Données
Le projet s'appuie sur une modélisation optimisée en **schéma en étoile**, garantissant des performances de calcul fluides et une navigation intuitive.

*   **Table de Faits :** `courses` (contenant l'historique des notes, les heures prévues/réalisées et les statuts).
*   **Tables de Dimensions :** 
    *   `students` (Profils des élèves, niveaux scolaires, absentéisme).
    *   `teachers` (Données enseignants, types de contrats, évaluations).
    *   `DimDate` (Dimension temporelle pour l'analyse des années scolaires et semestres).

---

## 🚀 Fonctionnalités Majeures & Formules DAX Clés

### 1. Gestion des Relations Complexes et Filtrage Croisé
Pour surmonter les limitations des flèches de relation unidirectionnelles du modèle, la mesure clé du **Taux de Réussite** a été sécurisée en forçant un filtrage bidirectionnel temporaire dynamique :
```dax
Taux de Réussite = 
DIVIDE(
    CALCULATE(
        [Total Élèves],
        CROSSFILTER(courses[student_id], students[student_id], BOTH),
        courses[statut passage] = "Réussi"
    ),
    [Total Élèves],
    0
)
