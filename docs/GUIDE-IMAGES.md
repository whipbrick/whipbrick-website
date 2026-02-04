# 📸 Guide Création Images - Whipbrick

## Images Requises

### 1. Photo Fondateur : `edward-peek.webp`

**Spécifications :**
- Format : WebP
- Dimensions : 400x400px (carré) ou 800x800px pour meilleure qualité
- Poids : < 100 KB
- Style : Photo professionnelle, fond neutre

**Étapes :**
1. Choisir une photo professionnelle
2. Recadrer en carré (1:1)
3. Convertir en WebP

**Outils en ligne :**
- https://cloudconvert.com/png-to-webp
- https://squoosh.app/ (Google)
- https://www.freeconvert.com/webp-converter

**Commande si vous avez ImageMagick :**
```bash
convert votre-photo.jpg -resize 800x800^ -gravity center -extent 800x800 -quality 85 edward-peek.webp
```

**Emplacement final :**
```
/assets/images/edward-peek.webp
```

---

### 2. Aperçu Outil : `transparency-tool-preview.webp`

**Spécifications :**
- Format : WebP
- Dimensions : 1200x800px (ratio 3:2) ou 900x600px
- Poids : < 150 KB
- Contenu : Screenshot de l'application transparency.whipbrick.com

**Options de création :**

#### Option A : Screenshot Direct
1. Aller sur https://transparency.whipbrick.com
2. Prendre un screenshot du dashboard principal
3. Flouter/anonymiser les données sensibles
4. Convertir en WebP

#### Option B : Mockup Figma/Canva
1. Créer un mockup avec des données factices
2. Montrer les graphiques clés :
   - Écarts salariaux homme/femme
   - Distribution par catégorie
   - Score de conformité
3. Export en WebP

#### Option C : Montage
Combiner plusieurs éléments :
- Interface d'upload
- Graphique principal
- Tableau de résultats
- Badge "100% Confidentiel"

**Éléments à inclure dans l'image :**
- Logo Whipbrick (petit, en coin)
- Titre : "Salary Transparency Analyzer"
- Visualisations de données
- Call-to-action visuel

**Emplacement final :**
```
/assets/images/transparency-tool-preview.webp
```

---

### 3. Diagramme Data Flow : `data-flow-diagram.webp`

**Spécifications :**
- Format : WebP
- Dimensions : 1200x600px environ
- Poids : < 100 KB

**Contenu suggéré :**
Diagramme montrant le flux :
```
[Sources de données] → [ETL] → [Stockage] → [BI/Analytics] → [Décisions]
```

**Outils de création :**
- Figma (https://figma.com) - Gratuit
- Canva (https://canva.com) - Templates disponibles
- Draw.io (https://app.diagrams.net/) - Open source
- PowerPoint/Google Slides → Export image

**Éléments visuels :**
- Icônes pour chaque étape
- Flèches directionnelles
- Couleurs cohérentes avec votre branding
- Texte lisible même en petit

**Emplacement final :**
```
/assets/images/data-flow-diagram.webp
```

---

## Conversion Rapide PNG/JPG → WebP

### Méthode 1 : En ligne (le plus simple)
1. Aller sur https://squoosh.app/
2. Uploader votre image
3. Choisir format "WebP" dans le panneau de droite
4. Ajuster la qualité (80-85% recommandé)
5. Télécharger

### Méthode 2 : Outil CLI (si installé)
```bash
# Installer cwebp (Ubuntu/Debian)
sudo apt-get install webp

# Convertir
cwebp -q 85 input.png -o output.webp

# Batch conversion
for file in *.png; do cwebp -q 85 "$file" -o "${file%.png}.webp"; done
```

### Méthode 3 : Python (si vous codez)
```python
from PIL import Image

img = Image.open("input.jpg")
img.save("output.webp", "webp", quality=85)
```

---

## Checklist Finale

Avant de déployer, vérifier :

- [ ] `edward-peek.webp` existe dans `/assets/images/`
- [ ] `transparency-tool-preview.webp` existe dans `/assets/images/`
- [ ] `data-flow-diagram.webp` existe dans `/assets/images/`
- [ ] Toutes les images < 150 KB
- [ ] Images optimisées pour web (qualité 80-85%)
- [ ] Pas de données sensibles visibles

---

## Alternatives Temporaires

Si vous n'avez pas encore les images, vous pouvez :

1. **Placeholder temporaire** : Utiliser des images de stock
2. **Commenter temporairement** : Cacher les sections d'images en attendant
3. **Image par défaut** : Logo Whipbrick en attendant

---

## Questions ?

Une fois les images créées :
1. Déposer dans `/assets/images/`
2. Me notifier
3. Je vérifierai que tout s'affiche correctement

**Note** : Les pages HTML sont déjà configurées pour utiliser ces images. Il suffit de les créer et les déposer au bon endroit.
