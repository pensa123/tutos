# Sopes 1

## Docker 
* Descargar imagenes de docker 

```r
docker pull ubuntu
```
* crear y correr imagen de docker.

```r
docker run -v ruta:/app -p 3000:3000 --name "contSD" -it ubuntu /bin/bash
```
* docker: nos dice que usaremos docker
* run: vamos a correr no solo crear una imagen
* -v:  creo que es que la carpeta de ruta:/app sera anclada a la imagen, todo lo que se ponga en esa carpeta estara tambien en la imagen.
* -p:  enlaza el puerto del docker con un puerto de nuestra compu
* --name: es el nombre del contenedor 
* -it esto nos dice que se correra en este plano y no en segundo plano creo :D 
* ubuntu: esto nos dice que usaremos la imagen de ubuntu 
* /bin/bash: correra la consola. 

-----------------------------------------------------------------------------------------------------
* iniciar / parar un contenedor
```r
 docker start/stop prueba1
```
* correr un contenedor ya iniciado
```r
docker exec -i -t contSD /bin/bash
```

* Listar contenedores 
```r
docker ps -a
```

* Listar imagenes de docker 
```r
docker images
```

* Eliminar contenedor 
```r
docker rm 
```

* Eliminar imagen 
```r
docker rmi 
```
----------------------------------------------------------------------------------------------------
## Utilizar ditroless / crear imagenes distroless
* se debe crear un Dockerfile 
* y se corren los siguientes comandos. 

* Ejemplo de Dockerfile
```r
FROM node:10.17.0 AS build-env
ADD . /app
WORKDIR /app


FROM gcr.io/distroless/nodejs
COPY --from=build-env /app /app
WORKDIR /app
EXPOSE 3000
CMD ["index.js"]
```

* comando para crear la imagen
```r
docker build -t contDistroless . 
```
* de esta forma se corre el container, si la imagen es de nodejs o de algo sin sistema operativo. 
```r
docker run -it -p 3000:3000 --name "contdistroless" contdistroless
```

## Kubernetes 

### instalar linkerd 


### instalar jaeger 
