# Versión de Ubuntu LTS
FROM ubuntu:20.04

# Comandos a ejecutar (dependencias)
# Actualizar la lista de paquetes disponibles en los repositorios 
RUN apt-get update
RUN apt-get install -y software-properties-common 

# Exponer el puerto 32002 (hay que tambien exponerlo en Kubernetes)
EXPOSE 32002

# Copiar el ejecutable serverFileManager desde el host al contenedor
# El archivo debe estar en el mismo directorio que el Dockerfile
# Sintaxis: COPY <origen_host> <destino_contenedor>
COPY brokerFileManager /app/brokerFileManager

# Dar permisos de ejecución al archivo
# chmod +x hace que el archivo sea ejecutable
# Sin esto, el contenedor fallará con "permission denied"
RUN chmod +x /app/brokerFileManager

# Comando que se ejecuta cuando el contenedor arranca
CMD /app/brokerFileManager



# ============================================
# NOTAS IMPORTANTES
# ============================================
#
# CONSTRUCCIÓN:
#    docker build -t jorgematesanz/broker-filemanager:1.0 <directorio_dockerfile>
#    docker images
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