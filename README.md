# Transmisson Control
 Dispatch is an automated container service which makes use of Transmission. This project provides automated deployment and destruction of Transmission containers while allowing for persistent data.

The idea is that it should be easy to download whatever torrent files you want, whenever you want, from wherever you want. Transmission Control will (eventually) allow for you to create containers from a web interface. The containers will then run an ephimeral version of Transmission and its associated web interface. One of the final goals of this project will be the ability to create automation rules, which will trigger events to occur based on observed conditions such as Webhooks or RSS feeds.

The data will be downloaded and stored onto unique Docker Volumes that users have access to. At this time it's unclear if this will be done by unique SFTP access or by other means, such as a tie to Nextcloud.

