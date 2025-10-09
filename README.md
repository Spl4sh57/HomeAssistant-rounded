# Mon dashboard
Voici mon dashboard Home Assistant pour smartphone.

<p align="left">
  <img src="img/Home-white.png" alt="Home white" width="250"/>
  <img src="img/Home-black.png" alt="Home black" width="250"/>
</p>

Je n’ai rien inventé, je m’inspire et je prends des idées que je modifie si besoin, mais pour résumer :
- Le thème principal est [**Rounded**](https://community.home-assistant.io/t/rounded-dashboard-guide/543043) de **Leon**, que j’ai très légèrement modifié.
- Pour compléter le thème, voir aussi les travaux de [**jimmy-landry**](https://https://github.com/jimmy-landry) et [**Tamper Evident**](https://www.youtube.com/@dontuseiftamperevident).
- La barre de navigation vient à la base de [**My Smart Home**](https://www.youtube.com/watch?v=q8spkVPQiL0).

Le dashboard est composé de 5 pages :

1. **Home** : la page principale, avec les informations importantes comme la température de la pièce de vie, son CO₂, et la possibilité de contrôler les lumières, prises, volets, portail extérieur. Les grosses icônes de couleur lancent des scènes.
2. **Consommation** : pour consulter la consommation électrique de la maison, et de plusieurs appareils.
3. **BambuLab** : permet de contrôler l’imprimante 3D et d’avoir un aperçu du print en cours.
4. **Eve** : pour contrôler l’aspirateur robot.
5. **Systèmes** : pour consulter des informations sur le Raspberry Pi ou encore le Synology.

## Cartes

#### Lumière :
<p align="left">
  <img src="img/btn-light.png">
</p>

<details><summary>Code</summary>

```yaml
type: custom:button-card
template: light_color
entity: light.rampe_led
icon: mdi:led-strip-variant
```

</details>

<details><summary>Template</summary>

```yaml
  light_color:
    icon: '[[[ return entity.attributes.icon ]]]'
    tap_action:
      action: toggle
      haptic: medium
    hold_action:
      action: more-info
      haptic: medium
    custom_fields:
      slider:
        card:
          type: custom:my-slider-v2
          entity: '[[[ return entity.entity_id ]]]'
          colorMode: brightness
          styles:
            container:
              - background: none
              - border-radius: 100px
              - overflow: visible
            card:
              - height: 16px
              - padding: 0 8px
              - background: |
                  [[[
                    if (entity.state == "on") return "linear-gradient(90deg, rgba(255,255,255, 0.3) 0%, rgba(255,255,255, 1) 100%)";
                    else return "var(--contrast4)";
                  ]]]
            track:
              - overflow: visible
              - background: none
            progress:
              - background: none
            thumb:
              - background: |
                  [[[
                    if (entity.state == "on") return "var(--black)";
                    if (entity.state == "off") return "var(--contrast20)";
                    else return "var(--contrast8)";
                  ]]]
              - top: 2px
              - right: '-8px'
              - height: 12px
              - width: 12px
              - border-radius: 10px
    styles:
      grid:
        - grid-template-areas: '"i" "n" "slider"'
        - grid-template-columns: 1fr
        - grid-template-rows: 1fr min-content min-content
      card:
        - background: var(--contrast2)
        - padding: 16px
        - '--mdc-ripple-press-opacity': 0
      img_cell:
        - justify-self: start
        - width: 24px
      icon:
        - width: 24px
        - height: 24px
        - color: var(--contrast8)
      name:
        - justify-self: start
        - font-size: 14px
        - margin: 4px 0 12px 0
        - color: var(--contrast8)
    state:
      - value: 'on'
        styles:
          card:
            - background: |
                [[[
                    var color = entity.attributes?.rgb_color;
                    if (entity.state != "on"){
                      return 'var(--contrast20)';
                    }
                    else if (color){
                      return 'rgba(' + color + ')'
                    }
                    else{
                      return 'var(--yellow)'
                    }
                ]]]
          icon:
            - color: var(--black)
          name:
            - color: var(--black)
      - value: 'off'
        styles:
          icon:
            - color: var(--contrast20)
          name:
            - color: var(--contrast20)
```

</details>

#### Groupe de lumières :
<p align="left">
  <img src="img/groupe-light.png">
</p>

<details><summary>Code</summary>

```yaml
type: custom:button-card
name: Pièces de vie
custom_fields:
  slider:
    card:
      type: custom:my-slider-v2
      entity: light.salon
      colorMode: brightness
      styles:
        container:
          - border-radius: 100px
          - overflow: visible
          - background: none
        card:
          - height: 40px
          - padding: 0 20px
          - background: var(--brightness)
        track:
          - overflow: visible
          - background: none
        progress:
          - background: none
        thumb:
          - background: var(--black)
          - top: 2px
          - right: "-18px"
          - height: 36px
          - width: 36px
          - border-radius: 100px
styles:
  grid:
    - grid-template-areas: "\"n\" \"slider\""
    - grid-template-columns: 1fr
    - grid-template-rows: 1fr min-content min-content
  card:
    - background: var(--brightness-tint)
    - padding: 16px
    - "--mdc-ripple-press-opacity": 0
  name:
    - justify-self: start
    - font-size: 14px
    - margin: 4px 0 12px 0
    - color: var(--contrast20)
```
</details>
