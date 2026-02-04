# 📋 Outil de Préparation à la Transparence Salariale - Spécification Complète

**Version:** 3.0 | **Date:** Janvier 2025 | **Langue:** Français | **Statut:** Prêt pour implémentation

---

## Table des Matières

1. [Résumé Exécutif](#1-résumé-exécutif)
2. [Modèle de Données](#2-modèle-de-données)
3. [Paramètres Adaptatifs](#3-paramètres-adaptatifs)
4. [Logique de Création des Catégories](#4-logique-de-création-des-catégories)
5. [Phase 1 : Stratégie Initiale](#5-phase-1--stratégie-initiale)
6. [Phase 2 : Optimisation des Catégories](#6-phase-2--optimisation-des-catégories)
7. [Normalisation des Intitulés](#7-normalisation-des-intitulés)
8. [Gestion des Cas Isolés](#8-gestion-des-cas-isolés)
9. [Scoring et Qualification](#9-scoring-et-qualification)
10. [Tiers de Service](#10-tiers-de-service)
11. [Protection des Données (RGPD)](#11-protection-des-données-rgpd)
12. [Annexes](#12-annexes)

---

## 1. Résumé Exécutif

### 1.1 Vision Produit

Un outil SaaS en trois niveaux aidant les entreprises européennes (100+ employés) à se préparer à la Directive Européenne sur la Transparence Salariale (2024/970).

| Tier | Nom | Question Répondue | Psychologie |
|------|-----|-------------------|-------------|
| **1** | Diagnostic de Préparation | Suis-je prêt ? | Je sais que j'ai un problème et besoin d'un plan |
| **2** | Feuille de Route Prioritaire | Que dois-je changer ? | Je sais exactement quoi changer |
| **3** | Partenariat Conformité | Comment prouver ma conformité ? | Je suis confiant pour passer l'audit |

### 1.2 Proposition de Valeur Unique

> "En 5 minutes, notre algorithme produit une catégorisation qu'un RH expérimenté mettrait 2 semaines à construire manuellement - et probablement avec moins de rigueur méthodologique."

### 1.3 Objectif Principal

Créer automatiquement des **Catégories de Transparence** robustes pour l'analyse des écarts salariaux :

- **Comparabilité maximale** au sein de chaque catégorie
- **Taille optimale** (ni trop petit, ni trop grand)
- **Auditabilité complète** (chaque décision justifiable)

### 1.4 Contraintes Fondamentales

| Contrainte | Explication |
|------------|-------------|
| **Taille minimum 5 personnes** | Contrainte dure - RGPD + significativité statistique |
| **Taille maximum adaptative** | Fonction de la taille de l'entreprise (10% max) |
| **Travail de valeur égale** | Les catégories regroupent des postes comparables |
| **Interdiction d'utiliser le salaire** | Le salaire ne peut pas servir à créer les catégories |
| **Automatisation complète** | Système 100% automatisé sans intervention humaine |
| **Transparence et auditabilité** | Chaque décision doit être traçable et justifiable |
| **Minimiser les isolés** | Ne pas grouper à tout prix, mais exclure le moins possible |

---

## 2. Modèle de Données

### 2.1 Champs Obligatoires

| Champ | Description | Synonymes Détectés |
|-------|-------------|-------------------|
| **employee_id** | Identifiant unique | `Employee ID`, `ID`, `id_employe`, `matricule`, `EMP`, `emp_id`, `id_salarie`, `numero_employe` |
| **job_title** | Intitulé de poste | `Job Title`, `Poste`, `Title`, `Position`, `Fonction`, `Intitule`, `titre`, `intitule_poste`, `libelle_poste` |
| **department** | Département/Service | `Department`, `Dept`, `Service`, `Division`, `Departement`, `direction`, `pole`, `unite`, `bu`, `business_unit` |
| **coefficient** OU **grade** | Niveau hiérarchique | Voir détail ci-dessous |

#### 2.1.1 Coefficient (Prioritaire si présent)

| Synonymes Détectés | Format Attendu |
|-------------------|----------------|
| `coefficient`, `coeff`, `position`, `classification`, `indice`, `classe`, `echelon`, `niveau_convention`, `ccn_niveau` | Numérique (170, 210, 450) ou Texte parsable ("3.1", "Cadre Position II") |

#### 2.1.2 Grade (Utilisé si coefficient absent)

| Synonymes Détectés | Format Attendu |
|-------------------|----------------|
| `Grade`, `Band`, `Niveau`, `Level`, `Band/Grade`, `Grade Level`, `niveau_grade`, `career_level`, `job_level` | Texte (Junior, Confirmé, Senior, L1, L2, L3) |

### 2.2 Champs Optionnels

| Champ | Synonymes | Utilisation |
|-------|-----------|-------------|
| **manager_id** | `Manager ID`, `Reports_To`, `N+1`, `superviseur_id`, `responsable_id`, `id_manager`, `hierarchique` | Calcul Manager/IC |
| **gender** | `Gender`, `Sexe`, `Genre`, `M/F`, `H/F`, `civilite` | Analyse écart H/F |
| **salary** | `Salary`, `Salaire`, `Annual Salary`, `Compensation`, `Pay`, `remuneration`, `salaire_brut`, `salaire_annuel` | Aperçu écarts |
| **fte** | `FTE`, `ETP`, `Full Time Equivalent`, `temps_travail`, `quotite`, `taux_activite` | Ajustement salaire |
| **job_family** | `Job Family`, `Famille`, `Family`, `Metier`, `Métier`, `famille_professionnelle`, `filiere`, `domaine_metier` | Dimension alternative |
| **seniority** | `Seniority`, `Anciennete`, `Tenure`, `Years`, `annees_experience`, `date_entree` | Subdivision si nécessaire |

### 2.3 Calcul Automatique Manager/IC

Si `manager_id` est fourni, le système calcule automatiquement :

| Valeur | Condition |
|--------|-----------|
| **Manager = Oui** | Si `employee_id` apparaît dans la colonne `manager_id` d'au moins un autre employé |
| **Manager = Non (IC)** | Sinon (Individual Contributor) |

---

## 3. Paramètres Adaptatifs

### 3.1 Seuils de Taille des Catégories

Les seuils s'adaptent à la taille de l'entreprise pour maintenir des proportions cohérentes.

**Formule :**

| Seuil | Calcul | Description |
|-------|--------|-------------|
| **min** | 5 | TOUJOURS 5 - contrainte RGPD + statistique |
| **optimal_max** | max(30, effectif × 10%) | 10% de l'effectif, minimum 30 |
| **soft_max** | max(40, effectif × 12%) | 12% de l'effectif, minimum 40 |
| **hard_max** | max(50, effectif × 15%) | 15% de l'effectif - subdivision obligatoire |

**Exemples :**

| Effectif | min | optimal_max | soft_max | hard_max |
|----------|-----|-------------|----------|----------|
| 150 | 5 | 30 | 40 | 50 |
| 300 | 5 | 30 | 40 | 50 |
| 500 | 5 | 50 | 60 | 75 |
| 1000 | 5 | 100 | 120 | 150 |

### 3.2 Classification des Tailles

| Taille | Statut | Action |
|--------|--------|--------|
| < 5 | 🔴 TROP PETITE | FUSIONNER (priorité haute) |
| 5 - optimal_max | 🟢 OPTIMALE | CONSERVER tel quel |
| optimal_max - soft_max | 🟡 ACCEPTABLE | Subdivision recommandée si possible |
| soft_max - hard_max | 🟠 À SUBDIVISER | Subdivision fortement recommandée |
| > hard_max | 🔴 TROP GRANDE | SUBDIVISION OBLIGATOIRE |

**Note importante :** Même si une catégorie est dans la plage "acceptable", si elle peut être subdivisée en catégories de 5+ employés avec les dimensions disponibles (Job Family, Manager/IC, Titre Simplifié), cette subdivision est recommandée.

### 3.3 Bandes de Coefficients Automatiques

Quand le coefficient a une granularité trop fine (>15 valeurs uniques), créer automatiquement des bandes adaptées à la taille de l'entreprise.

**Logique :**
1. Calculer l'écart entre min et max coefficients
2. Nombre cible de bandes = max(3, min(15, effectif / 40))
3. Créer des intervalles équilibrés (multiples de 10)

**Exemples de résultats :**
- 200 employés, coefficients 170-500 → 5 bandes de ~66 points
- 500 employés, coefficients 100-600 → 12 bandes de ~50 points

### 3.4 Table des Départements Connexes

Pour les fusions de catégories trop petites, utiliser cette matrice de proximité enrichie.

| Groupe | Primary | Secondary | Tertiary | Extended |
|--------|---------|-----------|----------|----------|
| **tech** | it, informatique, dsi | data, analytics, bi | engineering, r&d, product | digital, innovation, cybersécurité |
| **business** | sales, commercial, ventes | marketing, communication | business development, partnerships | customer success, account management |
| **finance** | finance, direction financière | comptabilité, compta | audit, contrôle de gestion | trésorerie, fiscalité, risk |
| **support** | rh, ressources humaines | admin, services généraux | legal, juridique, compliance | facilities, achats, office |
| **operations** | operations, production | logistique, supply chain | qualité, qhse, sécurité | maintenance, technique, industriel |
| **direction** | direction générale, comex, codir | stratégie, transformation | secrétariat général, gouvernance | présidence, board, chairman |

---

## 4. Logique de Création des Catégories

### 4.1 Définition d'une Catégorie de Transparence

Une **Catégorie de Transparence** est créée par l'intersection de dimensions objectives :

**Formule de base :**
```
Catégorie = Niveau (Coefficient/Grade) × Dimension_Organisationnelle (Dept ou Job Family)
```

**Formule étendue (si subdivision nécessaire) :**
```
Catégorie = Niveau × Dimension_Org × [Manager/IC | Titre_Simplifié | Titre_Famille]
```

### 4.2 Score de Sélection de Dimension

Pour choisir entre Department et Job Family comme dimension organisationnelle :

**Formule :**
```
Score = (0.50 × % employés en 5-optimal_max) +
        (0.30 × (1 - % employés en <5)) +
        (0.20 × (1 - coefficient_variation_tailles))
```

La dimension avec le score le plus élevé est sélectionnée.

---

## 5. Phase 1 : Stratégie Initiale

### 5.1 Vue d'Ensemble

```
PHASE 1 : STRATÉGIE INITIALE
│
├── ÉTAPE 1.1 : Calculer seuils adaptatifs
│   └── Basé sur total_employees → min=5, optimal_max=10%
│
├── ÉTAPE 1.2 : Préparer le Niveau
│   ├── Détecter coefficient ou grade
│   ├── Si coefficient granulaire (>15 valeurs) → créer bandes adaptatives
│   └── Si aucun → inférer depuis intitulé (avec WARNING)
│
├── ÉTAPE 1.3 : Calculer Manager/IC
│   └── Si manager_id fourni → dériver is_manager
│
├── ÉTAPE 1.4 : Normaliser les Intitulés (2 niveaux)
│   ├── Niveau 1: Titre_Famille (très agrégé) - pour fusions
│   └── Niveau 2: Titre_Simplifié (moyennement agrégé) - pour subdivisions
│
├── ÉTAPE 1.5 : Choisir la Dimension Organisationnelle
│   ├── Calculer Score(Niveau × Department)
│   ├── Calculer Score(Niveau × Job_Family) si disponible
│   └── Choisir dimension avec meilleur score
│
└── ÉTAPE 1.6 : Créer Catégories Initiales
    ├── Appliquer formule: Niveau × Dimension_Choisie
    ├── Calculer tailles
    └── Classifier selon seuils adaptatifs
```

### 5.2 Inférence du Grade depuis l'Intitulé

| Mots-clés détectés | Grade Inféré |
|-------------------|--------------|
| `directeur`, `director`, `vp`, `vice president`, `c-level`, `chief`, `head of` | Directeur |
| `manager`, `responsable`, `chef de`, `lead`, `superviseur` | Manager |
| `senior`, `sr`, `principal`, `expert`, `confirmé` | Senior |
| (aucun préfixe spécifique) | Confirmé |
| `junior`, `jr`, `débutant`, `assistant`, `stagiaire`, `apprenti` | Junior |

---

## 6. Phase 2 : Optimisation des Catégories

### 6.1 Vue d'Ensemble

```
PHASE 2 : OPTIMISATION (max 3 itérations)
│
├── POUR chaque itération:
│   │
│   ├── ÉTAPE 2.1 : Traiter Catégories Trop Grandes
│   │   ├── Pour chaque catégorie > hard_max: subdivision obligatoire
│   │   └── Pour chaque catégorie > soft_max: subdivision recommandée
│   │
│   ├── ÉTAPE 2.2 : Traiter Catégories Trop Petites
│   │   └── Pour chaque catégorie < 5: tenter fusion (seuil min = 35)
│   │
│   ├── ÉTAPE 2.3 : Traiter Irréductibles
│   │   ├── Direction → catégorie spéciale si ≥5
│   │   └── Autres → conserver visibles avec warning
│   │
│   └── ÉTAPE 2.4 : Valider convergence
│
└── FIN PHASE 2
```

### 6.2 Ordre de Subdivision

Pour scinder une catégorie trop grande, évaluer chaque option et choisir celle avec le meilleur score de viabilité (% de sous-catégories ≥5 employés) :

1. **Job Family** (si non utilisé en Phase 1)
2. **Manager/IC** (si disponible)
3. **Titre Simplifié**
4. **Titre Famille**

Seuil de viabilité : ≥70% des sous-catégories doivent avoir ≥5 employés.

### 6.3 Matrice de Similarité pour Fusion

| Critère | Points | Logique |
|---------|--------|---------|
| **Même Département** | +50 | Même contexte organisationnel |
| **Départements primary/secondary** | +40 | Même groupe, niveaux proches |
| **Départements même groupe** | +25 | Connexes |
| **Départements tertiary/extended** | +15 | Même groupe, éloignés |
| **Départements sans lien** | 0 | Groupes différents |
| **Coefficient/Grade identique** | +40 | Même niveau = valeur égale |
| **Coefficient adjacent (±50 points)** | +30 | Niveaux proches |
| **Coefficient proche (±100 points)** | +15 | Défendable |
| **Coefficient éloigné** | -10 | Problématique |
| **Même statut Manager/IC** | +15 | Même responsabilité |
| **Statut mixte/inconnu** | 0 | Acceptable |
| **Statut opposé** | -15 | Problématique |
| **Même Titre Famille** | +20 | Même métier |
| **Titre Famille proche** | +10 | Métiers connexes |

### 6.4 Seuils de Décision pour Fusion

| Score | Action | Confiance |
|-------|--------|-----------|
| **≥ 65** | Fusion RECOMMANDÉE | ✅ Automatique |
| **50-64** | Fusion ACCEPTABLE | ⚠️ Avec note |
| **35-49** | Fusion RISQUÉE | ⚠️⚠️ Warning fort |
| **< 35** | Fusion DÉCONSEILLÉE | ❌ → Irréductible |

---

## 7. Normalisation des Intitulés

### 7.1 Deux Niveaux de Normalisation

**Niveau 1 : Titre_Famille (très agrégé)**
- Utilisé pour : fusion des catégories <5, identification du métier
- Exemples de familles : développeur, data, commercial, marketing, rh, finance, admin, technique, management, projet, juridique, qualité, logistique, production

**Niveau 2 : Titre_Simplifié (moyennement agrégé)**
- Utilisé pour : subdivision des catégories trop grandes
- Logique : Supprimer séniorité (junior/senior), garder fonction

### 7.2 Règles de Normalisation

1. Supprimer préfixes de séniorité (`junior`, `senior`, `lead`, `principal`)
2. Supprimer suffixes de niveau (`I`, `II`, `III`, `niveau 1`, `grade 2`)
3. Appliquer équivalences sémantiques (`software engineer` → `développeur`)
4. Normaliser casse et accents

---

## 8. Gestion des Cas Isolés

### 8.1 Philosophie

> "Ne pas grouper à tout prix, mais exclure le moins possible."

### 8.2 Classification des Irréductibles

| Type | Condition | Traitement |
|------|-----------|------------|
| **Direction** | Niveau ≥ Directeur OU coefficient ≥ 500 | Regrouper en "Direction & Executives" si total ≥ 5, sinon cas isolé |
| **Experts** | Intitulé contient: architecte, expert, specialist, research | Rester dans leur coefficient/grade (pas de regroupement spécial) |
| **Isolés purs** | Aucune fusion acceptable (score < 35) | Conserver visibles avec warning |

### 8.3 Règles de Regroupement

- **Direction** : Si ≥5 personnes de niveau Direction → créer catégorie spéciale "direction_executives"
- **Experts et autres** : Garder dans leur catégorie d'origine, même si < 5, avec un flag "isolated"

---

## 9. Scoring et Qualification

### 9.1 Méthodologie de Scoring (4 Composantes)

| Composante | Poids | Ce qu'elle mesure | État Optimal |
|------------|-------|-------------------|--------------|
| **Consolidation des Titres** | 25% | Taux de réduction après normalisation | Réduction > 60% |
| **Viabilité des Catégories** | 35% | % employés dans catégories de taille correcte | > 80% en optimal |
| **Efficience des Catégories** | 20% | Ratio employés/catégorie approprié | 10-25 employés/catégorie |
| **Taux d'Isolés** | 20% | % d'individus en catégories < 5 | < 5% isolés |

### 9.2 Formule de Viabilité

```
viability_score = (
    (optimal_employees / total_employees * 100) +
    (acceptable_employees / total_employees * 60)
)
```

### 9.3 Qualification du Score

| Score | Niveau | Message |
|-------|--------|---------|
| **80-100** | ✅ Prêt | Structure robuste, prête pour l'analyse de transparence |
| **60-79** | 🟡 Partiellement Prêt | Ajustements ciblés nécessaires |
| **40-59** | 🟠 En Développement | Restructuration significative recommandée |
| **< 40** | 🔴 Non Prêt | Refonte complète nécessaire |

---

## 10. Tiers de Service

### 10.1 Tier 1 : Diagnostic (GRATUIT)

- Score global /100
- Nombre de catégories créées
- Répartition par taille
- Teaser Tier 2

### 10.2 Tier 2 : Feuille de Route (600€ HT)

- Export Excel complet
- Table mapping titres
- Plan d'action priorisé
- Analyse genre (si données)

### 10.3 Tier 3 : Consulting

- Analyse écarts salariaux
- Point System
- Package audit-ready

---

## 11. Protection des Données (RGPD)

| Principe | Mise en Œuvre |
|----------|---------------|
| **Minimisation** | Seuls les champs nécessaires |
| **Limitation stockage** | Session uniquement |
| **Consentement** | Checkbox obligatoire |
| **Transparence** | Informations claires avant upload |

---

## 12. Annexes

### 12.1 Processus Complet

```
INPUT → PHASE 1 (Stratégie) → PHASE 2 (Optimisation) → SCORING → OUTPUT

PHASE 1:
- Seuils adaptatifs
- Coefficient banding
- Manager/IC
- Normalisation 2 niveaux
- Sélection dimension
- Catégories initiales

PHASE 2:
- Subdivision (Job Family/Manager/Titre)
- Fusion (matrice similarité, seuil 35)
- Irréductibles (Direction/Experts/Isolés)
- Validation convergence
```

---

**Document préparé pour:** Équipe de Développement  
**Dernière mise à jour:** Janvier 2025  
**Version:** 3.0  
**Statut:** Prêt pour implémentation
