
Para descargar el proyecto de forma local, se puede descargar desde: https://github.com/ggalvezar/craftech y usar la rama "master".

Se crearon dos repositorio en dockerhub (ggalvez/frontend y ggalvez/backend)  para descargar las imagenes desde alli:
administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ docker image ls | grep ggalvez
ggalvez/backend               backend           a5d575866875   24 hours ago    679MB
ggalvez/backend               latest            a5d575866875   24 hours ago    679MB
ggalvez/frontend              latest            4a09e78c0b3d   27 hours ago    593MB


#### Prueba 2 - Despliegue de una aplicación Django y React.js
De acuerdo al enunciado, se planteo el escenario desde dos formas, usando: 
       -Docker Compose
       -Kubertenes

DOCKER-COMPOSE
---------------------------------------------------------------------------------------------------------------------------------------------------------------
1) Docker_Compose: En la carpeta FrontEnd y Backend exite un Dockerfile por cada carpeta respectivamente. En base a esos dockerfile, se armaron las imagenes de frontend (react) y backend(django). Una vez armadas se procedio a ejecutar el docker-compose para levantar las imagenes:

administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ docker-compose up -d
Creating network "enunciado2_default" with the default driver
Creating enunciado2_backend_1 ... done
Creating enunciado2_frontend_1 ... done

administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ docker-compose ps
        Name                       Command               State           Ports         
---------------------------------------------------------------------------------------
enunciado2_backend_1    python3 manage.py runserve ...   Up      0.0.0.0:8000->8000/tcp
enunciado2_frontend_1   docker-entrypoint.sh npm start   Up      0.0.0.0:3000->3000/tcp

PRUEBA de FRONTEND:
administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ curl -I http://localhost:3000
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Thu, 20 May 2021 11:22:51 GMT
ETag: W/"636-17989838ef8"
Content-Type: text/html; charset=UTF-8
Content-Length: 1590
Vary: Accept-Encoding
Date: Mon, 24 May 2021 18:42:20 GMT
Connection: keep-alive

PRUEBA de BACKEND:
administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ curl -I http://localhost:8000
HTTP/1.1 200 OK
Date: Mon, 24 May 2021 18:43:16 GMT
Server: WSGIServer/0.2 CPython/3.7.10
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Vary: Cookie
Content-Length: 4056
Set-Cookie:  csrftoken=oqMGhR09AwF2XUMQ3VuCEi1MTw6fU5g6QiAgLGXnGaZPJ5mvFOuUo6tWoM9BQlBx; expires=Mon, 23 May 2022 18:43:16 GMT; Max-Age=31449600; Path=/; SameSite=Lax
--------------------------------------------------------------------------------------------------------------------------------------------------------

KUBERNETES:
---------------------------------------------------------------------------------------------------------------------------------------------------------
Herramientas Usadas:
      -Kompose convert
      -Minikube: Para simular el cluster de kubertenes
       administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ kubectl get nodes
       NAME       STATUS   ROLES                  AGE    VERSION
       minikube   Ready    control-plane,master   3d6h   v1.20.2
        
1)Se realizo la conversion con Kompose convert para generar los KubertenesDeploy
2)Se creo un namespace craftech
    $kubectl create namespace craftech
3) Configuramos el contexto:
    $kubectl config set-context $(kubectl config current-context) --namespace craftech
4) Ahora procedemos a ejecutar la creacion de los pods: 
   $kubectl apply -f kubernetesdeploy/.

administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
backend-cbd6b7548-4ntlv     1/1     Running   0          4m43s
frontend-67c58dc5cc-p6tfs   1/1     Running   0          4m43s

5) Se crearon dos servicios tipo ClusterIP
administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ kubectl get svc
NAME       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
backend    ClusterIP   10.102.250.114   <none>        8000/TCP   15m
frontend   ClusterIP   10.111.112.90    <none>        3000/TCP   15m
administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ curl -I http://localhost:8000
HTTP/1.1 200 OK
Date: Mon, 24 May 2021 19:18:06 GMT
Server: WSGIServer/0.2 CPython/3.7.10
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Vary: Cookie
Content-Length: 4056
Set-Cookie:  csrftoken=wCJ2yp7iFj8B0vGNZe9D1nmdvBTQmmE4SJZoigVu86fOJWve21WLlDxAbx6SMFoD; expires=Mon, 23 May 2022 19:18:06 GMT; Max-Age=31449600; Path=/; SameSite=Lax

administrator@T490:~/Documents/AWS/DevOps-PIM/Craftech/Enunciado2$ curl -I http://localhost:3000
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Thu, 20 May 2021 11:22:51 GMT
ETag: W/"636-17989838ef8"
Content-Type: text/html; charset=UTF-8
Content-Length: 1590
Vary: Accept-Encoding
Date: Mon, 24 May 2021 19:18:15 GMT
Connection: keep-alive
       








#### Prueba 3 - CI/CD (PENDIENTE)

Dockerizar un nginx custom con el index deseado.
Elaborar un pipeline que buildee la nueva imagen y la actualize en la plataforma elegida. (docker-compose, swarm, kuberenetes, etc.)
Para la creacion del CI/CD se puede utilizar cualquier plataforma (CircleCI, Gitlab, Github, Bitbucket.)

**Requisitos y deseables:**



