# Tópicos Especiales en Telemática: C2466-ST0263-1716

## Estudiantes
- **Santiago Alberto Rozo SIlva**, [ntovara@eafit.brightspace.com](mailto:sarozos@eafit.brightspace.com)
- **Isis Catitza Amaya Arbeláez**, [icamayaa@eafit.edu.co](mailto:icamayaa@eafit.edu.co)
- **Samuel Oviedo Paz**, [soviedop@eafit.edu.co](mailto:soviedop@eafit.edu.co)
- **Samuel Acosta Aristizabal**, [sacostaa1@eafit.edu.co](mailto:sacostaa1@eafit.edu.co)
- **Nicolás Tovar Almanza**, [ntovara@eafit.brightspace.com](mailto:ntovara@eafit.brightspace.com)
  
## Profesor
- **Álvaro Enrique Ospina Sanjuan**, [aeospinas@eafit.edu.co](mailto:aeospinas@eafit.edu.co)

# Proyecto No 2

## 1. Breve descripción de la actividad

La actividad consiste en desplegar la aplicación del Reto 2 en un `clúster Kubernetes` con alta disponibilidad en `AWS IaaS`, utilizando `microk8s` para configurar el clúster en máquinas virtuales. El despliegue debe incluir un servidor de archivos `NFS` para volúmenes compartidos, y una capa de acceso al clúster implementada con `Service`, `Ingress` o un balanceador. Además, debe cumplir con los requisitos del Reto 2: un dominio propio con `HTTPS`, balanceador de carga, base de datos externa y sistema de archivos externos a la capa de servicios (app).


## 1.1. Aspectos cumplidos o desarrollados
- Diagrama de arquitectura detallado.
- Despliegue de un clúster de `Kubernetes` en `AWS` utilizando `microk8s`.
- Implementación de la aplicación en el clúster, basada en el `CMS Drupal`.
- Base de datos `MySQL 8.0.4-debian` desplegada en el clúster.
- Integración de un sistema de archivos compartido mediante `NFS`.
- Documentación completa del proyecto.

## 1.2. Aspectos NO cumplidos o desarrollados
Se cumplieron la mayoría de los requisitos del proyecto, a excepción de dos aspectos:
1. **Persistencia en la Base de Datos**: No se logró implementar una configuración de alta disponibilidad en la base de datos `MySQL`, ya que se utilizó una única instancia sin redundancia. Esto representa un único punto de fallo en el sistema, afectando la disponibilidad en caso de falla.
2. **Balanceador de Carga con Dominio**: No se logró hacer funcionar completamente el balanceador de carga junto con el dominio personalizado configurado.


### 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas
El proyecto se cimenta sobre la implementación de un  `clúster Kubernetes` con alta disponibilidad en `AWS IaaS`, utilizando `microk8s` como se muestra en el siguiente diagrama de arquitectura:

