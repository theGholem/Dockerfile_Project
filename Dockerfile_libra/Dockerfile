# Utilisation de l'image Ubuntu "noble" (attention : elle peut être une version non stable)

# Si possible, utiliser une version LTS comme 22.04 ou 20.04 pour la production.
FROM ubuntu:noble

# On rend paramétrable la version de l'application Libra et son empreinte de vérification
ARG LIBRA_VERSION=0.0.1
ARG LIBRA_CHECKSUM=d0757c5b427825126f98ffa07683d676fb8c62a02d098659cce9d3cd485b79a0
ARG LIBRA_REPOSITORY=https://github.com/Bornholm/libra

# Mise à jour des dépendances système pour réduire les risques de failles
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get upgrade -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Installation des dépendances nécessaires pour télécharger et vérifier l'application
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y wget ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Téléchargement de l'archive contenant l'exécutable Libra
RUN wget -O libra.tar.gz \
    ${LIBRA_REPOSITORY}/releases/download/v${LIBRA_VERSION}/libra_${LIBRA_VERSION}_linux_amd64.tar.gz

# Vérification de l'intégrité de l'archive téléchargée grâce à l'empreinte SHA256
RUN echo "${LIBRA_CHECKSUM}  libra.tar.gz" | sha256sum --check --status

# Création du répertoire où le binaire sera installé
RUN mkdir -p /usr/local/bin

# Extraction du binaire Libra et déplacement dans le répertoire d'exécution
RUN tar -xzf libra.tar.gz -C /usr/local/bin libra

# On rend le binaire exécutable
RUN chmod +x /usr/local/bin/libra

# Nettoyage des fichiers inutiles pour réduire la taille de l'image Docker
RUN rm -f libra.tar.gz

# Indique que l'application écoute sur le port 8080
EXPOSE 8080

# Déclaration d'un volume pour persister les données dans /uploads
VOLUME /uploads

# Configuration de l'exécutable Libra comme processus principal au démarrage du conteneur
CMD ["/usr/local/bin/libra"]
