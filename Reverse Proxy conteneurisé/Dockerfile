# On utilise une image Debian
FROM debian

# On met à jour les paquets et on installe Nginx
RUN apt-get update && apt-get install -y nginx

# On copie le fichier de configuration Nginx personnalisé dans l'image
COPY nginx.conf /etc/nginx/nginx.conf

# On expose le port 80
EXPOSE 80

# Commande pour démarrer Nginx en arrière-plan
CMD ["nginx", "-g", "daemon off;"]
