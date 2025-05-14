# Dockerfile pour le service poll
FROM python:3.11-slim

WORKDIR /app

# Copier les fichiers nécessaires pour l'application
COPY app.py /app/

# Installer les dépendances
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Exposer le port 80
EXPOSE 80

# Démarrer l'application Flask
CMD ["flask", "run", "--host=0.0.0.0", "--port=80"]
