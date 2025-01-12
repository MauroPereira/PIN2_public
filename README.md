
# PIN2_public
## Objetivo
### gh-tf-mio

lanzando v10 con buckets y dinamodb!!

(HINT. armar el bucket S3, y la base dynamodb a mano).

SI se quiere terrafonear, mejor.
Lanzando


# Terraform AWS Webserver Setup

Este proyecto usa **Terraform** para crear y configurar una infraestructura básica en **AWS**. La infraestructura incluye una **VPC**, **subred**, **Internet Gateway**, una **instancia EC2** con Apache, y la configuración del backend para el estado de Terraform en **S3** con bloqueo mediante **DynamoDB**.

## Requisitos

- **Terraform** instalado.
- Una cuenta en **AWS** con permisos suficientes para crear recursos como VPC, EC2, S3, DynamoDB, etc.
- El archivo de configuración `aws_access_key_id` y `aws_secret_access_key` configurado en tu entorno o en el archivo `~/.aws/credentials`.

## Estructura del Proyecto

Este proyecto se divide en varios archivos que describen cómo se deben crear los recursos de AWS.

### Archivos principales:

- **`setup.tf`**: Define los recursos básicos de infraestructura, como VPC, Internet Gateway, subred y grupo de seguridad.
- **`main.tf`**: Crea y configura la instancia EC2 y asigna el script de configuración de Apache.
- **`backend.tf`**: Configura el backend remoto para almacenar el estado de Terraform en S3 y utiliza DynamoDB para el bloqueo del estado.
- **`create_apache.sh`**: Script que se ejecuta cuando la instancia EC2 se lanza, instalando y habilitando Apache.

## Recursos Creados

1. **VPC**: Crea una red privada con el bloque CIDR `10.0.0.0/16`.
2. **Internet Gateway (IGW)**: Proporciona acceso a Internet para los recursos dentro de la VPC.
3. **Tabla de Rutas**: Se configura una ruta predeterminada que permite el acceso a Internet usando el Internet Gateway.
4. **Subred**: Crea una subred con el bloque CIDR `10.0.1.0/24`.
5. **Security Group**: Permite el tráfico entrante en los puertos `22` (SSH) y `80` (HTTP).
6. **Instancia EC2 (Web Server)**: Lanza una instancia de Amazon Linux 2 con Apache instalado y configurado para iniciar automáticamente.
7. **Backend de Terraform**: El estado de Terraform se almacena en un bucket de S3, y el bloqueo de estado se gestiona mediante DynamoDB.

## Configuración del Backend (S3 y DynamoDB)

El estado de Terraform se guarda en un bucket de **S3** y se utiliza una tabla de **DynamoDB** para el bloqueo del estado, lo cual permite trabajar en equipo de manera segura.

## Pasos para Implementar

1. **Clonar el repositorio**

   git clone https://github.com/tu-usuario/terraform-webserver-setup.git
   cd terraform-webserver-setup

2. **Inicializar el proyecto Terraform**

   Esto descarga los proveedores necesarios y configura el backend:

   terraform init

3. **Revisar el plan de ejecución**

   Verifica qué recursos se crearán con el siguiente comando:

   terraform plan

4. **Aplicar los cambios**

   Crea los recursos en AWS ejecutando:

   terraform apply

   Terraform te pedirá confirmación. Escribe `yes` para proceder.

5. **Acceder a la Instancia EC2**

   Una vez que Terraform haya terminado de crear los recursos, puedes acceder a la **instancia EC2** utilizando la IP pública que se imprime como resultado.

## Archivos de Configuración

### `setup.tf`

Este archivo define la infraestructura básica en AWS, incluyendo la creación de una VPC, un Internet Gateway, una tabla de rutas, una subred y un grupo de seguridad.

### `main.tf`

Crea una instancia EC2 basada en la AMI `ami-00c39f71452c08778` (Amazon Linux 2) y asocia el grupo de seguridad previamente creado. Se configura para ejecutar el script de Apache al iniciar.

### `backend.tf`

Configura el almacenamiento remoto de estado en un bucket S3 y utiliza DynamoDB para gestionar el bloqueo del estado.

### `create_apache.sh`

Este script se ejecuta en la instancia EC2 para instalar y configurar el servidor web Apache:

#! /bin/bash
sudo yum update -y
sudo yum install -y httpd.x86_64
sudo systemctl enable httpd --now

## Salidas

Al final de la ejecución, Terraform imprimirá la **IP pública** de la instancia EC2:

output "Webserver-Public-IP" {
  value = aws_instance.webserver.public_ip
}
```

## Limpieza de Recursos

Para eliminar los recursos creados por Terraform, puedes ejecutar el siguiente comando:

terraform destroy

Terraform te pedirá confirmación. Escribe `yes` para proceder.

## Notas

- Asegúrate de tener configuradas tus credenciales de AWS en tu entorno.
- Si es necesario, puedes personalizar la AMI de la instancia EC2 o cambiar el tipo de instancia.
- Este proyecto está diseñado para crear una infraestructura básica. Puedes ampliarlo según tus necesidades.

## Contribución

Si tienes alguna mejora o corrección, no dudes en hacer un **Pull Request**. ¡Estaré encantado de revisarlo!

---

¡Eso es todo! Ahora deberías poder configurar la infraestructura básica en AWS con Terraform y desplegar tu servidor web de manera sencilla.
