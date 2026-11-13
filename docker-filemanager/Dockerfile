# Versión de Ubuntu LTS
FROM ubuntu:20.04

# Comandos a ejecutar (dependencias)

# Actualizar la lista de paquetes disponibles en los repositorios 
RUN apt-get update
RUN apt-get install -y software-properties-common 

# Instalar curl
RUN apt-get install -y curl

# Exponer el puerto 32001 (hay que tambien exponerlo en Kubernetes)
EXPOSE 32001


# Crear directorio para archivos gestionados por el servidor
# Este directorio será usado por serverFileManager para:
# - Almacenar archivos subidos por clientes
# - Listar archivos disponibles
# - Descargar archivos a clientes
# En Kubernetes, se montará un volumen (NFS o hostPath) aquí
RUN mkdir -p /app/FileManagerDir

# Copiar archivo de configuración DNS personalizado
# Este archivo contiene los nameservers que el contenedor usará
# para resolver nombres de dominio
# IMPORTANTE: El archivo resolv.conf debe existir en el directorio
# del Dockerfile antes de construir la imagen
COPY resolv.conf /resolv.conf


# Copiar el ejecutable serverFileManager desde el host al contenedor
# El archivo debe estar en el mismo directorio que el Dockerfile
# Sintaxis: COPY <origen_host> <destino_contenedor>
COPY serverFileManager /app/serverFileManager


# Dar permisos de ejecución al archivo
# chmod +x hace que el archivo sea ejecutable
# Sin esto, el contenedor fallará con "permission denied"
RUN chmod +x /app/serverFileManager

# [IP:BROKER_NODE_IP] es la ip del nodo con el label "broker"
# Para asignar label a un nodo:
# kubectl label nodes <nombre_nodo> role=<rol>
# Para ver nodos y labels:
# kubectl get nodes --show-labels

# Comando que se ejecuta cuando el contenedor arranca
CMD ["/bin/sh", "-c", "cp /resolv.conf /etc/resolv.conf && IP=$(curl -s https://api.ipify.org) && echo \"IP: $IP\" && cd /app && /app/serverFileManager BROKER_NODE_IP 32002 $IP 32001"]

# ============================================
# NOTAS IMPORTANTES
# ============================================
#
# CONSTRUCCIÓN:
#    docker build -t jorgematesanz/server-filemanager:1.0 <directorio_dockerfile>
#    docker images
#
# PUBLICACIÓN EN DOCKER HUB:
#    docker push jorgematesanz/server-filemanager:1.0
#
# REQUISITOS PREVIOS:
#   1. Archivo 'serverFileManager' en el directorio
#   2. Archivo 'resolv.conf' en el directorio
#
# VERIFICACIÓN:
#   docker logs server
#   docker exec -it server cat /etc/resolv.conf
#   docker exec -it server curl -s https://api.ipify.org
#
# TROUBLESHOOTING:
#   - Si falla DNS: verificar que resolv.conf existe y tiene nameservers válidos
#   - Si falla curl: verificar conectividad a internet desde el contenedor
#   - Si no se registra en broker: verificar IP BROKER_NODE_IP y puerto 32002
#
# ============================================