![image](https://github.com/user-attachments/assets/66cfa784-8439-4d4f-8ee3-a4cacef37483)


### Componentes principales

#### 2.1.1. Componentes principales

- Se implementa un clúster `Kubernetes` en `AWS IaaS` mediante `microk8s` para lograr alta disponibilidad y escalabilidad en el despliegue de servicios de contenedores, en este caso, el `CMS Drupal`.
- Drupal se ejecuta en dos `pods` dentro del clúster, configurados como instancias de aplicación que manejan el tráfico y distribuyen la carga. Esto asegura mayor disponibilidad y permite el balanceo de carga entre las instancias para responder a múltiples solicitudes de usuarios.
- Se despliega una instancia de `MySQL 8.0.4-debian` como base de datos en el clúster, lo cual facilita el manejo de datos transaccionales y es accesible desde los `pods` de Drupal, aunque esta configuración actualmente carece de alta disponibilidad.
- Se utiliza un servidor de archivos `NFS` como sistema de almacenamiento compartido para los pods de Drupal, permitiendo que los datos y archivos estén disponibles de forma persistente y accesibles entre los nodos del clúster, asegurando redundancia y durabilidad.
  
#### 2.2. Patrones
- Estructura modular y separación de responsabilidades, segmentando la base de datos, la aplicación y el almacenamiento en diferentes servicios (`MySQL`, `Drupal` y `NFS`).
- Despliegue declarativo mediante manifiestos de `Kubernetes` (`YAML`) para definir cada componente, facilitando la reproducibilidad y escalabilidad del entorno.
- Patrón de alta disponibilidad mediante el uso de múltiples instancias de `Drupal`, balanceador de carga y almacenamiento compartido en `NFS`.

#### 2.3. Buenas prácticas
- Manejo de errores y escalabilidad en los servicios desplegados.
- Documentación completa del proyecto y comentarios en los archivos de configuración.
- Automatización mediante el uso de manifiestos para asegurar coherencia en el despliegue.
- Buena gestión de recursos, optimizando el uso de instancias y almacenamiento en el clúster.


### 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerías, paquetes, etc., con sus números de versiones.

El proyecto implementa una infraestructura de alta disponibilidad y escalabilidad para un sitio Drupal en un clúster `Kubernetes`, desplegado en `AWS` usando `microk8s`. A continuación se describe el ambiente técnico y los componentes utilizados:

#### 3.1. Lenguaje de Programación y Aplicación Principal:

Drupal es el `CMS principal` usado para el proyecto, desplegado mediante una imagen `Docker` de `Bitnami`.

#### 3.2. Servicios en Kubernetes:

- `microk8s` es la solución usada para implementar el clúster `Kubernetes` en AWS, permitiendo la orquestación y gestión de los contenedores en una infraestructura de alta disponibilidad.
- Balanceo de carga adicional mediante `AWS Load Balancer` (`ALB`), gestionando la entrada y salida del tráfico `HTTP` y `HTTPS` al clúster desde el exterior.

#### 3.3. Base de Datos:

`MySQL 8.0.4-debian` es la base de datos utilizada para el despliegue dentro del clúster `Kubernetes`. Esta versión específica de `MySQL` se seleccionó por su estabilidad y compatibilidad con `Drupal`.

#### 3.4. Sistema de Archivos:

Un servidor de archivos `NFS` proporciona un sistema de archivos compartido y persistente para los contenedores en el clúster. Este `NFS`, montado y gestionado manualmente en una instancia de `Ubuntu` en AWS, permite el almacenamiento accesible desde múltiples instancias dentro del clúster y se utiliza para almacenar archivos estáticos y contenido generado por el usuario en `Drupal`.

#### 3.5. Otras Herramientas y Librerías:

- **`kubectl`** para gestionar los recursos del clúster `Kubernetes` en `microk8s`.
- **`Helm`** empleado para simplificar el despliegue y la gestión de aplicaciones en el clúster `Kubernetes`.
- **`OpenSSH`** utilizado para establecer conexiones seguras `SSH` hacia las instancias en `AWS`.
- **`nfs-kernel-server`** y **`nfs-common`** para configurar y manejar el servidor `NFS` en `Ubuntu` y permitir el montaje del sistema de archivos en `Kubernetes`.

### 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

### 4.1. información general del clúster:

Para saber la información general del clúster ejecutamos el siguiente comando:

```bash
kubectl cluster-info
```

En nuestro caso la inforación arrojada de esta operación es la siguiente:

```bash
Kubernetes control plane is running at https://127.0.0.1:16443
CoreDNS is running at https://127.0.0.1:16443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

Luego comprobamos la información general de los pods con el siguiente comando:

```bash
kubectl get nodes -o wide
```

En nuestro caso la inforación arrojada de esta operación es la siguiente:

```bash
NAME                  STATUS   ROLES    AGE    VERSION   INTERNAL-IP     EXTERNAL-IP   OS-IMAGE            KERNEL-VERSION        CONTAINER-RUNTIME
ip-172-31-16-153      Ready    <none>   4d5h   v1.28.14  172.31.16.153   <none>        Ubuntu 24.04.1 LTS  6.8.0-1017-aws       containerd://1.6.28
ip-172-31-18-26       Ready    <none>   4d5h   v1.28.14  172.31.18.26    <none>        Ubuntu 24.04.1 LTS  6.8.0-1017-aws       containerd://1.6.28
ip-172-31-28-179      Ready    <none>   4d5h   v1.28.14  172.31.28.179   <none>        Ubuntu 24.04.1 LTS  6.8.0-1017-aws       containerd://1.6.28
```

### 4.2. Implementación de MicroK8s en AWS y Configuración de NFS.

Para deplegar el proyecto en el ambiente de `AWS`, se deben de seguir los pasos descritos a continuacion.

**4.2.1. Configuración del clúster en AWS**

A continuación se detalla el proceso para lanzar y configurar una instancia EC2 en AWS con MicroK8s:

#### Paso 1: Iniciar una instancia EC2 en AWS

- **Tipo de Instancia**: Selecciona una instancia de tamaño mediano, ya que esta proporciona un equilibrio adecuado entre rendimiento de CPU y memoria, ideal para ejecutar MicroK8s de manera eficiente.
- **Sistema Operativo**: Optamos por Ubuntu 22.04 LTS, debido a su estabilidad y compatibilidad con MicroK8s.
- **Espacio en Disco**: Configura un volumen de 20 GB para almacenar la instalación de MicroK8s y soportar las cargas de trabajo.
- **Grupo de Seguridad**: Define un grupo de seguridad que permita la comunicación necesaria para Kubernetes, incluyendo los puertos específicos.

#### Configuración del Grupo de Seguridad

Establece las siguientes reglas en el grupo de seguridad para habilitar la conectividad:

1. **SSH (Puerto 22)**: Permite el acceso remoto a través de SSH para gestionar la instancia.
2. **HTTP (Puerto 80)** y **HTTPS (Puerto 443)**: Habilita el tráfico web necesario para acceder a las aplicaciones.
3. **Reglas TCP Personalizadas**: Abre los puertos específicos requeridos por los servicios de MicroK8s.

#### Paso 2: Definir el Nombre del Host

Conéctate a la instancia mediante SSH y asigna un nombre al host para identificar el nodo:

```bash
sudo hostnamectl set-hostname microk8s-node1
```

#### Paso 3: Instalar MicroK8s

Utiliza Snap, el administrador de paquetes de Ubuntu, para instalar MicroK8s de la siguiente manera:

```bash
sudo snap install microk8s --classic --channel=1.28/stable
```

#### Paso 4: Configuración de Permisos de Usuario

Asigna el usuario actual al grupo de MicroK8s y ajusta los permisos de los directorios necesarios para su correcto funcionamiento:

```bash
sudo usermod -a -G microk8s $USER
sudo mkdir -p ~/.kube
sudo chown -f -R $USER ~/.kube
```

Reinicia la instancia para que los cambios de permisos tomen efecto:

```bash
sudo reboot
```

#### Paso 5: Comprobar la Instalación de MicroK8s

Una vez que la instancia se haya reiniciado, verifica que MicroK8s esté instalado y operativo:

```bash
microk8s status --wait-ready
microk8s version
microk8s inspect
```

Para obtener detalles del nodo, utiliza el siguiente comando:

```bash
cat /var/snap/microk8s/current/var/kubernetes/backend/localnode.yaml
```

#### Paso 6: Crear un Alias para `kubectl`

Para evitar conflictos con otras instalaciones de `kubectl`, puedes crear un alias para los comandos de `kubectl` de MicroK8s:

1. Abre el archivo `.bashrc` para editarlo:

   ```bash
   nano ~/.bashrc
   ```

2. Agrega la siguiente línea al archivo:

   ```bash
   alias mkubectl='microk8s kubectl'
   ```

3. Recarga el archivo `.bashrc` para aplicar los cambios:

   ```bash
   source ~/.bashrc
   ```

Ahora puedes usar `mkubectl` para ejecutar comandos en Kubernetes. Por ejemplo:

```bash
mkubectl get nodes
```
Por último, se deja un enlace a la documentación utilizada para esta sección:

[Deploying MicroK8s on AWS: A Step-by-Step Guide](https://www.cloudthat.com/resources/blog/deploying-microk8s-on-aws-a-step-by-step-guide)

**4.2.2. Configuración de NFS Mount en Ubuntu 20.04** 

En este tutorial, llamaremos `host` al servidor que comparte sus directorios, y `cliente` al servidor que los monta.

#### Paso 1: Descargar e instalar componentes necesarios

**En el host**

Actualiza el índice de paquetes y luego instala `nfs-kernel-server` para compartir directorios:

```bash
sudo apt update
sudo apt install nfs-kernel-server
```

**En el cliente**

Actualiza el índice de paquetes e instala `nfs-common` para la funcionalidad NFS:

```bash
sudo apt update
sudo apt install nfs-common
```

#### Paso 2: Crear directorios compartidos en el host

Ejemplo de un directorio compartido general y otro para directorios de inicio:

```bash
sudo mkdir -p /var/nfs/general
sudo chown nobody:nogroup /var/nfs/general
```

#### Paso 3: Configurar las exportaciones NFS en el host

Edita el archivo `/etc/exports` y agrega los directorios a compartir:

```bash
sudo nano /etc/exports
```

Añade las siguientes líneas:

```plaintext
/var/nfs/general    client_ip(rw,sync,no_subtree_check)
/home               client_ip(rw,sync,no_root_squash,no_subtree_check)
```

Aplica los cambios reiniciando el servidor NFS:

```bash
sudo systemctl restart nfs-kernel-server
```

#### Paso 4: Ajustar el firewall en el host

Permite el tráfico NFS en el puerto 2049 desde la IP del cliente:

```bash
sudo ufw allow from client_ip to any port nfs
```

#### Paso 5: Crear puntos de montaje en el cliente

Crea los directorios de montaje en el cliente:

```bash
sudo mkdir -p /nfs/general
sudo mkdir -p /nfs/home
```

Monta los intercambios desde el host:

```bash
sudo mount host_ip:/var/nfs/general /nfs/general
sudo mount host_ip:/home /nfs/home
```

Verifica el montaje:

```bash
df -h
```

#### Paso 6: Probar el acceso NFS

**Intercambio general**

Crea un archivo de prueba en `/nfs/general`:

```bash
sudo touch /nfs/general/general.test
ls -l /nfs/general/general.test
```

**Intercambio de directorio de inicio**

Crea un archivo de prueba en `/nfs/home`:

```bash
sudo touch /nfs/home/home.test
ls -l /nfs/home/home.test
```

#### Paso 7: Configurar montajes NFS en el arranque

Edita el archivo `/etc/fstab` en el cliente:

```bash
sudo nano /etc/fstab
```

Agrega los montajes:

```plaintext
host_ip:/var/nfs/general    /nfs/general   nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
host_ip:/home               /nfs/home      nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
```
Por último, se deja un enlace a la documentación utilizada para esta sección:

[How To Set Up an NFS Mount on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-20-04-es#paso-2-crear-los-directorios-compartidos-en-el-host)


**4.3.  Aplicar los archivo YAML.** 

Para la aplicación de la imagen de Drupal, utilizaremos la imagen proporcionada por Bitnami. A continuación se detalla la documentación oficial que se puede consultar para obtener la información del paso a paso para instalar está imagen en nuestro clúster:
Documentación oficial de Bitnami Drupal](https://hub.docker.com/r/bitnami/drupal)


### 4.4. Resultados




### Referencias










