# 💧 Dashboard Home Assistant - Pompe Piscine

Ce dashboard Home Assistant permet de gérer automatiquement la pompe de piscine selon :
- La température de l’eau (manuelle ou capteur)
- La température extérieure
- La consommation solaire / réseau
- Le nombre de turnovers journaliers souhaités

## 📦 Fonctionnalités
- 💦 Affichage du statut de la pompe + bouton toggle
- 📊 Réglages dynamiques selon les modes : Été, Hiver, Turnover, Solaire
- 🌡 Capteurs visibles (eau, extérieur, consommation)
- 🧠 Automatisation compatible avec les entités personnalisées


## 🛠 Installation

### 1. Prérequis
- [Mushroom Cards](https://github.com/piitaya/lovelace-mushroom)
- [Card-mod](https://github.com/thomasloven/lovelace-card-mod)

### 2. Ajouter la vue
1. Crée une vue en YAML dans Home Assistant
2. Copie-colle le contenu de `dashboard.yaml`

### 3. Crée les entités nécessaires
Tu dois créer les entités suivantes dans tes helpers :
- `input_boolean.pompe_info_visible`
- `input_number.coefficient_filtration`
- `input_number.turnovers`
- `input_text.temperature_eau_manuel`
- `input_select.temperature_mode`
- `input_number.seuil_surplus_solaire`
- `input_number.seuil_conso_reseau`
- `input_select.etats`
- `sensor.temps_pm_today_formatted`
- `sensor.temps_restant_filtration_piscine`
- `sensor.nombre_de_turnover_effectue`
- `sensor.0xa4c13801a76fffff_temperature` (temp eau)
- `sensor.0xa4c1383871aca185_temperature` (temp ext)
- `sensor.pompe_piscine_puissance`
- `switch.pompe_piscine`

## 👤 Auteur
Loïc – Maintenance & Smart Home Passionné  
📹 TikTok : lien à venir

