# Logos Clients - Guide d'Utilisation

## 📁 Format des Logos

**Format recommandé :** PNG avec fond transparent

**Spécifications techniques :**
- **Hauteur :** 60-80px (la largeur s'ajuste automatiquement)
- **Résolution :** 2x pour écrans Retina (120-160px de hauteur réelle, affichée à 60-80px)
- **Couleur :** Original (le site applique automatiquement un filtre grayscale au repos)
- **Fond :** Transparent (.png)

## 🎨 Effet Visuel

Les logos s'affichent en **noir & blanc** (grayscale) par défaut, et passent en **couleur au survol** pour un effet élégant et professionnel.

## 🏢 Logos à Ajouter (d'après tes témoignages)

Télécharge les logos officiels depuis les sites des entreprises ou leurs kits presse :

1. **AXA XL** → `axa-xl.png`
   - Kit presse : https://www.axaxl.com/newsroom

2. **EY (Ernst & Young)** → `ey.png`
   - Kit presse : https://www.ey.com/en_gl/newsroom/media-resources

3. **La Vie Foods** → `la-vie-foods.png`
   - Site web ou contact direct pour logo

4. **MBI Health** → `mbi-health.png`
   - Site web ou contact direct pour logo

## 🔧 Comment Ajouter un Logo

1. Télécharge le logo en PNG haute résolution
2. Place-le dans le dossier `/assets/images/clients/`
3. Décommente la ligne correspondante dans `index.html` (ligne ~615)
4. Remplace `src="assets/images/clients/ton-logo.png"` par le bon nom de fichier

## 📝 Code à Décommenter dans index.html

Cherche les lignes commentées (<!-- ... -->) dans la section "Ils me font confiance" et décommente-les :

```html
<img src="assets/images/clients/axa-xl.png" alt="AXA XL" style="height: 60px; width: auto; filter: grayscale(100%); opacity: 0.7; transition: all 0.3s ease;" onmouseover="this.style.filter='grayscale(0%)'; this.style.opacity='1';" onmouseout="this.style.filter='grayscale(100%)'; this.style.opacity='0.7';">
```

## ✅ Vérification

Après ajout :
1. Ouvre `index.html` dans un navigateur
2. Scrolle jusqu'à "Ils me font confiance"
3. Vérifie que les logos s'affichent en noir & blanc
4. Survole les logos → ils doivent passer en couleur

## 🚀 Optimisation

Pour de meilleures performances, utilise un outil comme [TinyPNG](https://tinypng.com/) pour compresser tes logos avant de les ajouter.
