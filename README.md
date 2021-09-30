# Linux Containers (LXC)

Linux containers zijn een technologie die het installeren en uitvoeren van programma's vereenvoudigt. Zoals de naam doet vermoeden is een linux container een geïsoleerde omgeving waarin één of meerdere linux processen op gecontroleerde wijze kunnen uitvoeren. Een linux container voorziet namelijk alle bestanden, libraries en programma's die nodig zijn om de processen in de container te ondersteunen.

De oefenzitting gaat voornamelijk over het *gebruik* van Linux containers (verder gewoon containers genoemd). Containers worden aangemaakt op basis van een *image* die alle informatie bevat om de container te starten. Je kan dit vergelijken met een *image* van een Virtuele Machine of de binaire code van een regulier programma. Bestaande images voor populaire programma's en systemen zijn te downloaden van zowel publieke als privé marktplaatsen, ook wel container registries genoemd, zoals [dockerhub](https://hub.docker.com/), [quay.io](https://quay.io/) en de [github](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) en [gitlab](https://about.gitlab.com/blog/2016/05/23/gitlab-container-registry/) container registries. Daarnaast is het ook mogelijk om zelf container images aan te maken, maar dit valt buiten het bestek van deze oefenzitting. Geïnteresseerden kunnen meer informatie over containers en images terug vinden onderaan dit document bij de referenties.

