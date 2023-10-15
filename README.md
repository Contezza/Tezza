# Clone

Dit is een geclonede Repository van het origineel. Contezza ontwikkeld in Gitlab en 'nog' niet op Github.

# <img src="docs/src/docs/asciidoc/images/tezza_logo.png" width="300">

Tezza is een Contezza product dat gericht is op de ontwikkelingen rondom [Common Ground](https://www.vngrealisatie.nl/roadmap/common-ground). De werknaam was voorheen Groundwork en is nu Tezza geworden.

## Kickstart

Start op het hoofdproject `run.sh`. Hiermee worden alle docker images gestart en ingericht. Hiermee kun je snel lokaal de omgeving starten en testen. Zorg ervoor dat het volgende beschikbaar is op je development omgeving.

1. Maven 3.3.9.
2. Docker Desktop.
3. Docker is aangemeld op harbor.contezza.nl (`docker login harbor.contezza.nl`).
4. Docker network `dev_network` bestaat (`docker network create -d bridge dev_network`). Je kunt met `docker network ls` bekijken of netwerk is aangemaakt.
5. Zorg dat je ongeveer 10 GB geheugen hebt toegekend aan Docker Desktop.

Overzicht URL's:

- Tezza App: http://localhost:8082
- Vernietigings App: http://localhost:8083
- Alfresco Share: http://localhost:8081/share
- Alfresco Platform: http://localhost:8080/alfresco
- Activiti App: http://localhost:9080/activiti-app
- Activiti Admin: http://localhost:9090/activiti-admin
- Camunda: http://localhost:9091/camunda
- Open Zaak: http://localhost:8000
- Open Notificaties: http://localhost:8001
- Gemma Zaken Demo: http://localhost:8002
- Objecten API: http://localhost:8003
- Objecttypen API: http://localhost:8004

Voer eerst **setup** script uit (Tezza Console) voordat je gaat testen.

Indien al eerder gestart en om de laatste versie van de Tezza app te gebruiken, verwijder eerst de bestaande image:

```
docker rmi harbor.contezza.nl/apps/tezza:latest -f
```

## Tezza omgeving

Hieronder de verschillende omgevingen en URL's die beschikbaar zijn. De omgevingen worden via [docker](deployment/docker) opgeleverd. Lees de [handleiding](extensions/src/docs/asciidoc/includes/_introduction.adoc) voor een goed beeld van de inrichting.

| NAAM                                            | DEV                                       | DEMO                                       |
| ----------------------------------------------- | ----------------------------------------- | ------------------------------------------ |
| **Tezza Apps**                                  |
| Tezza App                                       | https://dev.tezza.eu/tezza                | https://demo.tezza.eu/tezza                |
| Gemeente Utrecht App                            | https://dev.tezza.eu/gemutr-tezza         | https://demo.tezza.eu/gemutr-tezza         |
| Gemeente Utrechtse Heuvelrug App                | https://dev.tezza.eu/gemuhr-tezza         | https://demo.tezza.eu/gemuhr-tezza         |
| Omgevingsdienst Noord-Holland Noord (ODNHN) App | https://dev.tezza.eu/odnhn-tezza          | https://demo.tezza.eu/odnhn-tezza          |
| **Contezza Apps**                               |
| Contezza Admin Tools                            | https://dev.tezza.eu/contezza-admin-tools | https://demo.tezza.eu/contezza-admin-tools |
| Contezza Migration                              | https://dev.tezza.eu/contezza-migration   | https://demo.tezza.eu/contezza-migration   |
| Contezza Teams App                              | https://dev.tezza.eu/contezza-teams       | https://demo.tezza.eu/contezza-teams       |
| **Akten Apps**                                  |
| Gemeente Utrechtse Heuvelrug App                | https://dev.tezza.eu/gemuhr-akten         | https://demo.tezza.eu/gemuhr-akten         |
| Gemeente Amsterdam App                          | https://dev.tezza.eu/gemams-akten         | https://demo.tezza.eu/gemams-akten         |
| **Alfresco Content Apps**                       |
| Contezza App                                    | https://dev.tezza.eu/contezza-aca         | https://demo.tezza.eu/contezza-aca         |
| Alfresco Content App                            | https://dev.tezza.eu/aca                  | https://demo.tezza.eu/aca                  |
| **Misc Apps**                                   |
| Process Workspace App (Alfresco)                | https://dev.tezza.eu/process-workspace    | https://demo.tezza.eu/process-workspace    |
| **Alfresco Content Services**                   |
| Alfresco Platform                               | https://dev.tezza.eu/alfresco             | https://demo.tezza.eu/alfresco             |
| Alfresco Share                                  | https://dev.tezza.eu/share                | https://demo.tezza.eu/share                |
| **Alfresco Process Services**                   |
| Activiti App                                    | https://dev.tezza.eu/activiti-app         | https://demo.tezza.eu/activiti-app         |
| Activiti Admin                                  | https://dev.tezza.eu/activiti-admin       | https://demo.tezza.eu/activiti-admin       |
| **Camunda**                                     |
| Camunda                                         | https://dev.tezza.eu/camunda              | https://demo.tezza.eu/camunda              |
| **VNG Realisatie**                              |
| Open Zaak                                       | https://dev-openzaak.tezza.eu             | https://demo-openzaak.tezza.eu             |
| Open Notificaties                               | https://dev-notificaties.tezza.eu         | https://demo-notificaties.tezza.eu         |
| Gemma Zaken Demo                                | https://dev-demo.tezza.eu                 | https://demo-demo.tezza.eu                 |
| **OpenGem**                                     |
| Objects API                                     | https://dev-objecten.tezza.eu             | https://demo-objecten.tezza.eu             |
| Objecttypes API                                 | https://dev-objecttypen.tezza.eu          | https://demo-objecttypen.tezza.eu          |
| **Gemeente Utrecht**                            |
| Backscan Archief                                | https://dev.tezza.eu/gemutr-bsa           | https://demo.tezza.eu/gemutr-bsa           |
| Digiplaza Rapporten                             | https://dev.tezza.eu/gemutr-reports       | https://demo.tezza.eu/gemutr-reports       |
| **Gemeente Amsterdam**                          |
| RM App                                          | https://dev.tezza.eu/gemams-rm            | https://demo.tezza.eu/gemams-rm            |

## Development

Gebruik onderstaande commando voor alle maven submodules.

```
clean install -Pdocker-build-start
```

Voer onderstaande script uit op hoofdproject en alle volumes worden verwijderd.

```
./purge.sh
```

### Open Zaak

Export data binnen de container.

```
// Dump alles
/app/src/manage.py dumpdata --natural-primary --natural-foreign --exclude corsheaders > openzaak.json

// Dump catalogi
/app/src/manage.py dumpdata catalogi > openzaak.json

// Dump config
/app/src/manage.py dumpdata vng_api_common authorizations notifications zgw_consumers > openzaak.json

// Kopieer naar eigen PC
docker cp <container id>:/app/openzaak.json .
```

### Docker

Handige docker commando's die tijdens development van pas komen.

```
# Stop alle containers
docker stop $(docker ps -a -q)

# Verwijder alle containers
docker rm $(docker ps -a -q)

# Verwijder alle volumes
docker volume prune

# Verwijder laatste app verie
docker rmi harbor.contezza.nl/apps/tezza:latest -f
```

## Versie

Deze module is gemaakt op Alfresco Enterprise 7.2.x en gebruikt ook SDK 4.4.

## Release

Release wordt uitgevoerd met Gitlab CI. Geef in commit bericht `create-fix-release` mee en er wordt een release :rocket: gemaakt.

Er is een wekelijke hot-fix release van Tezza en een maandelijkse minor-release.
De backend oftewel de Backend For Frontend volgt dezelfde release mechanisme als de frontend, maar het kan voorkomen dat er niet wekelijks altijd een release wordt gedaan.
Zie de [releasenotes](https://develop.contezza.io/products/tezza/) voor meer informatie.

In de Tezza app --> Profiel --> [App Info](https://develop.contezza.io/products/contezza-apps/tezza/#app-info) is de Tezza release incl. de sub-modules releases te zien en de backend release. 

## Documentatie

Documentatie is gemaakt in [asciidoc](docs/src/docs/asciidoc/index.adoc). Deze is ook te vinden op onze [Gitlab Pages](https://develop.contezza.io/products/tezza/).

## Maven

Gebruik onderstaande afhankelijkheden om module te gebruiken in een ander project.

```
<dependency>
  <groupId>nl.contezza.tezza</groupId>
  <artifactId>tezza-services-platform</artifactId>
  <version>[version]</version>
</dependency>

<dependency>
  <groupId>nl.contezza.tezza</groupId>
  <artifactId>tezza-services-share</artifactId>
  <version>[version]</version>
</dependency>
```

# License

Tezza wordt geleverd met een BSL 1.1 licentie.
Hieronder staat de informatie uit deze licentie

## Vanaf 1-5-2024 wordt Tezza volledig Open Source!
Zie bijgevoegd [Licentie bestand](LICENSE.md).
Tot die tijd kunnen klanten toegang krijgen tot onze Gitlab omgeving waar de code beschikbaar wordt gesteld.
Dit kan door een ticket in te schieten hier in github en dan nemen we contact op of via onze website https://contezza.nl/common-ground/

## Notice BSL

The Business Source License (this document, or the “License”) is not an Open Source license. However, the Licensed Work will eventually be made available under an Open Source License, as stated in this License.
