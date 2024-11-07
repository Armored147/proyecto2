# Tópicos Especiales en Telemática: C2466-ST0263-1716

## Estudiantes
- **Isis Catitza Amaya Arbeláez**, [icamayaa@eafit.edu.co](mailto:icamayaa@eafit.edu.co)
- **Santiago Alberto Rozo Silva**, [sarozos@eafit.edu.co](mailto:sarozos@eafit.edu.co)
- **Santiago Alberto Rozo Silva**, [sarozos@eafit.edu.co](mailto:sarozos@eafit.edu.co)
- **Santiago Alberto Rozo Silva**, [sarozos@eafit.edu.co](mailto:sarozos@eafit.edu.co)
  
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

![Add a heading](https://github.com/user-attachments/assets/66f55aa4-00fa-437d-909d-62a823a43b50)


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
- `NGINX Ingress Controller` configurado como balanceador de carga para distribuir el tráfico entre las instancias de `Drupal` en el clúster.
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
Kubernetes control plane is running at https://0713612045000A113B198A3B9AAEB7BD.gr7.us-east-1.eks.amazonaws.com
CoreDNS is running at https://0713612045000A113B198A3B9AAEB7BD.gr7.us-east-1.eks.amazonaws.com/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```

Luego comprobamos la información general de los pods con el siguiente comando:

```bash
kubectl get nodes -o wide
```

En nuestro caso la inforación arrojada de esta operación es la siguiente:

```bash
NAME                              STATUS   ROLES    AGE   VERSION               INTERNAL-IP       EXTERNAL-IP      OS-IMAGE         KERNEL-VERSION                  CONTAINER-RUNTIME
ip-192-168-11-45.ec2.internal     Ready    <none>   15h   v1.31.0-eks-a737599   192.168.11.45     44.203.226.125   Amazon Linux 2   5.10.226-214.880.amzn2.x86_64   containerd://1.7.22
ip-192-168-219-158.ec2.internal   Ready    <none>   15h   v1.31.0-eks-a737599   192.168.219.158   <none>           Amazon Linux 2   5.10.226-214.880.amzn2.x86_64   containerd://1.7.22
ip-192-168-222-225.ec2.internal   Ready    <none>   15h   v1.31.0-eks-a737599   192.168.222.225   <none>           Amazon Linux 2   5.10.226-214.880.amzn2.x86_64   containerd://1.7.22
```

### 4.2. Configuracion del clúster de Kubernetes y uso de AWS CLI.

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

**4.2.2. Configuración de NFS entre Servidor y Cliente** 

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

**4.4. Instalar controladores para EFS** 

 Instalaremos los controladores necesarios para que EFS pueda ser utilizado en el clúster.
1. Instalamos Helm:
```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
2. Agregar el repositorio del controlador CSI de EFS y actualizar Helm:
```bash
helm repo add aws-efs-csi-driver https://kubernetes-sigs.github.io/aws-efs-csi-driver/
helm repo update
```
3. Instalar el controlador CSI de Amazon EFS:
```bash
helm upgrade --install aws-efs-csi-driver --namespace kube-system aws-efs-csi-driver/aws-efs-csi-driver
```
Esto desplegará el controlador CSI de EFS en tu clúster EKS, lo que te permitirá utilizar EFS para el almacenamiento persistente en tu despliegue de WordPress. De igual forma se deja la información oficial de Helm en el siguiente enlace:

[Instalación de Helm](https://helm.sh/docs/intro/install/)

**4.5.  Instalar ingress-nginx en el cluster.** 

Para utilizar el balanceador de carga con nginx es necesario tener instalado en el cluster ingress-nginx como se explica a continuación:

1. Aplicar el manifiesto de Ingress NGINX.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
2. Verificar que el pod del controlador de Ingress esté en ejecución.

```bash
kubectl get pods --namespace ingress-nginx
```
3. Comprobar que el controlador Ingress NGINX tenga una IP pública asignada.

```bash
kubectl get service ingress-nginx-controller --namespace ingress-nginx
```

**4.6.  Aplicar los archivo YAML.** 

Para la aplicación de la imagen de Drupal, utilizaremos la imagen proporcionada por Bitnami. A continuación se detalla la documentación oficial que se puede consultar para obtener la información del paso a paso para instalar está imagen en nuestro clúster:
Documentación oficial de Bitnami Drupal](https://hub.docker.com/r/bitnami/drupal)

**4.7. Certificado SSL.**

En esta sección, abordaremos el proceso de creación de un certificado SSL para asegurar la comunicación entre el servidor y los clientes. Para usos prácticos haremos uso de la plataforma Let's Encrypt para proteger la información sensible y garantizar la integridad de los datos transmitidos, para ello seguiremos los siguientes pasos:

1. Instalar cert-manager:
 ```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml
```
2. Verifica que cert-manager está en ejecución:
 ```bash
kubectl get pods --namespace cert-manager
```
3. Configurar un ClusterIssuer para `Let's Encrypt`:
Crea un recurso  `ClusterIssuer` para `Let's Encrypt`, que permite emitir certificados SSL para los dominios del clúster. Crea un archivo YAML llamado `letsencrypt-clusterissuer.yaml` con el siguiente contenido:
 ```bash
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: tu-email@example.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: nginx

```
**Nota:** Reemplaza `tu-email@example.com` con tu dirección de correo electrónico para recibir notificaciones relacionadas con la renovación del certificado.

4. Aplica el archivo `ClusterIssuer`:
 ```bash
kubectl apply -f letsencrypt-clusterissuer.yaml
```
5. Solicitar un certificado SSL:
Crea un archivo `YAML` llamado `certificate.yaml` con el siguiente contenido, reemplazando `tudominio.com` con tu dominio.
 ```bash
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: tudominio-cert
  namespace: default
spec:
  secretName: tudominio-tls
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
  commonName: tudominio.com
  dnsNames:
  - tudominio.com
```
6. Aplica el archivo Certificate:
 ```bash
kubectl apply -f certificate.yaml
```
Cert-Manager ahora solicitará automáticamente un certificado de Let's Encrypt para tu dominio y almacenará el certificado y la clave privada en el secreto tudominio-tls en el espacio de nombres default.

7. Verificar el estado del certificado
 ```bash
kubectl describe certificate tudominio-cert --namespace default
```
Por último en el siguiente enlace se deja la información oficial sobre Let's Encrypt:
[Let's Encrypt - Página Oficial](https://letsencrypt.org/getting-started/)


### 4.7. Resultados




### Referencias










