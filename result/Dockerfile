# Dockerfile pour le service result
FROM node:20-alpine

WORKDIR /app

# Copier les fichiers nécessaires pour l'application
COPY package.json package-lock.json /app/

# Installer les dépendances
RUN npm install --production

# Copier le reste des fichiers
COPY . /app/

# Exposer le port 80
EXPOSE 80

# Démarrer l'application Node.js
CMD ["node", "app.js"]
