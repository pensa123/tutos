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
### Comandos para ver lo que tiene nuestro cluster

* Listar nodos de kubernetes, creo que por defecto crea uno
```r
kubectl get nodes
```

* Listar pods de kubernetes, pods son las como computadoras que se crean con imagenes de docker. 
```r
kubectl get pods
```
* Listar los servicios de kubernetes, los servicios son los que enlazan la ip del pod o deployment con la ip del cluster y así se puede conectar. 
```r
kubectl get services
```
* Listar los pods, pero de esta forma tambien se mira la ip interna que estos tienen y a que nodo pertenecen
```r
kubectl get pods -o wide 
```
* Listar los deployments, estos son como pods pero se reinician una vez terminan y se pueden moniorear con jaeger o linkerd. con los pods no se puede monitorear
```r
kubectl get deployments
```
* Describe el nodo, su ip y su ip externa y muchas otras cosas pero soo eso buscaba xd
```r
kubectl describe nodes nodo_a_describir
```
* A todos los comandos anteriores se les puede agregar "-n namespace" para seleccionar los pods/servicios/deployment del namespace especifico

### Comandos para crear pods, deployments y servicios
* crear namespace 
```r
kubectl create namespace nombre_del_namespace
```

* crear pod
```r
kubectl run mipod --image=nginx --restart=Never
```

* craer deployment
```r
kubectl create deployment app1 --image=nginx
```
* ejecutar un deploment en vez de -n proyecto cualquier namespace, si esta en blanco se usa el namespace default  
```r
kubectl exec -it deployment.apps/py -n proyecto -- bash
```
* Ejecutar un pod 
```r
kubectl exec -it pod/mipod -n proyecto -- bash
```
* Crear un servicio, en vez de deployment.apps se puede usar pod y el --name puede ser cualquiera, tambien se pude usar cualquier namespace 
```r
kubectl expose deployment.apps/app1 --port=80 --type=NodePort --name=app1apache
```

### instalar linkerd 

* Se usa el siguieten comando para deescargar linkerd
```r
curl https://run.linkerd.io/install | sh
```

* se pega lo de abajo en el archivo  ~/.profile  
```r
export PATH=$PATH:$HOME/.linkerd2/bin
```

* se usa el siguiente comando, para que se pueda usar linkerd en la compu 
```r
source ~/.profile 
```
* para ver si esta insalado en la maquina fisica :D 
```r
linkerd version
```

* se puede hacer un linkerd check --pre para ver si se puede instalar pero yo no lo hice jejeje

* instala linkerd en el servidor. 
```r
linkerd install | kubectl apply -f -
```

* con esto comprobamos que esta instalado 
```r
linkerd check 
```
* si corremos linkerd version ahora nos dice que esta instalado en el cluster y en local 


* el siguiente comando de abajo abre una pagina en el localhost, aca salen los pods que se tienen corriendo pero por defecto sale meshed 0/1, si es así no se puede monitorear
```r
linkerd dashboard
```

* para pasar los deployment a meshed se utiliza el siguiente comando: 
```r
kubectl get deployments -n namespace -o yaml | linkerd inject - | kubectl apply -f -
```

* con el siguiente comando se verifica si ya se estan monitorenado (si ya estan meshed 1/1)
```r
linkerd stat deployments -n namespace
```
* y si se da a hora linkerd dashboard ya  aparecen los deployments y se pueden monitorear :D
* esto tambien incluy graphana y tambien hay una forma de ver como se comunican internamente los deployments

### instalar jaeger 

* descargar el archivo contour.yaml.txt :D (estara en la misma carpeta en este repo. 
* kubectl apply -f contour.yaml.txt
* git clone https://github.com/sergioarmgpl/jaeger1.16-python-flask-envoy-k8s.git
* cd jaeger # en la ruta donde se descargo el repo :D 
* se edita editas el archivo que se llama jaeger_install.sh, ponemos el namespace que usamos
* /bin/bash jaeger_install.sh # se corre el siguiente comando para que se intale al fin :D
* se edita el arcivo simplest.yaml en el host se pone el subdominio que creemos. 
* kubectl create -f simplest.yaml  # no se que hace xd
* kubectl get services -n tuproyecto # muestra la ip en que esta el servicio (creo que esto tambien se agrega en el simplest.yaml)
* kubectl create -f ingress.yaml
* y ya esta listo, aunque es mas facil y entendible linkerd :D 
