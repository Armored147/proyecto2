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
- **`AWS CLI`** utilizado para conectarse al clúster y gestionar las configuraciones de `AWS`.
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

Para realizar la creación y configuración del entorno de desarollo en este caso, se debe seguir el tutorial disponible en el siguiente enlace de YouTube hasta el video número 8:

[Playlist del tutorial de Kubernetes en AWS](https://www.youtube.com/playlist?list=PLkqaOL-oB94HAIRkA_5qdqk-x-1Hgo4i2)

Es importante que, en el paso de creación del clúster, selecciones instancias `t3.small` para reducir costos, esto en el video `04-Tutorial. Crear un cluster Kubernetes en AWS. Crear los nodos`, al llegar al minuto `5:40`.

**Nota:** En el video `03 - Crear un clúster Kubernetes en AWS`, al llegar al minuto `1:42` durante la configuración del clúster, para fines prácticos, se omite el proceso de creación de roles y se utiliza el LabRole asignado por defecto. Del mismo modo, en el video `04 - Crear los nodos`, al llegar al minuto `1:35` en la configuración del grupo de nodos, utiliza `LabRole` en lugar de crear un nuevo rol.

**4.2.2. Uso de AWS CLI** 

Para terminos prácticos se sugiere hacer uso de `WSL` (Windows Subsystem for Linux) para realizar el trabajo con el clúster, por lo cuál se deja un paso a paso de lo que se debe realizar para dicha instalación y conexión:
1. Actualizar el sistema (opcional pero recomendado):
```bash
sudo apt update
sudo apt upgrade
```
2. Instalar AWS CLI:
```bash
sudo apt install awscli -y
```
3. Verificar la instalación: Asegúrate de que AWS CLI se haya instalado correctamente ejecutando:
```bash
aws --version
```
4. Configurar AWS CLI:
```bash
aws configure
```
Se te pedirá que ingreses:

- AWS Access Key ID: ingresa tu clave de acceso.
- AWS Secret Access Key: ingresa tu clave secreta.
- Default region name: ingresa la región (ej. us-west-2).
- Default output format: puedes dejarlo en blanco o ingresar json, text, o table.
**Nota:** Esta información se puede encontrar en la sección de `Launch AWS Academy Learner Lab`, en la sección de la consola denominada `i AWS Details` realizando click sobre el botón `Show` ubicado en el apartado de `AWS CLI:`
![image](https://github.com/user-attachments/assets/61af7bbf-bb74-476d-bfe3-416e74326091)

5. Configurar las credenciales: Realizamos el siguiente comando para modificar el archivo `credentials`, pegamos la información encontrada en la sección de `aws_session_token` del paso anterior, en el espacio con el mismo nombre que se encuentra en el archivo.
```bash
vim ~/.aws/credentials
```
6. Verificar conexión: Paea verificar la conexión podemos correr el siguiente comando que nos describirá las instancias EC2 de nuestro clúster:
```bash
aws ec2 describe-instances
```
Por último, se deja un enlace a la documentación oficial de AWS sobre AWS CLI:

[Documentación de instalación de AWS CLI en Linux](https://docs.aws.amazon.com/cli/v1/userguide/install-linux.html#install-linux-path)

**4.3. Configurar el sistema de archivos EFS** 

La capa de archivos debe quedar configurada con el servicio `EFS de AWS`, por lo que debes seguir el siguiente tutorial para que el servicio `EFS` pueda ser utilizado en el `cluster K8`:

[Crear un sistema de archivos EFS - Documentación del controlador CSI de AWS EFS](https://github.com/kubernetes-sigs/aws-efs-csi-driver/blob/master/docs/efs-create-filesystem.md)

**Nota:** Se debe seguir todos los pasos que se especifican a excepción del paso para determinar la dirección IP del grupo de nodos, lo cuál para este ejercicio es indiferente.

**Nota:** En la sección de creación del servicio EFS, en el paso b, al identificar el ID de cada subred en nuestra VPC, obtenemos una tabla como la siguiente:
```bash
|                           DescribeSubnets                          |
+------------------+--------------------+----------------------------+
| AvailabilityZone |     CidrBlock      |         SubnetId           |
+------------------+--------------------+----------------------------+
|  region-codec    |  192.168.128.0/19  |  subnet-EXAMPLE6e421a0e97  |
|  region-codeb    |  192.168.96.0/19   |  subnet-EXAMPLEd0503db0ec  |
|  region-codec    |  192.168.32.0/19   |  subnet-EXAMPLEe2ba886490  |
|  region-codeb    |  192.168.0.0/19    |  subnet-EXAMPLE123c7c5182  |
|  region-codea    |  192.168.160.0/19  |  subnet-EXAMPLE0416ce588p  |
+------------------+--------------------+----------------------------+
```
Esta tabla nos indica las diferentes subredes de nuestra VPC, para las cuales realizaremos el paso C en cada una de las subredes especificadas allí.
Por último crearemos el `access point` para nuestro servicio `EFS` en `AWS` para esto.
1. Buscaremos el servicio EFS en la sección de `servicios` de `AWS`
![image](https://github.com/user-attachments/assets/0857ad13-a6ea-468b-848f-0ae5ff87b60d)
2. Estando ya en el servicio de `EFS` iremos a la parte izquierda y daremos click en `Access points`, esto nos redirigirá a un nuevo apartado dónde haremos click en `Create access point`.
![image](https://github.com/user-attachments/assets/d2bba492-4ac4-49c3-9da0-c744261ce3fc)
3. Por último configuraremos el access point con base en los siguientes parametros:
![image](https://github.com/user-attachments/assets/f49f58a8-af4c-4589-9880-13a41772b12b)

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










