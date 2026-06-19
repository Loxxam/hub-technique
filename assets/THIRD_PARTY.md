# Ressources tierces self-hostées

Ce dossier contient toutes les ressources tierces utilisées par les pages HTML du projet. Aucune dépendance runtime à un CDN public n'est tolérée.

## Polices — `./fonts/`

Téléchargées depuis Google Fonts le **2026-05-19**, servies localement via `./fonts.css`.

| Famille | Variantes | Source d'origine |
|---|---|---|
| Instrument Serif | regular + italic | `https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1` |
| Inter | 300, 400, 500, 600, 700 | `https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700` |
| JetBrains Mono | 400, 500, 600 | `https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600` |

Format : `woff2` uniquement (suffisant pour navigateurs 2020+).
Nombre de fichiers : 17 (un par range Unicode/poids).

## Procédure de mise à jour

```bash
# 1. Récupérer le CSS Google Fonts avec UA navigateur récent
curl -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15" \
  "https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500;600&display=swap" \
  -o fonts.css

# 2. Télécharger chaque .woff2 référencé dans fonts.css
# 3. Réécrire les URLs gstatic.com → URLs relatives dans fonts.css

# Note : opération à faire hors du réseau guest Desjoyaux (qui peut bloquer Google CDN).
```

## Pourquoi self-hosté

- **Réduction de surface IPS** : pas de requête vers un CDN public en runtime → pas de faux positif Fortinet sur les signatures `*.TLS.DoS`.
- **Fonctionnement hors-ligne** : les pages doivent rester lisibles sans connexion internet.
- **Souveraineté** : pas de logs de consultation côté CDN tiers.
- **Conformité RGPD** : pas de transfert d'IP utilisateur vers Google Fonts.
