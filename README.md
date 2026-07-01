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

## 📐 Structure du Rapport & Fonctionnalités des Pages

Le tableau de bord est structuré en **3 pages thématiques clés**, chacune répondant à un angle d'analyse décisionnel précis :

### 👤 Page 1 — Vue Élèves (Suivi de la Performance & Risques)
Cette page se concentre sur la trajectoire académique des étudiants et la détection précoce du décrochage.
*   **KPIs Principaux :** Volume total d'élèves inscrits et Taux de Réussite global (sécurisé via `CROSSFILTER` pour un calcul exact).
*   **Analyses Visuelles :** Corrélation dynamique entre le taux d'absentéisme et la chute des notes moyennes.
*   **Aide à la Décision :** Isolation et listage des profils d'élèves jugés "à risque" pour déclencher des actions de tutorat ciblées.

### 👨‍🏫 Page 2 — Vue Enseignants (Charge de Travail & Évaluations)
Conçue pour l'analyse des ressources humaines et de la répartition de la charge pédagogique.
*   **KPIs Principaux :** Total des enseignants actifs, moyenne des heures dispensées, ancienneté moyenne et note d'évaluation globale.
*   **Analyses Visuelles :** 
    *   Histogramme continu de distribution des performances (regroupement par *Bins* de 0,5).
    *   Graphique en barres 100% empilées croisant les matières enseignées et le statut contractuel (Titulaire, Contractuel, Vacataire).
*   **Aide à la Décision :** Tableaux dynamiques isolant le Top 5 et le Flop 5 des performances pour guider les plans de formation ou de redistribution des charges.

### 📚 Page 3 — Vue Cours (Suivi de l'Exécution Pédagogique)
Dédiée au suivi opérationnel des programmes et de la validation des modules.
*   **KPIs Principaux :** Taux de Réalisation Moyen du volume horaire (calculé proprement via le ratio des sommes d'heures prévues/réalisées pour éviter les biais de granularité).
*   **Analyses Visuelles :** Répartition des statuts de passage par matière et par semestre.
*   **Aide à la Décision :** Identification visuelle immédiate des matières affichant les taux d'échec les plus critiques (ex: Chimie, Littérature, Anglais) nécessitant des ajustements de planning ou des cours de soutien.

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




