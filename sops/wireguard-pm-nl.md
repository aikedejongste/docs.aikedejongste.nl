---
layout: default
title: SOP - Wireguard bewaren
parent: SOPs
---

# Wireguard configuratie veilig bewaren

Bewaar de inhoud van het `wg0.conf` of `jouw-naam.conf` bestandje op een veilige locatie.
Bijvoorbeeld in een password manager. In je mailbox of in de chat is geen veilige plaats.
De VPN configuratie is uniek voor jou en is lastig te resetten. Dus bewaar hem goed zodat
we geen nieuwe VPN config hoeven te maken als je een nieuwe computer hebt!

Laat het ook *direct* weten als je VPN configuratie is uitgelekt. Dan kunnen we de juiste
beveiligingsmaatregelen nemen.

## 1. Bestandje openen

Ga naar je `Downloads` map of de locatie waar het `wg0.conf` of `jouw-naam.conf`
bestandje is opgeslagen. Klik vervolgens rechts op het bestandje en kies
`openen met / open with` of `openen met Kladblok`.

<!-- markdown-link-check-disable -->
![Wireguard Leeg](/docs/assets/images/wireguard-open-with.png)

## 2. Eventueel programma kiezen

In de meeste gevallen mag je nu een programma uit een lijst kiezen om het bestandje
mee te openen. Kies `Notepad` / `Kladblok`.

![Wireguard Leeg](/docs/assets/images/wireguard-notepad.png)

## 3. Kopieer de inhoud

Selecteer alle text en kopier het naar je klembord.

## 4. Maak notitie in je password manager

Open je Password Manager (bijvoorbeeld Bitwarden of 1Password) en maak een nieuwe notitie.
Een notitie is handiger dan een 'login' omdat je hierbij meer ruimte hebt voor text.
Plak de tekst die we net gekopieerd hebben en sla de notitie op.

![Wireguard Open](/docs/assets/images/wireguard-pm.png)

## 5. Opruimen

Verwijder nu de email of het chatbericht waarin je de VPN configuratie hebt ontvangen. En
verwijder ook het bestandje uit je Downloads map.

## 6. Herstellen

Als je een nieuwe computer hebt of om een andere reden je VPN configuratie wilt
herstellen voer je bovenstaande acties uit maar dan met de laatste eerst. Dus:

1. ga naar je password manager en kopieer de inhoud van de notitie

2. open Kladblok en plak de inhoud

3. sla de tekst die nu in Kladblok staat op als `jouwnaam.conf`

4. volg de normale stappen voor het instellen van Wireguard: [Wireguard instellen](/sops/wireguard-nl.html).
<!-- markdown-link-check-enable -->