In deze oefenzitting voorzien we ondersteuning voor twee linux container managers, afhankelijk van je werkomgeving. Voor je persoonlijke machine maken we gebruik van [docker](https://www.docker.com/), een programma voor het beheren, opstellen en uitvoeren van Linux containers. Indien je werkt met de PC's in het computerlabo, ga je werken met [podman](https://podman.io/). Docker en podman zijn equivalent in zake functionaliteit voor het uitvoeren van containers en beiden zijn in staat om containers te maken van container images die voldoen aan de Open Container Initiative [OCI](https://opencontainers.org/) specificatie. Je kan instructies voor de installatie van docker op je persoonlijke machine hieronder terug vinden.

## Installatie

Hieronder kan je de installatie instructies terugvinden voor Linux, MacOS en Windows

### Linux

Afhankelijk van de linux package manager die je gebruikt, verschilt de installatie van docker voor linux. Hieronder vind je de instructies terug voor systemen die gebruik maken van de `apt` package manager (Ubuntu, Debian). De instructies voor andere package managers kan je terug vinden op de volgende pagina: https://docs.docker.com/engine/install/

#### Ubuntu

Voer de volgende commando's uit om docker te installeren

```bash
# update package informatie
sudo apt-get update

# installeer benodigdheden voor docker
sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

# installer de cryptografische sleutels voor docker code repo
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# set-up van code repository
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# update package informatie
sudo apt-get update

# installeer docker tools en docker
sudo apt-get install docker-ce docker-ce-cli containerd.io

# controleer je installatie
sudo docker run hello-world
```

Indien je docker commando's wilt uitvoeren zonder sudo, neem dan zeker een kijkje op
de volgende webpagina: https://github.com/sindresorhus/guides/blob/main/docker-without-sudo.md

### MacOS
Docker voor MacOS is eenvoudig te installeren via de officïele installer.
Je kan de installer downloaden via https://docs.docker.com/desktop/mac/install/
en daar de juiste installer te selecteren. Kies 'Mac with Intel chip' indien je een Mac machine
hebt met een Intel processor en 'Mac with Apple chip' indien je laptop voorzien
is van een M1 processor.

#### Intel chip
Indien je een MacOS machine hebt met een Intel processor is de installatie relatief eenvoudig. Download de installer (Docker.dmg) en dubbelklik op de installer. Sleep daarna het docker icoontje naar je 'Applications folder' en laat de installer zijn werk doen.

Aangezien docker voor MacOS gebruik maakt van een Linux virtuele machine, moet je nagaan of docker actief is. Dit kan je doen door na te gaan of het docker icoontje ![docker-icon](https://docs.docker.com/desktop/mac/images/whale-x.png) rechts boven zichtbaar is in de menu bar.

#### Apple Silicon
Indien je een M1 processor in je laptop hebt zitten, verloopt de installatie met een extra stap. Je moet namelijk eerst rosetta installeren, als je dit nog niet gedaan hebt. Dit doe je
door het onderstaande commando uit te voeren in de terminal. Je kan een terminal sessie
starten door de `cmd` + `space` toetsen in te duwen en te zoeken op terminal.

```bash
softwareupdate --install-rosetta
```

De rest van de stappen zijn identiek aan de Intel processor versie.

### Windows

Voor Windows machines maken we gebruik van het programma `docker` om linux containers uit te voeren. Om docker te installeren surf je naar https://hub.docker.com/editions/community/docker-ce-desktop-windows/ en volg je de instructies.

1. Klik op de knop `Get Docker Desktop for Windows`. Dit zou de download van de docker desktop installer moeten starten.
2. Van zodra de installer gedownload is, dubbelklik op het bestand  `Docker for Windows Installer` (of de naam die je aan het bestand gegeven hebt) om de installatie te starten.
3. Als de installer vraagt om te kiezen voor 'Enable Hyper-V Windows Features' of 'Install required Windows components for WSL 2' selecteer dan **Install required Windows components for WSL 2**.
4. Volg de instructies van de installer om er voor te zorgen dat Docker for Windows de juiste rechten heeft.
5. Als de installatie succesvol is, sluit af door op `Close` te klikken.
6. Indien je huidig account niet het administrator account van je computer is, moet je de huidige gebruiker handmatig aan de `docker-users` groep toevoegen. Ga naar je instellingen als administrator en klik door naar `Lokale gebruikers en groepen > Groepen > docker-users` en voeg de gewenste gebruiker toe.

Na de installatie zou docker-deskop actief moeten zijn, je kan dit controleren door op zoek te gaan naar het docker icoontje ![docker icon](https://d1q6f0aelx0por.cloudfront.net/icons/whale-x-win.png) in je taakbalk.

Je kan controleren of docker correct geïnstalleerd is door het commando `docker version` uit te voeren in windows powershell.

## Opdrachten

Gelijkaardig aan de Linux oefenzitting verloopt de LXC oefenzitting volgens verschillende levels. Je kan levels 'unlocken' door de opdrachten uit te voeren op het huidige level, waarna de LXC container je een volgende opdracht geeft.

Moest je tijdens deze oefenzitting ergens vast komen te zitten, vraag dan zeker hulp aan een van de assistenten. Je kan een overzicht van de commando's voor docker en podman hieronder terugvinden.

### Tips

Net zoals bij bestanden in Linux is het mogelijk om de container images die je gedownload hebt op je computer op te lijsten. Dit kan je doen met het commando:

```bash
# eigen machine
docker images
# PC klas
podman images
```

Het is ook mogelijk om het aantal gebruikte containers weer te geven, je kan de actieve containers oplijsten met het commando:

```bash
# eigen machine
docker ps
# PC klas
podman ps
```

En een overzicht van alle containers (actief en inactief) kan je tonen met:

```bash
# eigen machine
docker ps --all
# PC klas
podman ps --all
```

Daarnaast kan je ook containers en images verwijderen, dit doe je op basis van de naam van de image of de container. Om images te verwijderen gebruik je het volgende commando:

```bash
# verwijder image op eigen machine
docker rmi [Naam van de image]
# verwijder image in PC klas
podman rmi [Naam van de image]
```

Inactieve containers kan je direct verwijderen als volgt:

```bash
# eigen machine
docker rm [Naam of id van de container]
# PC klas
podman rm [Naam of id van de container]
```

Indien je een actieve container wilt verwijderen, kan je dit doen met de docker/podman variant op het `kill` commando:

```bash
# Geef signaal aan de container om te stoppen op eigen machine
docker kill [Naam of id van de container]
# Geef signaal aan de container om te stoppen in PC klas
podman kill [Naam of id van de container]
```

### Level 1 (Eigen machine)

In level 1 exploreren we het uitvoeren van containers in docker. Voor deze opdracht
maak je gebruik van het subcommando `docker run` om een container te starten.

Gebruik de image `ghcr.io/informaticawerktuigen/lxc-docker-level1:latest`
om je container mee te starten. De container zal de opdracht voor level 2 uitprinten en daarna stoppen.

tip: geef de optie `--rm` mee aan het `docker run` commando om de container automatisch te verwijderen nadat hij klaar is.

### Level 1 (PC klas)

In level 1 exploreren we het uitvoeren van containers in podman. Voor deze opdracht
maak je gebruik van het subcommando `podman run` om een container te starten.

Gebruik de image `ghcr.io/informaticawerktuigen/lxc-podman-level1:latest` om
je container mee te starten. De container zal vervolgens de opdracht voor
level 2 uitprinten en daarna stoppen.

tip: geef de optie `--rm` mee aan het `podman run` commando om de container automatisch te verwijderen nadat hij klaar is.

## Referenties
Hieronder vind je enkele referenties met betrekking tot docker en podman. De referenties
aangeduid met `(extra)` zijn enkel voor de studenten die meer willen weten over
het opstellen en builden van container images.

### Docker
1. Referentie `docker run`: https://docs.docker.com/engine/reference/commandline/run/
2. Referentie alle docker commando's: https://docs.docker.com/engine/reference/commandline/docker/
3. (extra) Eigen container image opstellen: https://docs.docker.com/get-started/02_our_app/
4. (extra) Dockerfile referentie: https://docs.docker.com/engine/reference/builder/
5. (extra) Dockerfile best practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

### Podman
1. Refentie `podman run`: https://docs.podman.io/en/latest/markdown/podman-run.1.html
2.  Referentie alle podman commando's: https://docs.podman.io/en/latest/Commands.html
