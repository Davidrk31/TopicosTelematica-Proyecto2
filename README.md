# Proyecto 2
# Info de la materia: ST0263 Topicos especiales en Telematica.

## Estudiante(s):
- **David Alvarez Grisales**
- **Julio Cesar Posada Torres** 

## Profesor:
- **Nombre:** Alvaro Enrique Ospina Sanjuan
- **Correo Electrónico:** aeospinas@eafit.edu.co

## Proyecto 2.

## 1. Breve descripción de la actividad.
En este proyecto, desplegaremos una aplicación WordPress utilizando MicroK8s en AWS. En AWS, configuramos tanto el nodo maestro como los nodos trabajadores para implementar MicroK8s, que nos permite crear un clúster propio de Kubernetes. Además, se configuró una máquina como servidor NFS para garantizar la persistencia de los datos de WordPress y la base de datos, asegurando así la persistencia de los datos.
Se realizo inicialmente en GPC, pero cambiamos a AWS.

# 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

Se cumplieron todos los requerimientos del proyecto 2:
- Aplicacion wordpress dockerizada con varios pods.
- Configuracion de los servicios, deployment, pv y pv para el wordpress.
- Base de datos MYSQL
- Servidor NFS para la persistencia de los datos
- Uso de ingress para permitir el trafico para wordpress
- Certificado SSL
- App desplegada en un dominio propio

# 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)
Se cumplieron todos los requerimientos necesarios
# 2. información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.
Arquitectura:
Inicialmente se tenia planeado hacerlo en google cloud, pero presentamos errores que no nos permitieron continuar asi que lo hicimos en AWS

Pero la arquitectura que diseñamos antes de realizarlo sigue siendo la misma a pesar de usar otro ambiente como lo es AWS:

![image](https://github.com/Davidrk31/TopicosTelematica-Proyecto2/assets/89051979/53f1472c-cb73-4fbe-81b4-87e426e8c319)


# 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.
Se uso el software microk8s en AWS, ademas del uso de una base de datos MYSQL y un servidor NFS para la persistencia de los datos.
Se usaron las versiones:
- docker.io/bitnami/mysql:8.0
- Wordpress
- NFS versión 4.1
- AWS Ubuntu Ubuntu 24.04 LTS.

Configuracion de las instancias:
Se configuro una VPC para el proyecto 2 con su respectivo grupo de seguridad y 2 subredes publicas:
![image](https://github.com/Davidrk31/goku-/assets/89051979/64606d05-3c15-439b-879e-12d2e710fac8)

Todas las instancias se configuraron iguales con t2.medium y 25gb de almacenamiento
![image](https://github.com/Davidrk31/goku-/assets/89051979/76ef830f-a274-467f-8c5e-ddfaedb147b9)


Instancias:
![image](https://github.com/Davidrk31/goku-/assets/89051979/13fd7433-e988-436a-b878-f428265f6fae)

Y se instalo microk8s en todas las instancias siguiendo este tutorial:
https://microk8s.io/docs/getting-started

```
sudo snap install microk8s --classic --channel=1.30
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube
microk8s status --wait-ready
microk8s kubectl get services
```

# Agregamos los nodos Esclavo
- `microk8s join 10.0.24.88:25000/9a47166f45a62e47ae61ed7de54070fc/0796d19dd2c7 --worker`
- `microk8s join 10.0.24.88:25000/5b3349d47cedb52550e223ecd0d2a3ad/0796d19dd2c7 --worker`
![image](https://github.com/Davidrk31/goku-/assets/82523496/bbf0cfce-c190-4083-ae76-b8ad1734cebb)
![image](https://github.com/Davidrk31/goku-/assets/82523496/596d89bb-f1c0-46e8-b42e-58bf21f09bf5)

# Configuración de la instancia NFS:
Se realizo siguiendo este tutorial https://microk8s.io/docs/how-to-nfs 
![image](https://github.com/Davidrk31/goku-/assets/82523496/26b3179b-81cd-4e4a-835a-9aad59c3e2b2)


## Master carpetas finales:
![image](https://github.com/Davidrk31/goku-/assets/89051979/248f5d2d-e464-4404-8732-9ddc77728b3f)

## Certificado ssl
Para el certificado SSL seguimos este tutorial:

https://stackoverflow.com/questions/67430592/how-to-setup-letsencrypt-with-kubernetes-microk8s-using-default-ingress

Después de haber creado los manifestos del certificado ssl verificamos su creación con la siguiente línea de código:
- `microk8s kubectl get certificate`
![image](https://github.com/Davidrk31/goku-/assets/82523496/a2d51e1d-bac6-4b0b-8c33-482563812d71)
Tenemos que esperar a que el estado esté en "TRUE"

# 4 Ambiente de ejecucion:
El proyecto se encuentra   en https://proyecto2.construmarket.tech/

# Resultados del proyecto
![image](https://github.com/Davidrk31/goku-/assets/89051979/ca73d357-f4cc-4f30-b835-9e9cfa605214)

![image](https://github.com/Davidrk31/goku-/assets/89051979/d71c6569-b0f5-4ce5-9808-1f3e328eceb9)


# Referencias:
* [Instalación Kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
* https://foolcontrol.org/?p=3754
* [Instalación Helm](https://helm.sh/docs/intro/install/)
* [NFS](https://microk8s.io/docs/how-to-nfs)
* [microk8s](https://microk8s.io/docs/getting-started)
* [SSL](https://stackoverflow.com/questions/67430592/how-to-setup-letsencrypt-with-kubernetes-microk8s-using-default-ingress)



