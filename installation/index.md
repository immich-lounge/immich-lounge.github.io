# Installation

Check the [requirements](<https://immich-lounge.github.io/#requirements>) before installing. Immich Lounge supports Immich 3.1 or later; birthday memories require Immich 3.3 or later.

## Companion

If you already run immich with Docker Compose, the simplest setup is to add one service to that existing compose file.

Use a normal Docker port mapping on `4383`. Do not expose the companion to the public internet.

## Preferred setup

Choose the Compose layout that fits your installation. Keep the companion on the same trusted LAN as your Roku and Immich server, and persist its data in a Docker volume.

> [!WARNING]
>
> **Keep the companion on your LAN**
>
> Use a normal Docker port mapping on `4383`. The companion does not provide public-internet authentication.

Add the service and named volume below to your existing Immich Compose file. Keep its existing services and volumes.

Companion service and volume

```
# docker-compose.yml
# Add this service to your existing Immich docker-compose.yml.
services:
  immich-lounge-companion:
    image: ghcr.io/immich-lounge/immich-lounge-companion:latest
    container_name: immich-lounge-companion
    pull_policy: always
    ports:
      - "4383:4383" # change the left side if 4383 is already in use; do NOT expose publicly
    volumes:
      - immich-lounge-data:/data
    environment:
      - ASPNETCORE_URLS=http://+:4383
    restart: unless-stopped

volumes:
  immich-lounge-data:
```

From that stack's folder, start only the companion:

```
docker compose up -d immich-lounge-companion
```

Create a folder for the companion and save this as `docker-compose.yml`:

docker-compose.yml

```
# docker-compose.yml
# Add this service to your existing Immich docker-compose.yml.
services:
  immich-lounge-companion:
    image: ghcr.io/immich-lounge/immich-lounge-companion:latest
    container_name: immich-lounge-companion
    pull_policy: always
    ports:
      - "4383:4383" # change the left side if 4383 is already in use; do NOT expose publicly
    volumes:
      - immich-lounge-data:/data
    environment:
      - ASPNETCORE_URLS=http://+:4383
    restart: unless-stopped

volumes:
  immich-lounge-data:
```

From that folder, start the companion:

```
docker compose up -d
```

The companion stores settings, profiles, schedules, and cache data under `/data`.

> [!NOTE]
>
> **Why localhost does not work for the Immich URL**
>
> The companion and Roku both need to reach the Immich server. `localhost` refers to the device making the request, so it does not identify your Immich server from the Roku. Use its LAN host name or IP address.

To update the companion later, run these from its Compose folder:

```
docker compose pull immich-lounge-companion
docker compose up -d immich-lounge-companion
```

Reference: [docker-compose.yml](<https://github.com/immich-lounge/immich-lounge/blob/master/docker-compose.yml>).

## Roku apps

Add the Roku apps to your account:

| App | Channel Store | Add Link |
| --- | --- | --- |
| Immich Lounge | [Open in Store](<https://channelstore.roku.com/details/b07c35a07c79ab9a68c29c923ac481ce:bdf9083c719f8f235036b2c91bcef535/immich-lounge>) | [Add to Account](<https://my.roku.com/account/add/immichlounge>) |
| Immich Lounge Screensaver | [Open in Store](<https://channelstore.roku.com/details/1dbc550b0d535eebf60d6fc4b77ec0fb:a6fafa094f6a14e19c10f0bb1e06e315/immich-lounge-screensaver>) | [Add to Account](<https://my.roku.com/account/add/immichloungesaver>) |

Install on your Roku:

- **Immich Lounge**
- **Immich Lounge Screensaver**

The channel handles normal playback. The screensaver is configured from Roku system settings.

## First Roku setup

Open **Immich Lounge** on Roku and enter the companion URL:

```
http://192.168.1.10:4383
```

Then choose a profile.

Manual companion setup on Roku uses the companion host and port `4383`.

## Screensaver setup

From the Roku home screen, go to `Settings` -\> `Theme` -\> `Screensavers`, switch the active screensaver to **Immich Lounge Screensaver**, then open **Configure Screensaver**.

The screensaver can share the same companion URL and use either the same or a different profile.

For the step-by-step Roku flow, see [Using the Roku Apps](<https://immich-lounge.github.io/roku-apps/#change-the-roku-screensaver>) and Roku's own help article: [How to change the screensaver on your Roku streaming device](<https://support.roku.com/article/212015418>).

## Network notes

You usually only need two paths to work:

- Roku to companion on port `4383`
- Roku to immich on your immich server port, usually `2283`

The Roku fetches media and most metadata directly from immich. The companion is not a media proxy.

## After install

- Use [Configuration](<https://immich-lounge.github.io/configuration/index.md>) to understand the available connection and profile settings.
- Use [Using the Companion](<https://immich-lounge.github.io/companion-app/index.md>) for the normal companion workflow after setup.
- Use [Using the Roku Apps](<https://immich-lounge.github.io/roku-apps/index.md>) for first-run flow, remote controls, and the Roku settings menu.
