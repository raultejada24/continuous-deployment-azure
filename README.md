# Despliegue continuo en Azure

Este proyecto consta de un servidor WEB sencillo para la gestión de posts (anuncios).

## Despliegue en Azure (local)

En primer lugar, nos logueamos en el cliente de Azure (el login se realizará con ayuda del navegador)

```
az login
```

Creamos un grupo de recursos `posts-group`
```
az group create --name posts-group --location westeurope
```

Creamos nuestra aplicación lanzando un contenedor a partir de la imagen `maes95/posts:v1`. Le pondremos a la aplicación el nombre `posts-app`

```
az containerapp up \
  --name posts-app \
  --resource-group posts-group \
  --location westeurope \
  --image maes95/posts:latest \
  --target-port 8080 \
  --ingress external
```

Podemos obtener la URL de nuestra aplicación con el siguiente comando:
```
az containerapp show -n posts-app -g posts-group --query properties.configuration.ingress.fqdn
```

Podemos borrar la aplicación con el siguiente comando:
```
az container delete -n posts-app -g posts-group --yes
```

Comandos ejecutados para construir la imagen y subirla a DockerHub:

```
mvn spring-boot:build-image \ -DskipTests \ -Dspring-boot.build-image.imageName=raultejada24/posts:v1

docker push raultejada24/posts:v1
```