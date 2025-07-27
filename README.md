# 💧 Dashboard Home Assistant – Pompe Piscine

Ce dashboard Home Assistant permet de gérer automatiquement la pompe de piscine selon :
- La température de l’eau (manuelle ou capteur)
- La température extérieure
- La consommation solaire ou réseau
- Le nombre de turnovers journaliers souhaités

---

## 📦 Fonctionnalités
- 💦 Carte centrale cliquable pour afficher les réglages
- ☀️ Modes personnalisés : Été / TurnOver / Solaire
- 🌡 Capteurs d’eau, température extérieure, puissance conso
- 🔧 Réglages dynamiques selon les modes choisis
- ⚙️ Entièrement compatible avec Mushroom Cards + Card-mod

---

## 📸 Aperçu
![Dashboard piscine](screenshots/dashboard.png)

type: vertical-stack
cards:
  # 💧 Carte principale - Statut pompe piscine
  - type: custom:mushroom-template-card
    primary: 💧 Pompe Piscine - Statut & Infos
    secondary: 🕒 {{ now().strftime('%H:%M') }}
    icon: mdi:water-pump
    layout: vertical
    fill_container: true
    tap_action:
      action: call-service
      service: input_boolean.toggle
      service_data:
        entity_id: input_boolean.pompe_info_visible
    card_mod:
      style: |
        ha-card {
          background: rgba(33, 150, 243, 0.15);
          color: white;
          border-radius: 16px;
          box-shadow: 0 4px 12px rgba(0,0,0,0.25);
          padding: 20px;
          font-weight: 600;
          font-size: 1.2em;
          text-align: center;
          cursor: pointer;
          transition: background 0.3s ease;
        }
        ha-card:hover {
          background: linear-gradient(135deg, #1976d2, #1ca4d7);
        }

  # 🛠 Réglages filtration (affichés si input_boolean actif)
  - type: conditional
    conditions:
      - entity: input_boolean.pompe_info_visible
        state: "on"
    card:
      type: entities
      title: Réglages filtration
      entities:
        - entity: input_number.coefficient_filtration
          name: Coefficient de filtration
        - entity: input_number.turnovers
          name: Nb de turnovers/jour
        - entity: input_text.temperature_eau_manuel
          name: Température eau (manuelle)
        - entity: input_select.temperature_mode
          name: Température (mode)
        - type: section
          label: Réglages Mode Solaire
        - entity: input_number.seuil_surplus_solaire
          name: Seuil Surplus Solaire (W)
        - entity: input_number.seuil_conso_reseau
          name: Seuil Conso Réseau (W)
      card_mod:
        style: |
          ha-card {
            border-radius: 16px;
            margin-top: 10px;
            padding: 16px;
            background-color: #e3f2fd;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            opacity: 0;
            transform: translateY(-10px);
            animation: fadeSlideDown 0.4s ease-out forwards;
          }

          @keyframes fadeSlideDown {
            0% {
              opacity: 0;
              transform: translateY(-10px);
            }
            100% {
              opacity: 1;
              transform: translateY(0);
            }
          }

  # 🌡️ Affichage rapide des capteurs
  - type: glance
    show_name: true
    show_icon: true
    show_state: true
    title: null
    entities:
      - entity: sensor.temperature_eau
        name: Eau
      - entity: sensor.temperature_exterieure
        name: Extérieur
      - entity: sensor.pompe_piscine_puissance
        name: Conso
      - entity: switch.pompe_piscine
        name: Pompe
        icon: mdi:water-pump
    state_color: false
    theme: modern_dark

  # ⚙️ Choix du mode de filtration
  - type: custom:mushroom-entity-card
    entity: input_select.etats
    name: Mode filtration
    icon: mdi:settings
    layout: vertical
    fill_container: true
    card_mod:
      style: |
        ha-card {
          border-radius: 1em;
          background-color: rgba(255, 255, 255, 0.1);
        }

  # ☀️ Affichage spécifique au mode ÉTÉ
  - type: conditional
    conditions:
      - entity: input_select.etats
        state: Été
    card:
      type: horizontal-stack
      cards:
        - type: custom:mushroom-entity-card
          entity: sensor.temps_filtration_aujourdhui
          name: Temps filtration aujourd'hui
          icon: mdi:timer-sand
          layout: vertical
          fill_container: true
          card_mod:
            style: |
              ha-card {
                border-radius: 1em;
                background-color: rgba(255, 255, 255, 0.05);
              }
        - type: custom:mushroom-entity-card
          entity: sensor.temps_filtration_restant
          name: Temps restant filtration (h)
          icon: mdi:clock-outline
          layout: vertical
          fill_container: true
          card_mod:
            style: |
              ha-card {
                border-radius: 1em;
                background-color: rgba(255, 255, 255, 0.05);
              }

  # 🔄 Affichage spécifique au mode TurnOver
  - type: conditional
    conditions:
      - entity: input_select.etats
        state: TurnOver
    card:
      type: horizontal-stack
      cards:
        - type: custom:mushroom-entity-card
          entity: sensor.temps_filtration_aujourdhui
          name: Temps de filtration effectué
          icon: mdi:timer
          layout: vertical
          fill_container: true
          card_mod:
            style: |
              ha-card {
                border-radius: 1em;
                background-color: rgba(255, 255, 255, 0.05);
              }
        - type: custom:mushroom-entity-card
          entity: sensor.nombre_turnover_effectue
          name: Nombre de turnover effectué
          icon: mdi:counter
          layout: vertical
          fill_container: true
          card_mod:
            style: |
              ha-card {
                border-radius: 1em;
                background-color: rgba(255, 255, 255, 0.05);
              }
