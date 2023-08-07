---
layout: default
title: SOP - Wireguard instellen
parent: SOPs
---

# Wireguard instellen als gebruiker

## 1. Download en installeer de Wireguard client
<!-- markdown-link-check-disable -->

Deze kun je vinden op [https://www.wireguard.com/install](https://www.wireguard.com/install/)

## 2. Sla “wg0.conf” of "jouw-naam.conf" op in Downloads op je computer

Dit bestandje krijg je via e-mail of chat.

Je hoeft het niet te openen. De QR-code die bij de e-mail zit mag je negeren.

## 3. Open de Wireguard client en klik op “import tunnels”

![Wireguard Leeg](/docs/assets/images/wireguard-leeg.png)

## 4. Selecteer het wg0.conf of jouw-naam.conf bestandje

![Wireguard Open](/docs/assets/images/wireguard-open.png)

## 5. Activeer de tunnel

Hiermee start de verbinding, dit doe je elke keer als toegang wilt krijgen. Je hoeft de verbinding niet te verbreken als je klaar bent.

![Wireguard Activate](/docs/assets/images/wireguard-activate.png)

## 6. Als de verbinding actief is ziet dat er zo uit

Je kunt dit venster nu sluiten of minimaliseren

![Wireguard Actief](/docs/assets/images/wireguard-actief.png)

## 7. Bewaren VPN configuratie

Bewaar de inhoud van het `wg0.conf` of `jouw-naam.conf` bestandje op een veilige locatie.
Bijvoorbeeld in een password manager. In je mailbox of in de chat is geen veilige plaats.
De VPN configuratie is uniek voor jou en is lastig te resetten. Dus bewaar hem goed!

Laat het ook *direct* weten als je VPN configuratie is uitgelekt. Dan kunnen we de juiste
beveiligingsmaatregelen nemen.

Klik hier voor uitleg over het bewaren: [VPN config in Password Manager](/sops/wireguard-pm-nl.html).
<!-- markdown-link-check-enable -->
