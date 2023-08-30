---
layout: default
title: SOP - Toegang backups
parent: SOPs
---

# [email] Toegang backups

Onderwerp: Toegang tot backups

Beste <klant>,

Als onderdeel van het beheer van server ... zorg ik er ook voor dat er dagelijks backups gemaakt worden. Deze worden versleuteld en gecomprimeerd extern opgeslagen.

Ik vind het belangrijk dat je zelf, dus ook zonder mij, toegang hebt tot deze backups. Hieronder vind je een beknopte uitleg hoe je hierbij kunt. Je kunt deze uitleg ook gebruiken om zelf backups van de backups te maken.

De backups worden gemaakt met [Restic](https://restic.net/#introduction). En opgeslagen in een 'bucket' bij [Backblaze](https://www.backblaze.com/cloud-storage).

Je hebt alleen toegang tot de bucket met jouw backups en kunt niet inloggen in de webinterface bij Backblaze. Voor toegang heb je een 'id' en een 'key' nodig. Deze vind je de Bitwarden Send link onderaan deze e-mail. Dit is voldoende om een backup van de backup te maken.

De backup zelf is versleuteld door Restic en om daadwerkelijk bestanden uit de backup te kunnen halen heb je de naam van de repository en het wachtwoord ervan nodig. Deze staan ook in de Bitwarden Send.

Als je hulp nodig hebt hierbij en ik dat niet zelf kan doen kun je contact opnemen met [Constructors](https://constructors.nl/about/) of [LowVoice](https://www.lowvoice.nl).

Het enige dat nu belangrijk is om te doen is de inhoud van de Bitwarden Send veilig op te slaan. Bijvoorbeeld in een password manager. De Bitwarden Send link is 1 keer te openen.

Link:

Laat het gerust weten als je vragen hebt,

Met vriendelijke groet,

Aike
