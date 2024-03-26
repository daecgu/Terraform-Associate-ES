# Fundamentos de Terraform

Completa los tutoriales ["Get Started"](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) para crear modificar y destruir tu primera infraestructura utilizando Terraform y aprender acerca de los Terraform providers y los conceptos básicos de Terraform state.
<details>
<summary> Tutoriales "Get Started" </summary>

---------------------------------------------------------------------------
Algunos tutoriales tienen laboratorios interactivos, o tienen su documentación para Windos/Linux/Mac. EN ESTE PROYECTO DE TRADUCCIÓN únicamente cubriré Linux y en concreto Ubuntu/Debian, el resto quedan en los enlaces a la la página original que traduzco. 

### [¿Qué es infraestructura como código con Terraform?](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code)
<details>
Este tutorial está traducido en la sección [Aprende acerca de la infraestructura como código (IAC)](https://github.com/daecgu/Terraform-Associate-ES/blob/main/Learn_about_infrastructure_as_Code.md#introducucci%C3%B3n-a-la-infraestructura-como-c%C3%B3digo-con-terraform---httpsdeveloperhashicorpcomterraformtutorialsaws-get-startedinfrastructure-as-code). Por lo tanto no lo traduciré nuevamente en esta sección.

Contiene un Laboratorio Interactivo.
</details>

### [Instalación de Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
<details>
Para utilizar Terraform es necesario instalarlo. HashiCorp distribuye Terraform como un paquete binario. También puedes instalar Terraform utilizando administradores de paquetes.

HashiCorp Oficialmente mantiene y firma oficialmente paquetes para las siguientes distribuciones Linux: Ubuntu/Debian, CentOS/RHEL, Fedora y Amazon Linux. 
Asegurate que el sistema esté actualizado y tengas instalado ```gnupg```, ```software-properties-common``` y ```curl```. Utilizaremos estos paquetes para verficar la firma de HashiCorp e instalar el paquete de respositorios Debian.

```sh
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```
Instalar la [GPG Key](https://apt.releases.hashicorp.com/gpg) de HashiCorp:

```sh
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```
Verificar la clave:
```sh
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```
El comando ```GPG``` reportará la huella digital de la clave:
```sh
/usr/share/keyrings/hashicorp-archive-keyring.gpg
-------------------------------------------------
pub   rsa4096 XXXX-XX-XX [SC]
AAAA AAAA AAAA AAAA
uid           [ unknown] HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 XXXX-XX-XX [E]
```

Añade el repositorio oficial de HashiCorp al sistema. El comando ```lsb_release -cs``` encuentra la distribución publicada para tu sistema, como ```buster```, ```groovy``` o ```sid```.
```sh
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```
Descarga la información del paquete de HashiCorp
```sh
sudo apt update
```
Instala Terraform desde el Repositorio:
```sh
sudo apt-get install terraform
```
#### Verifica la instalación:
Verifica que la instalación ha funcionado abriendo una nueva sesión de terminal y listando los subcomandos de Terraform disponibles:
```sh
terraform -help
```
Añade cualquier subcomando a ```terraform -help``` para aprender más acerca de lo que hace y las opciones disponibles.
```sh
terraform -help plan
```

#### Resolución de problemas.
Si tienes un error indicando que ```terraform``` no se ha encontrado, tu variable de entorno ```PATH``` no se ha configurado correctamente. Asegurate de que tu variable ```PATH``` contiene el directorio en el que Terraform ha sido instalado. 

#### Habilita el auto-completado mediante tabulador
Si utilizas tanto Bash como Zsh, puedes habilitar el autocompletado para los comandos de Terraform. Para habilitarlo, primero asegurate que el el archivo de configuración existe para tu shell seleccionada. 
Para ello comprueba que existen los archivos ```~/.zshrc``` o ```~/.bashrc```.

Ahora instala el paquete de autocompletado: 
```sh
terraform -install-autocomplete
```
Una vez que está enstalado el autocompletado, necesitarás reiniciarl la Shell.

#### Tutorial de inicio Rápido
Ahora que hemos instalado Terraform, vamos a provisionar un servidor NGINX en menos de un minuto utilizando Docker en Linux.
Es preciso tener instalado [Docker Engine](https://docs.docker.com/engine/install/) para poder continuar con este tutorial.

Crea un directorio llamado ```learn-terraform-docker-container```.
```sh
mkdir learn-terraform-docker-container
```
En este directorio de trabajo albergaremos los archivos de configuración que describen la infraestructura que deseamos que Terraform cree y administre. Cuando inicializas y aplicas la configuracíon aquí, Terraform utiliza este directorio para guardar los plugins, modulos y la información acerca de la infraestructura real que ha creado.

Vamos al directorio en le que queremos trabajar:
```sh
cd learn-terraform-docker-container
```

En el directorio de trabajo, crea un archivo llamado ```main.tf``` y pégalo en la siguiente configuración de Terraform en él. 

```terraform
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}

```

Inicializa el proyecto, lo que hará que descargue un "provider" que permite a Terraform interactuar con Dcoker.
```sh
terraform init
```
Vamos a desplegar el contenedor de servidor NGINX con ```apply```. Cuando Terrafom pregunte por la confirmación deberemos responder ```yes``` y presionar ```enter```.
```sh
terraform apply
```
Verifica que el contenedor Nginx esté funcionando correctamente visitando <a href="http://localhost:8000">localhost:8000</a> en tu navegador web o ejecuta el comando ```docker ps``` para ver el contenedor.

<img src="https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dtutorials%26version%3Dmain%26asset%3Dpublic%252Fimg%252Fterraform%252Fgetting-started%252Fterraform-docker-nginx.png%26width%3D2048%26height%3D510&w=2048&q=75" width="900" height="200">

```sh
$ docker ps
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
425d5ee58619        e791337790a6              "nginx -g 'daemon of…"   20 seconds ago      Up 19 seconds       0.0.0.0:8000->80/tcp     tutorial
```
Ahora vamos a parar el contenedor utilizando el siguiente comando:
```sh
terraform destroy
```
Ya has desplegado y destruido un servidor web NGINX con Terraform. 
</details>

### [Construye infraestructura](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build)
<details>
Una vez instalado Terraform ya estás preparado para crear tu primera infraestructura.

En este tutorial vas a desplegar una instancia EC2 en Amazon Web Services (AWS). Las instancias EC2  son máquinas virtuales que corren en AWS. Son un componente común en muchos proyectos. 

#### Prerequisitos:
Para poder realizar este tutorial necesitarás:
- [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) instalado.
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) instalado.
- [Cuenta de AWS](https://aws.amazon.com/free) y [credenciales asociadas](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html) que te permitirán crear recursos. 

Para utilizar las credenciales IAM para autenticar al Terraform AWS provider, establece la variable de entorno ```AWS_ACCESS_KEY_ID``` y tu clave ```AWS_SECRECT_ACCESS_KEY```.

```sh
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
```

Este tutorial utilizará recursos que estén dentro de la categorización [AWS free tier](https://aws.amazon.com/free/). Si tu cuenta no califica para los recursos gratuitos, no somos responsables de ningún cargo en el que puedas incurrir. 

#### Escribe la Configuración
El conjunto de archivos utilizado apra describir la infraestructura en Terraform se conoce como configuración Terraform (Terraform configuration). Escribirás tu primera configuración para definir una instancia AWS EC2. 

Cada Configuración Terraform debe ir en su propio directorio de trabajo. Crear un directorio para tu configuración.
```sh
mkdir learn-terraform-aws-instance
```

Muevete al directorio:
```sh
cd learn-terraform-aws-instance
```

Crear un fichero para definir tu infraestructura:
```sh
touch main.tf
```

Abre ```main.tf``` en tu editor de texto, copia la configuración y guarda el archivo. 

```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```
Esta es una configuración completa que puedes desplegar con Terraform. Ahora explicaremos cada bloque de la configuración en más detalle.

#### Bloque Terraform
El bloque ```terraform {}``` contiene las configruaciones de Terraform, incluyendo los providers de Terraform que utilizaremos para aprovisionar la infraestructura. Para cada provider, el atributo ```source``` define un hostname, namespace y el tipo de proveedore opcionales. Terraform instala los providers del [Terraform Registry](https://registry.terraform.io/) por defecto. En este ejemplo de configuracion, el ```aws``` provider source está definido como ```hashicorp/aws```, el cual es una abreviatura de ```registry.terraform.io/hashicorp/aws```.

También puedes establecer una versión para cada provider definido en el bloque ```required_provders```. El atributo ```version``` es opcional, pero se recomienda utilizarlo de manera que terrafrom no instale una version que no funcione con tu configuración. Si no especificas una versión del provider, Terraform automáticamente descargará la versión más reciente durante la inicialización. 

Para aprender más dirigete a [provider source documentation](https://developer.hashicorp.com/terraform/language/providers/requirements)

#### Bloque Provideres
El bloque ```provider``` configura un provider especifico, en este caso ```aws```. Un provider es un plugin que Terraform utiliza para crear y manejar los recursos.

Puedes utilizar múltiples bloques de provider en tu configuración Terrafrom apra administrar recuross de distintos providers. Puedes incluso utilizar diferrentes providers juntos. Por ejemplo, puedes pasar la IP de tu instancia AWS EC2  para monitorizar el recurso desde DataDog.

#### Bloque Resources:
Utiliza los bloques de ```resource``` para definir componentes de tu infraestructura. Un recurso puede ser un componente virtual o físico, como una instancia EC2, o puede ser un recurso lógico como una aplicación Heroku. 

Los bloques de recursos tienen dos grupos de "string" antes del bloque: el tipo del recurso y el nombre del recurso. En este ejemplo, el tipo del recurso es ```aws_instance``` y el nombre es ```app_server```. El prefijo del tipo señala el nombre del provider. En la configuración e ejemplo, Terraform adminsitra el recurso ```aws_instance```  con el ```aws``` provider. Juntos, el tipo del recurso y el nombre del recurso froman un ID único para el recurso. Por ejemplo, el ID para la instancia EC2 es: ```aws_instance.app_server```.

Los bloques de recurso contienen argumentos que utilizas para configurar el recurso. Argumentos pueden contener cosas como: tamaño de máquina, imágenes de Disco, VPC IDs. Nuestra [referencia de providers](https://developer.hashicorp.com/terraform/language/providers) indica los argumentos opcionales y requeridos para cada recurso. Para la instancia EC2, la configuración de ejemplo establece la AMI ID una imagen de Ubuntu, y el tipo de instancia a ```t2.micro```, que califica dentro del rango gratuito de AWS. Además establece una etiqueta para darle un nombre a la instancia. 

#### Inicializa el directorio.
Cuando creas una configuración nueva -- or compruebas una configuración existente desde con ocntrol de versiones -- necesitas inicializar el directorio con ```terraform init```.

Inicializar un directorio de configuración descarga e instala los providers definidos en la configuración, en este caso ```aws``` provider.

Inicializa el directorio:

```sh
terraform init
```

Terraform descarga el provider ```aws``` y lo instala en unsubdirectorio oculto de tu directorio de trabajo llamado ```.terraform```. El comando ```terraform init``` indica qué versión del provider ha sido instalada. Terraform además crear a un archivo denominado ```.terraform.lock.hcl``` que especifica la versión exacta del provider, de manera que puedas controlar cuando quieres actualizar el privider utilizado para el proyecto. 

#### Da formato y valida la configuración
Recomendamos utilizar un formato consistente en todos tus archivos de configuración. El comando ```terraform fmt``` automáticamente actualiza las configuraciones en el directorio actual para que tengan una consistencia y legibilidad.

Da formato a la configuración. Terraform mostrará los nombres de los archivos que han sido modificado. En este caso tu archivo de configuración tenía el formato correcto, por lo que Terraform no devolverá ningún nombre.

```sh
terraform fmt
```

Puedes estar seguro de que tu configuración es sintácticamente válida y consistente internamente utilizando el comando ```terraform validate```.

Valida tu configuración. El ejemplo de configuración aportado es válido, por lo tanto Terraform  devolverá un mensaje de éxito. 

```sh
terraform validate
```

#### Crea la infraestructura
Aplica la configuración con el comando ```terraform apply```. 

Antes de aplicar ningún cambio, Terraform muestra el plan de ejecución que describe las acciones que Terraform realizará para actualizar la infraestructura para que coincida con la configuración.

El formato de la salida es similar al formato de ```diff``` generado por herramientas como git. La salida tiene un ```+``` al lado de ```aws_instace.app_server```, lo que significa que Terraform creará este recurso. Debajo de eso muestra los atributos que se establecerán. Cuando un valor mostrado es ```(known after apply)``` significa que el valor no es conocido hasta que el recurso es creado. Por ejemplo AWS asigna los Amazon Resource Names (ARNs) a las instancias cuando las crea, por lo que Terraform no puede saber el valor del atributo ```arn``` hasta que no se apliquen los cambios y el AWS provider devuelva el valor desde la AWS API.

Terraform ahora se pausa y espera ser aprobado antes de proceder. Si algo del plan parece incorrecto o  peligroso, es seguro abortar aquí antes de que Terraform modifique la infraestructura. 

En este caso el plan es aceptable, por lo que es preciso confirmar con un ```yes``` para proceder. El plan de ejecución tarda un tiempo hasta que la instancia EC2 está disponible. 

```sh
  Enter a value: yes

aws_instance.app_server: Creating...
aws_instance.app_server: Still creating... [10s elapsed]
aws_instance.app_server: Still creating... [20s elapsed]
aws_instance.app_server: Still creating... [30s elapsed]
aws_instance.app_server: Creation complete after 36s [id=i-01e03375ba238b384]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

Has creado infraestructura utilizando Terraform. Puedes visitar la consola EC2 y encontrar tu instancia. 

#### Inspecciona el esatdo
Cuando la configuración ha sido aplciada, Terraform escribe datos en un fichero denominado ```terraform.tfstate```. Terraform guarda los IDs y propiedades de los recursos que administra en este archivo, de manera que pueda atualizar o destruir esos recursos en adelante.

El archivo Terraform state es la única manera en la que Terraform puede hacer seguimento de los recursos que administra, y habitualmente contiene información sensible, por lo que debes guardar este archivo de estado de forma segura y con acceso restringido a aquellos miembros del equipo que deben administrar la infraestructura. En producción recomendamos [guardar tu archivo de estado remotamente](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-migrate) con Terraform Cloud o Terraform Enterprise. Terraform también soporta otros "[remote backends](https://developer.hashicorp.com/terraform/language/settings/backends/configuration)" que puedes utilizara para almacernar y administrar tu archivo state. 

Inspeciona el estado actual utilizando ```terraform show```.

Cuando Terraform crea esta instancia Ec2, genera los metadatos del recurso desde el AWS provider y escribe los metadatos en este archivo state. En tutoriales posteriores, modificarás tu configuración para hacer referencia a estos valores y configurar otros recursos y valroes de salida.

#### Administrar manualmente el archivo State. 
Terraform tiene un comando denominado ```terraform state``` para administración avanzada del archivo state. Utiliza el subcomando ```list``` para listar los recursos de tu proyecto. 

#### Solución de Problemas
Si ```terraform validate``` ha sido exitosa y tu ```apply``` ha fallado, puede que te encuentres uno de estos errores comunes:
- Si utilizaste una región equivocada (en este caso una región diferente a ```us-west-2```), deberaś cambiar tu ```ami```, dado que AMI IDs son especificos de la región. Elige un AMI ID especifico para tu región siguiendo [estas instrucciones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html#finding-quick-start-ami), y modifica ```main.tf``` con este id. Entonce vuelva a lanzar ```terraform apply```.
- si no tienes una VPC por defecto en tu cuenta AWS in la región correcta, navega a AWS VPC Dashboard en la interfaz web, crea una nueva VPC en tu región y asocia una subnet y un grupo de seguridad a esa VPC. Entonces añadie el ID del grupo de seguridad  (```vpc_security_gorup_ids```) y el OD de la subred (```subnet_id```) como argumentos al recurso ```aws_instance```", y reemplaza los valoers con los nuevos de tu grupo de seguridad y subnet.

```terraform
 resource "aws_instance" "app_server" {
   ami                    = "ami-830c94e3"
   instance_type          = "t2.micro"
+  vpc_security_group_ids = ["sg-0077..."]
+  subnet_id              = "subnet-923a..."
 }
```

Guarda los cambios en main.tf, y vuelve a lanzar el comando ```terraform apply```. 

Recuerda añadir estas líneas a tu configuración para tutoriales posteriores. Para más inforamción, mira [este documento](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html) de AWS para trabajar con VPCs. 

</details>

### [Cambiar infraestructura](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-change)
<details>
En el último tutorial, creaste tu primera infraestructura con Terraform: una instancia EC2 en AWS. En este tutorial, modificaras ese recurso y aprenderás cómo aplicar cambios a tus proyectos Terraform.

La infraestructura evoluciona constantemente, y Terraform te ayuda a administrar ese cambio. Conforme cambias las configuraciones de Terraform, Terraform construye un plan de ejecuión que solo modifica lo que es neceasrio para alcanzar el estado deseado. 

Cuando utilizamos Terraform en proudcción, se recomienda utilizar un sistema de control de versiones para administrar los archivos de configuración y guardar en un Backend remoto los archivos State, como por ejemplo Terraform Cloud o Terraform Enterprise.

#### Prerequisitos.
Este tutorial asume que estás continuando de los tutoriales anteriores. Sino, sigue los pasos siguiente antes de continuar.
- Instala Terraform CLI y AWS CLI, como se describe en el último tutorial.
- Crea un directorio llamado ```learn-terraform-aws-instance``` y copia pega la siguiente configuración en un archivo llamado ```main.tf```.

```terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```
- Inicializa la configuración mediante el comando ```terraform init```.
- Aplica la configuración mediante el comando ```terraform apply``` y responde a la solicitud de confirmación con ```yes```.

Una vez que has aplicado exitosamente la confiuración, puedes continuar con el resto del tutorial.

#### Configuración 
Ahora actualiza el ```ami``` de tu instancia. Cambia el recurso ```aws_instance.app_server``` dentro del bloque resource en ```main.tf``` reemplazando el ami con uno nuevo.

```diff
 resource "aws_instance" "app_server" {
-  ami           = "ami-830c94e3"
+  ami           = "ami-08d70e59c07c61a3a"
   instance_type = "t2.micro"
 }
```


Esta actualización cambia el AMI a un Ubuntu 16.04 AMI. El AWS provider sabe que no puede cambiar el AMI de una instancia después de que haya sido creada, por lo que Terraform destruirá la instancia vieja y creará una nueva.

#### Aplica los cambios
Después de cambiar la configuración, ejecuta el comando ```terraform apply``` nuevamente verás cómo Terraform aplica este cambio en los recursos existentes. 

El prefijo ```-/+``` significa que Terraform destruirá y recreará el recurso en vez de actualizar el recurso. Terraform puede actualizar algunos atributos (indicando con el prefijo ```~```), pero cambiar el AMI para una instancia EC2 requiere recrearla. Teerraform maneja estos detalles por ti, y la ejecución muestra lo que Terraform hará. 

Adicionalmente, el plan de ejecución muestra que el cambio de AMI es lo que fuerza a Terraform a sustituir la instancia. Utilizando esta información, puedes ajustar los cambios para evitar actualizaciones destructivas si es necesario. 

Nuevamente, Terraform pregunta por autorización para el plan de ejcución antes de proceder. Responde ```yes``` para que se ejecuten todos los pasos planeados. 

Como se ha indicado en el plan de ejecución, Terraform primeramente destruye la instancia existente y luego crea una nueva en su sitio. Puedes usar ```terraform show``` para que Terraform muestre los nuevos valores asociados al a instancia. 
</details>

### [Destruye infraestuctura](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-destroy)
<details>
Ya has creado y actualizado una instnacia EC2 en AWS con Terraform. En este tutorial utilizarás Terraform para destruir esta infraestuctura.

Una vez que no necesites más esta infraestructura, puedes uqerer destruirla para reducir costos y vulnerabilidades de seguridad. Por ejemplo, si quieres eliminar un entorno de producción de un servicio, o administrar entornos de vida cortos o sistemad de pruebas. Además de construir y modificar infraestuctura, Terraform puede destruir o recrear la infraestuctura que gestiona.

#### Destruye
El comando ```terraform destroy``` termina con los recursos manjeados por nuestro proyecto de Terraform. Este comando es el inverso a ```terraform apply```, en el que se termina con todos los recurso especificados en el Terraform State. No destruye recursos que no estén administrado por el proyecto atctual de Terraform. 

Destruye los recursos creado mediante el comando ```terraform destroy```.
El prefijo ```-``` indica que esa instancia será destruida. De igual manera que en apply, terraform muestra su plan de ejecución y espera autorización antes de aplicar cualquier cambio.

Responde ```yes``` para ejecutar este plan y destruir la infraestuctura. 

De igual manera que con ```apply```, Terraform determina el orden en el que se destruyen los recursos. En este caso, Terraform identifica una única instancia sin ninguna dependencia, por lo que destruye la instancia. En casos más complicados con múltiples recursos, Terraform los destruirá en un orden adecuado para respetar las dependencias.
</details>

### [Define variables de entrada](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-variables)

<details>
Los ejemplos que hemos realizado hasta ahora utilizan valores "hard-coded". Las configuraciones Terraform puede incluir variables para hacer tu confirguración más dinámica y flexible.

#### Prerequisitos
- Tener un directorio llamado ```learn-terraform-aws-instance``` con la siguiente configuración en un fichero llamado ```main.tf```:
```terraform
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-08d70e59c07c61a3a"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```
Asegurate que tu configuración coincide con esta, y que has ejecutado el comando ```terraform init``` en el directorio ```learn-terraform-aws-instance```.

####Establece el nombre de la instancia con una variable
La configuración actual incluye una serie de valores "hard-coded". Las variables Terraform permite escribir configuraciones que son flexibles y fáciles de reutilizar.

añade una variable que defina el nombre de la instancia. Para ello:
Crea un nuevo archivo llamado ```variables.tf``` con un bloque que defina una nueva variable ```instance_name```.

```terraform
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "ExampleAppServerInstance"
}
```

Date cuenta de que Terraform carga todos los archivos en el directorio actual que terminen en ```.tf```, por lo que puedes nombrar tus archivos de configuración como desees. 

En ```main.tf```, actualiza el bloque de recurso ```aws_instance``` para utilizar la nueva variable. El nombre de instancia ```instance_name``` tomará su valor por defecto a menos que declares un valor diferente. 

#### Aplica tu configuración
Aplica la configuración y responde a la solicitud de confirmación ```yes```.

Ahora aplica la configuración de nuevo, pero esta vez vamos a sobreescribir el valor por defecto del nombre de la instancia utilizando ```-var``` flag.  Terraform Actualizara la etiqueta ```Name``` con el nuevo nombre. Recuerda responder al prompt de confirmación con un ```yes```.

```sh
terraform apply -var "instance_name=YetAnotherName"
```

Establecer variables a través de la línea de comandos no guardará estos valores. Terraform soporta muchas maneras de utilizar y establecer variables, e manera que puedas evitar introducirlas repetidasveces conforme vas ejecutando comandos. Para aprender más, sigue el tutoria en profundidad: [Customize Terraform Configuration with Variables](https://developer.hashicorp.com/terraform/tutorials/configuration-language/variables)

</details>

### [Consulta de datos mediante "outputs"](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-outputs)
<details>

En el tutorial anterior, utilizamos una variable de entrada para parametrizar la coriguración de Terraform. En este tutorial utilizaremos "output values" para presentar información útil para el usuario de Terraform. 

Si no has terminado el tutorial anterior, hazo antes de continuar con este.

#### Output EC2 instance configuration
Crea un archivo llamado ```outputs.tf``` en tu carpeta ```learn-terraform-aws-instance```.

Agrega la siguiente configuración al fichero ```outputs.tf``` para definir las salidas para tu instancia EC2: ID y dirección IP.

```terraform
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.app_server.id
}

output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.app_server.public_ip
}
```

#### Inspecciona los valores de salida
Debes aplicar los cambios en la configuración antes de poder utilizar estos output values. Aplica la configuración mediante el commando ```terraform apply``` y responde ```yes``` a la solicitud de confirmación.

Terraform mostrará los output values en la pantalla cuando apliques tu configuración. Puedes solicitar los valores output mediante el comando ```terraform output```. 

Puedes utilizar los Terraform outputs para conectar tus proyectos Terraform con otras partes de tu infraestructura o con otros proyectos Terraform. Para aprender más, sigue el tutoria len profundidad: [Output Data from Terraform](https://developer.hashicorp.com/terraform/tutorials/configuration-language/outputs)

#### Destruye tu infraestructura.
Destruye tu infrasestructura mediante el comando ```terraform destroy``` a no ser que planees continuar con los tutoriales que siguen. Responde a la confirmación ```yes```.

</details>

### [Guarda el archivo State remotamente](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-remote)
<details>
Ahora que hemos consturido, cambiado y destruido la infraestructura de tu sistema local. Esto está bien para testing y desarrollo, pero en entornos de producción deberías tener tu archivo State seguro y encriptado, donde tus compañeros tenga acceso para colaborar en la infraestructura. El mejor modo de hacer esto es correr Terraform en un entorno remoto con acceso compartido al archivo State.

[Terraform Cloud](https://cloud.hashicorp.com/products/terraform) permite a los miembros del equipo versionado, auditado y colaboración en cambios de infraestructura. Permite almaenar variables, incluidos API tokens y Access Kyes, y facilita un entorno seguro y estable para largos procesos de ejecución de Terraform. 

En este tutorial migraras tu State a Terraform Cloud. 

#### Prerequisitos
Este tutorial asume que has completado los anteriores. 

#### Establece Terraform Cloud
Si tienes una cuenta de HashiCorp Cloud Platform o Terraform Cloud, accede utilizando tus credenciales existentes. Para información más detalla acerca de como registrarte y crear una organización puedes revisar este [tutorial](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-sign-up)

Después modifica ```main.tf``` añade al bloque ```cloud```a tu configuración Terraform y reemplaza el ```organization-name``` por el nombre de tu organización.

``` terraform
terraform {
  cloud {
    organization = "organization-name"
    workspaces {
      name = "learn-tfc-aws"
    }
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
}
```

#### Accede a Terraform Cloud
Accede a tu cuenta de Terraform Cloud desde el Terraform terminal mediante el comando ```terraform login```. Confirma con un ```yes``` ysigue el flujo de trabajo en la ventana del navegador que automáticamente se abrirá. Necesitaras pegar la API Key genearada en tu termina cuando se rquiera. Para más detalles puedes seguir este [tutorial](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-login).

#### Inicializa Terraform
Ahora que has configurado tu integración con Terraform Cloud, ejecuta el comando ```terraform init``` para reinicializar tu configuración y migrar tu archivo Stat a Terraform Cloud. Introduce ```yes``` cuando se te pregunte por confirmación para la migración.

Ahora que Terraform ha migrado el archivo State a Terraform Cloud, elimina el archivo State local. 

```sh
rm terraform.tfstate
```
 Cuando utilices Terraform Cloud con el flujo de trabajo CLI puedes elegir ejecutar Terraform remotamente o desde tu máquina local. Cuando utilizas la ejecución local, Terraform Cloud ejecutará Terraform en tu máquina y guardará remotamente el State File en Terraform Cloud. Para este tutorial utilizaremos la ejecución remota.

 #### Establece variables de trabajo.
 El comando ```terraform init``` crea el espacio de trabajo ```learn-tfc-aws``` en tu organización Terraform Cloud . Debes configurar tu espacio de trabajo con tus credenciales de AWS para autenticar el AWS provider.

Navega a tu espacio de trabajo ```learn-tfc-aws``` en Terraform Cloud y ve a la página de Variables. Debes Añadir tu  ```AWS_ACCESS_KEY_ID``` y ```AWS_SECRET_ACCESS_KEY``` como variables de entorno, asegurandote de marcarlas como "sensitive".

#### Aplica la configuración
Ahora ejecuta ```terraform apply``` disparar una ejecución en Terraform Cloud. Terraform mostrará que no hay cambios para hacer.

Esto significa que Terraform no ha detectado ninguna diferencia entre tu configuración y los recursos físicos existentes. Como resultado, Terraform no necesita hacer nada. 

Terraform ahora guadda tu archivo State remotamente en la Terraform Cloud. Esto hace que el trabajo colaborativo sea más facile y mantiene el State y la  información secreta fuera de tu disco local. El State remote se carga en memoria únicamente cuando es utilizado.

#### Destruye tu infraestructura
Asegurate de ejecutar el comando ```terraform destroy``` para limpiar los recursos creados en este tutorial. Terraform ejecutara esto en Terraform Cloud y mostrará la salida por tu terminal. Cuando se meustre recuerda confirmar con un ```yes```.  Puedes confirmar la operación visitando tu area de trabajo en la interfaz web de Terraflorm Cloud  y confirmar la ejecución.

#### Pasos Siguientes
Esto concluye los "getting started Tutorials" de Terraform. Ahora puedes utilizar Terraform para crear y manejar tu infraestructura. Para más tutoriales prácticos con el lenguaje de configuración de Terraform, provisionamiento de recursos o importar infraestructura existente, puedes mirar los siguientes tutoriales.

- [Configuration Languaje](https://developer.hashicorp.com/terraform/tutorials/configuration-language)
- [Modules](https://developer.hashicorp.com/terraform/tutorials/modules/module)
- [Provision](https://developer.hashicorp.com/terraform/tutorials/provision)
- [Import](https://developer.hashicorp.com/terraform/tutorials/state/state-import)

Para leer más acerca de las opciones de configuración explora la [Documentación de Terraform](https://developer.hashicorp.com/terraform/docs).

##### Aprende más con Terraform Cloud
Aunque Terraform Cloud puede almacenar el estado para admitir ejecuciones de Terraform en máquinas locales, funciona aún mejor como un entorno de ejecución remoto. Soporta dos flujos de trabajo principales para realizar ejecuciones de Terraform:

- Un flujo de trabajo impulsado por VCS, en el que automáticamente pone en cola planes siempre que se comprometen cambios en el repositorio VCS de tu configuración.
- Un flujo de trabajo impulsado por API, en el que una "pipeline" de CI u otra herramienta automatizada puede subir configuraciones directamente.

Para una experiencia práctica al flujo de trabajo impulsado por VCS de Terraform Cloud, sigue los [tutoriales de inicio rapido](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started). Terraform Cloud también ofrece [soluciones comerciales](https://www.hashicorp.com/products/terraform/pricing) que incluyen gestión de permisos de equipo, aplicación de políticas, agentes y más. 

</details>

------------------------------------------------------------------------------------------------------------------------------------------------------
</details>

### [Proposito del State de Terraform](https://developer.hashicorp.com/terraform/language/state/purpose)
<details>
El fichero State es un requisito necesario para que Terraform funcione. A menudo se pregunta si es posible que Terraform trabaje sin State, o que Terraform no use State y simplemente inspeccione los recursos del mundo real en cada ejecución. Esta página ayudará a explicar por qué se requiere el estado de Terraform.

En los escenarios donde Terraform podría prescindir del State, hacerlo requeriría trasladar grandes cantidades de complejidad de un lugar (el State) a otro lugar (el concepto de reemplazo).

#### Mapear la realidad
Terraform requiere algún tipo de base de datos para mapear la configuración de Terraform al mundo real. Por ejemplo, cuando tienes un recurso ```resource "aws_instance" "foo"``` en tu configuración, Terraform utiliza este mapeo para saber que el recurso ```resource "aws_instance" "foo"``` representa un objeto real con el ID de instancia ```i-abcd1234``` en un sistema remoto.

Para algunos proveedores como AWS, Terraform teóricamente podría usar algo como las etiquetas de AWS. Los prototipos iniciales de Terraform en realidad no tenían archivos State y usaban este método. Pero rápidamente se encontarron problemas. El primer problema importante fue simple: no todos los recursos admiten etiquetas y no todos los proveedores de nube admiten etiquetas.

Por lo tanto, para el mapeo de la configuración a los recursos en el mundo real, Terraform utiliza su propia estructura de estado.

Terraform espera que cada objeto remoto esté vinculado solo a una instancia de recurso en la configuración. Si un objeto remoto está vinculado a múltiples instancias de recurso, el mapeo de la configuración al objeto remoto en el estado se vuelve ambiguo y Terraform puede comportarse de manera inesperada. Terraform puede garantizar un mapeo uno a uno cuando crea objetos y registra sus identidades en el estado. Al importar objetos creados fuera de Terraform, debes asegurarte de que cada objeto distinto se importe solo a una instancia de recurso.

#### Metadatos 
Terraform también debe rastrear metadatos como las dependencias de recursos.

Terraform usa habitualmente la configuración para determinar el orden de dependencia. Sin embargo, cuando eliminas un recurso de una configuración de Terraform, Terraform debe saber cómo eliminar ese recurso del sistema remoto. Terraform puede ver que existe un mapeo en el archivo State para un recurso que no está en tu configuración y planificar su destrucción. Sin embargo, dado que la configuración ya no existe, el orden no se puede determinar únicamente a partir de la configuración.

Para asegurar una operación correcta, Terraform retiene una copia del conjunto más reciente de dependencias dentro del State. Ahora, Terraform todavía puede determinar el orden correcto para la destrucción a partir del estado cuando eliminas uno o más elementos de la configuración.

Una manera de evitar esto sería que Terraform conociera un orden requerido entre tipos de recursos. Por ejemplo, Terraform podría saber que los servidores deben eliminarse antes que las subredes de las que forman parte. Sin embargo, la complejidad de este enfoque aumenta rápidamente: además de que Terraform debe comprender la semántica de ordenamiento de cada recurso para cada proveedor, también debe entender el ordenamiento entre proveedores.

Terraform también almacena otros metadatos por razones similares, como un puntero a la configuración del proveedor que se utilizó más recientemente con el recurso en situaciones donde hay presentes múltiples proveedores con alias.

#### Rendimiento
Además del mapeo básico, Terraform almacena un caché de los valores de los atributos para todos los recursos en State. Esta es la característica más opcional del State de Terraform y se realiza únicamente con el objetivo de mejorar el rendimiento.

Al ejecutar ```terraform plan```, Terraform debe conocer el estado actual de los recursos para determinar los cambios que necesita hacer para alcanzar la configuración deseada.

Para infraestructuras pequeñas, Terraform puede consultar a los proveedores y sincronizar los últimos atributos de todos tus recursos. Este es el comportamiento predeterminado de Terraform: para cada plan y aplicación, Terraform sincronizará todos los recursos en tu State.

Para infraestructuras más grandes, consultar cada recurso es muy lento. Muchos proveedores cloud no proporcionan APIs para consultar múltiples recursos a la vez, y el tiempo de ida y vuelta para cada recurso es de cientos de milisegundos. Además de esto, los proveedores cloud casi siempre tienen limitación de tasa de API, por lo que Terraform solo puede solicitar un cierto número de recursos en un período de tiempo. Los usuarios más grandes de Terraform hacen un uso intensivo de la opción ```-refresh=false``` así como de ```-target``` para sortear esto. En estos escenarios, el estado en caché se trata como el registro de la verdad.En la configuración predeterminada, Terraform almacena el estado en un archivo en el directorio de trabajo actual donde se ejecutó Terraform. Esto está bien para comenzar, pero cuando se usa Terraform en un equipo, es importante que todos trabajen con el mismo estado para que las operaciones se apliquen a los mismos objetos remotos.

#### Sincronización
En la configuración predeterminada, Terraform almacena el archivo State en el directorio de trabajo actual donde se ejecutó Terraform. Está bien para comenzar, pero cuando se usa Terraform en equipo, es importante que todos trabajen con el mismo State para que las operaciones se apliquen a los mismos objetos remotos.

El [State remoto](https://developer.hashicorp.com/terraform/language/state/remote) es la solución recomendada a este problema. Con un backend de estado completamente equipado, Terraform puede utilizar el bloqueo remoto como medida para evitar que dos o más usuarios diferentes ejecuten Terraform al mismo tiempo, y así asegurar que cada ejecución de Terraform comience con el State más recientemente.

</details>

### [Ajustes de Terraform](https://developer.hashicorp.com/terraform/language/settings)
<details>

El bloque de ajustes especial ```terraform``` se usa para configurar algunos comporateminetos del mismo Terraform, como por ejemplo requerir una versión mínima de terraform para aplicar tu configuración.

#### Sintaxis del bloque Terraform
Los ajustes de Terraform se agrupan dentro del bloque ```terraform```:

```terraform
terraform {
  # ...
}
```

Cada bloque ```terraform``` puede contener un número de ajustes relacionadas al comportamiento de Terraform. Dentro de este bloque únicamente se pueden usar valores constantes; los argumentos no pueden referirse a objetos nombrados como recursos, variables de entrada, etc. y no pueden usar ninguna de las funciones incorporadas del lenguaje de Terraform.

Las distintas opciones soportadas dentro de un bloque ```terraform``` son descritas a continuación.

#### Configurando Terraform Cloud
El bloque anidado ```cloud``` configura Terraform Cloud para habilitar su "[CLI-driven run workflow](https://developer.hashicorp.com/terraform/cloud-docs/run/cli)"
- Dirigete a [Terraform Cloud Configuration](https://developer.hashicorp.com/terraform/language/settings/terraform-cloud) para un resumen de la sintaxis del bloque ```cloud```.
#### Traducción de la página Terraform Cloud configuration:
<details>
Solo necesitas configurar estos ajustes cuando quieras utilizar Terraform CLI para interactuar con Terraform Cloud. Terraform Cloud ignora estos cuando interactura con Terraform a través de un control de versiones o la API.

##### Ejemplo de uso
Para configurar el Terraform Cloud CLI integration, añade un bloque anidado ```cloud``` dentro del bloque ```terraform```. No puedes utilizar el CLI y un [State Backend](https://developer.hashicorp.com/terraform/language/settings/backends/configuration) en la misma configuración. 

```terraform
terraform {
  cloud {
    organization = "example_corp"
    ## Required for Terraform Enterprise; Defaults to app.terraform.io for Terraform Cloud
    hostname = "app.terraform.io"

    workspaces {
      tags = ["app"]
    }
  }
}
```

</details>
- Dirigete a [Using Terraform Cloud](https://developer.hashicorp.com/terraform/cli/cloud) en la documentación deTerraform CLI para más detalles acerca de cómo inicializar y configurar el Terraform Cloud CLI integration.  

[Settings](https://developer.hashicorp.com/terraform/cli/cloud/settings) [Initializing and migrating](https://developer.hashicorp.com/terraform/cli/cloud/settings) [Command Line Arguments](https://developer.hashicorp.com/terraform/cli/cloud/command-line-arguments)

#### Configura un Terraform Backend
El bloque anidado ```backend``` configura el State Backend que Terraform debe utilizar. La sintaxis y el comportamiento del bloque ```backend``` se describe en [Backend Configuration](https://developer.hashicorp.com/terraform/language/settings/backends/configuration)

#### Especifica un versión requerida de Terraform

Ejemplo prácticos: intenta realizar el [Manage Terraform Versions](https://developer.hashicorp.com/terraform/tutorials/configuration-language/versions) o [Manage Terraform Versions in Terraform Cloud](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-versions)

La configuración ```required_version``` acepta un argumento de [restricción de versión](https://developer.hashicorp.com/terraform/language/expressions/version-constraints), que especifica qué versiones de Terraform se pueden usar con tu configuración.

Si la versión de Terraform no coincide con las restricciones especificiadas, Terraform emiitrá un error sin ejecutar ninguna otra acción. 

Cuando usas "[child modules](https://developer.hashicorp.com/terraform/language/modules)", cada módulo puede especifica su propios requerimientos de version. Los requerimientos de todos los módulos en el árbol deben ser satisfechos.

Utiliza requerimientos de version en un entorno colaborativo para asegurarte que todo el mundo utiliza una versión específica de Terraform, o al menos una versión que tenga el comportamiento esperado para esa configuración.

La configuración ```required_version```  aplica solo a la versión de Terraform CLI. Los tipos de recursos de Terraform son implementados por plugins de proveedores, cuyos ciclos de lanzamiento son independientes de Terraform CLI y entre sí. Utiliza el bloque ```required_providers``` para gestionar las veriones específicas para cada provider.

#### Especificar Provider Requirements
El bloque ```required_providers``` especifica todos los providers requeridos por el módulo actual, mapeando cada provider a una dirección de origen y una restricción de versión.

```terraform
terraform {
  required_providers {
    aws = {
      version = ">= 2.7.0"
      source = "hashicorp/aws"
    }
  }
}
```
#### Características de Lenguaje Experimental
El equipo de Terraform algunas veces introduce algunas nuevas opciones, inicial mente mediante un "opt-in experiment", de manera que la comunidad pueda probar la nueva funcionalidad y dar feedbac sobre ella antes de que se convierta en una restricción de compatibilidad hacia atrás.

En versiones donde hay funcionalidades experimentales, puedes habilitarlas para cada módulo estableciendo el argumento ```experiments``` dentro de un bloque ```terraform```:
```terraform
```terraform {
  experiments = [example]
}
```

Los experimentos son sujetos de cambios arbitrarios en versionas futuras, dpenediendo del resultado del experimento, pueden cambiar drásticamente antes de la versión final o pueden no publicarse de una forma estable. Estos cambios pueden aparecer incluso en versiones menores y "patch releases". No recomendamos utilizar funcionalidades experimentales en módulos de Terraform que están dirigidos a Producción.

Para hacer esto explícito y evitar llamadas a módulos que dependan de una característica experimental, cualquier módulo con experimentos habilitados generará un aviso en cada ejecución de ```terraform plan``` o ```terraform apply```. Si quieres intentar utilizar características experimentales en módulos compartidos, te recomendamos habilitar el experimento solo en publicaciones alfa o beta del módulo.

La introudción y finalización de experimentos se informa en el [Changelog de Terraform](https://github.com/hashicorp/terraform/blob/main/CHANGELOG.md), de manera que puedas consultar las notas de la versión para descubrir qué palabras clave de experimento, si las hay, están disponibles en una versión particular de Terraform.

#### Enviar metadatos a Providers
El bloque ```terraform``` puede tener anidado el bloque ```provider_meta``` para cada provider que un módulo está usando, si el provider define en esquema para el mismo. esto permite al provider recibir información específica del módulo, está destinado principalmente para módulos distribuidos por el mismo vendedor que el proveedor asociado. Para más información, [Provider Metadata](https://developer.hashicorp.com/terraform/internals/provider-meta).

</details>

### [Administra las  versiones de Terraform](https://developer.hashicorp.com/terraform/tutorials/configuration-language/versions)
<details>

HashiCorp activamente desarrolla y mantiene Terraform. Para acceder a neuvas características de Terraform deberás actualizar la versión de Terraform que usas en tu configuración. Establece ```required_version``` para controlar la versión de terraform que tus configuraciones utilizan y realizar actualizaciones predecibles.

En este tuotiral, actualizarás una configuración existente a una versión actualizada de Terraform y aprenderas a administrar diferentes veriones de terraform con un equipo. 

#### prerequisitos
Para completar este tutorial necesitarás:
- Tener instalado localmente Terraform CLI versión 0.15 o superior.
- Una cuenta de AWS.
- Tus credencias AWS configuradas localmente
- [Git CLI](https://git-scm.com/downloads)

#### Clona el respositorio de ejemplo:
```
git clone https://github.com/hashicorp/learn-terraform-versions.git
```
Cambia de directorio de trabajo:
```sh
cd learn-terraform-versions
```

Este repositorio contiene una configuración completa de Terraform que despliega un ejemplo de una aplicación web en AWS. En cualquier caso, esta configuración utiliza una versión antigua de Terraform. Tendrás que actualizarla para utilizar una versión más reciente de Terraform. 

#### Revisa la configuración de ejemplo
Abre ```main.tf``` y encuentra el bloque ```terraform```.

```terraform
terraform {
  required_providers {
    aws = {
      version = "~> 2.13.0"
    }
    random = {
      version = ">= 2.1.2"
    }
  }

  required_version = "~> 0.12.29"
}
```
Esta configuración establece ```required_version``` a ```~>0.12.29```.  El simbolo ```~>``` permite que la versión de "patch" sea superior a 29 pero requiere que la versión mayor y menor (```0.12```) coincida con la configuración especificada. Terraform mandará un error si intentas utilizar esta configuración con una más reciente que ```0.12.x```, porque tiene la configuración ```required_version```.

Utiliza el subcomando ```version``` para comprobar qué versión de Terraform tienes instalado y la versión de cualquier provider que esté utilizando tu configuración. 
```sh
terraform version
```

Terraform  te permitirá saber si hay alguna versión actualizada disponible.

Intenta inicializar el proyecto con ```terraform init```. Terraform mostrará un error indicnado que tu versión local es muy nueva para esta restricción de versión ```required_version```.

HashiCorp Utiliza el formato ```major.mino.patch``` para las versiones de TErraform. HashiCorp actualiza Terraform Frecuentemente, por lo que es común utilizar configuraciones escritas para una versión más antigua de Terraform. Las nuevas versiones ```minor``` y ```patch``` de Terraform son compatibles hacia atrás escritars por versiones previas. Por esto, puedes actualizar a una nueva versión menor de Terraform y mantener tus configuraciones existentes. En cualquier caso, actualizar tu versión de Terraform puede tener otras consecuencias, como actualizar la versión de tus ```providers```. Algunas actualizaciones e versiones pueden actualizar la versión de tu archivo State o requerir editar el archivo de configuración para implementar nuevas características. Utiliza el ajuste ```required_version```  para controlar con qué versión de Terraform trabajaras con tus configuraciones y asegurar que las actualizaciones de infraestuctura sean seguras y predecibles. 

en ```main.tf```, cambia ```0.12.29``` con tu versión actual de Terraform, que ha sido mostrada por pantalla cuando ejecutaste el comando ```terraform version```. Asegurate de cuardar este archivo. Asegurate de guardar el archivo.

```diff
   required_providers {
## ...
   }

-  required_version = "~> 0.12.29"
+  required_version = "~> <TERRAFORM VERSION>"
 }
```

Ahora inicializa tu configuración mediante el comando ```terraform init```. 
Una vez inicializado, aplica esta configuración con el comando ```terraform apply``` para crear la infraestructura de ejemplo. Recuerda responder a la pregunta acerca de confirmación con un ```yes```.

#### Inspecciona el archivo Satate de Terraform
Cuando ejecutas comandos Terraform, Terraform guarda su versión actual en tu archivo de proyecto State, junto con la versión de formato del State. Como TErraform guarda este archivo State como texto, puedes inspeccionarlo para determinar qué version de Terraform lo ha generado. 

```sh
grep -e '"version"' -e '"terraform_version"' terraform.tfstate
```

Si tu sistema no tiene el comando ```grep```, puedes abrir el archivo ```terraform.tfstate``` en tu editor de texto para mirar los valores de ```version``` y de ```terraform_version``` cerca del principio del archivo.

Terraform únicamente actualizará ```version``` en el archivo State cuando una nueva versión de Terraform requiera un cambio en el formato del archivo State. Terraform actualizará la ```terraform_version``` cada vez que apiques un cambio a tu configuración utilizando una versión más reciente de Terraform. 

En general, Terraform continuará el trabajo con un archivo State a lo largo de las actualizaciones de las versiones menores. Cuando se publican major o minor, Terraform actualizará la versión del archivo State si es necesario, y dará un error si intentas correr una versión más antigua de Terraform utilizando una versión no soportada del archivo State. 

Si ibas a intentar aplicar esta configuración utilizando una versión más antigua de Terraform que no soporta la versión actual del archivo State, Terraform devuelve un ```state lock error``` y muestra qué versión sería necesaria. 

Una vez que utilizas una versión más actualizada del archivo State en un proyecto, no hay forma de revertir a una versión más antigua del archivo State. 

Terraform manjea de forma independiente las versiones de los provider de la versión de Terraform en sí mismo. Algunas veces una versión más antigua de un provider no funcionará con una nueva versión de Terraform. Cuando actualizas Terraform, revisa las versiones de tus provider y considera actualizarlas también. Realiza nuestro tutorial acerca de [bloquear y actualizar las versiones de los provider](https://developer.hashicorp.com/terraform/tutorials/configuration-language/provider-versioning) para aprender a manejar las versiones de los provider. 

#### Restricciones de las versiones de Terraform.

La siguiente tabla resume algunos de los caminos posibles en los quep uedes indicar al versión de Terraform en el ajuste ```required_version```.  Asumiendo que tienes Terraform v0.15.0 como tu versión actual objetivo. Dirigete a [Terraform Docuementation](https://developer.hashicorp.com/terraform/language/expressions/version-constraints) para una explicación detallada de las restricciones de la versión. 

| Required Version | Significado | Consideraciones |
|--------------|--------------|--------------|
|  ```0.15.0``` | Solo Terraform v0.15.0  | Para actualizar Terraform, primero edita la configuración ```required_version```  |
| ```>= 0.15```  | Cualquier version de Terraform v0.15.0 o superior | Incluye Terraform v1.0.0 y superiores  |
| ``` ~> 0.15.0 ```  | Cualquier versión de Terraform v0.15.x, pero no v1.0 o superior | Las versiones menores están pensadas para no ser disruptivas  |
| ``` <= 0.15, <2.0.0```  | Terraform v0.15.0 o superior, pero inferior que v2.0.0 | Evita actualizaciones major superiores |

En general, animamos a usar la última versión disponible de Terraform para tener todas las ventajas de las nuevas funciones y la ressolución de bugs. En cualqueir caso, no es necesario actualizar tu proyecto de Terraform a la última versión cada vez que utilizas Terraform, a menos que necesites una característica especifica o que se haya solucionado un bug. 

Como una buena práctica, considera utilizar ```~>``` para mantener tu version major y minor de Terraform. Esto permitirá que tu y tu equipo utiliceis los parches de actualización sin tener que actualizar tu configuración de Terraform. Entonces puedes planear cuando quieres actualizar tu configuración y usar una nueva versión de Terraform, y revisara detenidamente los cambios para asegurar que tu proyecto siga funcionando como se espera.

Por ejmplo, si escribes tu configuración Terraform utilizando 1.0.0, y añades ```required_version = "~> 1.0.0"``` a tu bloque ```terraform { }```. Esto te permitira que tu y tu equipo utiliceis Terraform ```1.0.x```, pero necesitarás actualizar tu configuración de Terraform para ```1.1.0``` o superior.

#### Limpia tu infraestructura
Destruye la infraestructura creada en este tutorial mediante el comando ```terraform destroy```, recuerda responder a al petición de confirmación con un ```yes```.

#### Siguientes pasos
Ahora que eres capaz de manejar versiones de Terraform utilizando Terraform CLI. Cuando utilices Terraform en producción, te recomendamos que tu y tu equipo tengas planes y procedimientos para determinar cómo administrar las versiones de Terraform y gestionar actualizaciones. 

Terraform Cloud y Terrafor Enterprise incluyen características que ayudan a que el equipo trabaje conjuntamente en los proyectos Terraform, como proveer un entorno de ejecucion administrado por Terraform y soporte para equipos y permisos. Cuando usas Terraform Cloud o Terraform Enterprise, puedes configurar cada espacio de trabajo para usar cualquiera de las versiones de Terraform qu especifiques. 

</details>


### [Resumen acerca de Providers](https://developer.hashicorp.com/terraform/language/providers)
<details>
Terraform confía en plugins llamados providers para interactuar con los provedores Cloud, SaaS y otras APIs.

Las configuraciones Terraform deben declarar qué prividers requieren para que Terraform pueda instalarlas y usarlas. Adicionalmente, algunos proveedores require configuración (por ejemplo endpoints URLs o regiones Cloud) antes de ser utilizadas.

#### ¿Qué hacen los Providers?
Cada provider añade un conjunto de [tipos de recursos](https://developer.hashicorp.com/terraform/language/resources) y/o [fuentes de datos](https://developer.hashicorp.com/terraform/language/data-sources) que Terraform puede gestionar. 

Cada tipo de recurso es implementado por un provider, sin providers Terraform no puede manjear ningún tipo de inraestructura. 

La mayorái de provideres configuran una plataforma específica de infraestructura (ya sea Cloud o local). Los providers pueden ofrecer utilidades locales para tareas como generar números aleatorios para nombres únicos de recursos.

#### ¿De dónde proceden los Providers?
Los providers son distribuidos de forma separada a Terraform, cada provider tiene sus propios ciclos de publicación y numeros de versión. El [registro de Terraform](https://registry.terraform.io/browse/providers) es principal directorio público disponible de Terraform providers para la mayoría de plataformas de infraestuctura. 

#### Documentación de los Provider
Cada provider tiene su propia documentación, describe los tipos de recursos y sus argumentos. 

El registro Terraform,incluye documentación para un gran rango de providers desarrollados por HashiCorp, "thid-party vendors", y la comunidad de Terraform. Utiliza el link "Documentation" en el encabezado de un proveedor para navegar en su documentación. 

La documentación de los Provider está versionada, puedes usar el menu versión en el encabezado para cambiar qué versión quieres ver. 

Para detalles acerca de escribir, generar y ver la documentación del provedor, puedes ver [provider publishing documentation](https://developer.hashicorp.com/terraform/registry/providers/docs).


#### Cómo utilizar Providers
Los providers son publicados de forma separada a Terraform  y tienen su propios números de versión. En producción recomendamos restringir las versiones aceptables de providers en el bloque de configuración de providers requirements., apra estar seguros de que terraform init no instalará versiones actualizadas del proveedor que sean incompatibles con la configuración.

Para usar recursos de un provider dado, necesitas incluir alguan información acerca de él en tu configuración. Mira las siguientes páginas para entrar en detalle:
- [Provider Requirements](https://developer.hashicorp.com/terraform/language/providers/requirements) documenta cómo se declaran los providers de manera que terraform pueda instalarlos.
- [Provider Configuration](https://developer.hashicorp.com/terraform/language/providers/configuration) doucmenta cómo se configuran las opcioens para los providers.
- [Dependency Lock File](https://developer.hashicorp.com/terraform/language/files/dependency-lock)documenta un archivo adicional HCL que puede incluir una configuración que indica a Terraform que utilice siempre un conjunto específico de Provider Versions.

#### Instalación de Provider
- Terraform Cloud y Terraform Enterprise instalan los providers como parte de cada ejecución.
- Terraform CLI encuentra e instala los providers cuando inicializa el directorio de Trabajo. Puede Automáticamente descargar providers de un registro Terraform o cargarlos desde cache o un "local mirror". Si estás usando un directorio de trabajo persistente, debes reinicializar cuando cambies la configuración de los providers.

Para ahorrar tiempo y ancho de banda, Terraform CLI soporta un plugin opcional de cache. Puedes habilitar la cache utilizando el ```plugin_cache_dir``` estableciendo en el [Archivo de configuruación de CLI](https://developer.hashicorp.com/terraform/cli/config/config-file).

Para asegurar que Terraform siempre instala la misma versión de provider para una determinada confiugración, puedes usar TErraform CLI para crear un archivo "lock" de dependencias y llevarlo al sistema de control de versiones con tu configuraicón. Si un archivo "lock" está presente, Terraform Cloud, CLI, y Entrerpiese le obedecerán cuando instalen los providers.

#### Cómo encontrar Providers
Algunos providers en el Registro de Providers son desarrollados y publicados por HashiCorp, algon son publicados por mantenedores de las plataformas, y otros son publicados por usuarios y voluntarios. Los providers utilizan las siguientes placas para indicar quién las desarrolla y mantiene.

| Tier|	Description	| Namespace |
|--------------|--------------|--------------|
| Official | Official providers are owned and maintained by HashiCorp |	hashicorp |
|Partner |Partner providers are written, maintained, validated and published by third-party companies against their own APIs. To earn a partner provider badge the partner must participate in the |HashiCorp Technology Partner Program.| Third-party organization, e.g. mongodb/mongodbatlas |
|Community |Community providers are published to the Terraform Registry by individual maintainers, groups of maintainers, or other members of the Terraform community.|	Maintainer’s individual or organization account, e.g. DeviaVir/gsuite |
|Archived | Archived Providers are Official or Partner Providers that are no longer maintained by HashiCorp or the community. This may occur if an API is deprecated or interest was low. | hashicorp or third-party |

#### ¿Cómo desarrollar Providers?
Providers están escritos en Go, utilizando el plugin Terraform SDK. Para más inforamción: [Plugin Development documentation](https://developer.hashicorp.com/terraform/plugin), [Call APIs with Terraform Privders](https://developer.hashicorp.com/terraform/tutorials/providers-plugin-framework?utm_source=WEBSITE&utm_medium=WEB_IO&utm_offer=ARTICLE_PAGE&utm_content=DOCS).

</details>

### [Cómo trabaja Terraform con Plugins](https://developer.hashicorp.com/terraform/plugin/how-terraform-works)
<details>
Terraform es una herramienta para consturir, cambiar y versionar infraestructura de forma segura y eficiente. TErraform se construye en una arquitectura basada en plugins, permitiendo a desarrolladores ampliar TErraform escribiendo nuevos plugins o compilando versiones modificadas de los existentes. 

Terraform está dividido lógicamente en dos partes principales: Nucleo de Terraform (Terraform core) y Terraform Plugins. El núcleo de Terraform utiliza llamadas a procedimientos remotos (RPC) para comunicarse con los Plugins de Terraform, y ofrece muchos caminos para descubrir y cargar los plugins a utilizar. Los plugins de terraform exponen una implementación para un servicio específico, como AWS, o un provisioner, como Bash. 

#### Núcleo de Terraform
El núcleo de Terraform es un [binario compilado estáticamente](https://en.wikipedia.org/wiki/Static_build#Static_building) escrito en el lenguaje de programación [Go](https://golang.org/). El binario compilado es la herramienta de línea de comandos (CLI) ```terraform```, el punto de accesio para cualquiera que utilice Terraform. El código fuente está disponible en [github](https://github.com/hashicorp/terraform).

#### Responsabilidades primaria del núcleo de TErraform
- Infraestructura como código: leer e interpolar archivos de configuración y módulos.
- Manejo del estado de los recursos
- Construcción del [Resource Graph](https://developer.hashicorp.com/terraform/internals/graph)
- Ejecución del plan.
- Comunicación con los plugins por RPC.

#### Terraform Plugins
Los plugins de Terraform está nescritos en Go y son binarios ejecutables llamado por el Nucleo de Terraform a través de RPC. Cada Plugin expone una implementación para un servicio específico, como AWS, o prisioner como Bash. Todos los Providers y Provisioners utilizados en configuraciones Terraform son Plguins. Son ejecutados como un proceso separado y se comunican  con el binario de Terraform a traves de una interfaz RPC. Terraform tiene diversos Provisioners incorporados, mientras que los providers son descubiertos dinámicamente según necesidad. El núcleo de Terraformprovee marco de trabajo de alto nivel que abtrae la forma en la que los plugins son encontrados y la comunicación RPC, de manera que los desarrolladores no tienen que gestionar esto. 

Lo terraform Plugins son los responsables de la implementación especifica de dominio de su tipo.

#### Responsabilidades primarias de los plugins de Provider:
- Inicialización de cualquier librería utilizada para realizar las llamadas API.
- Autenticación con la infraestructura del Provider.
- Definicion de los recursos administrados y fuente de datos que señalan a los servicios especificos.
- Definir funciones que habiliten o simplifiquen la lógica computacional para las configuraciones de los profesionales.

##### Responsabilidades de Provisioner Plugins:
- Ejecutar comandos o scripts en el recurso designado después de la creación o en la destrucción.

#### Discovery
!! Tema Avanzado: Esta sección describe cómo son descubiertos los Terraform Plugin a un nivel de detalle que un desarrollador de plugin debe saber.

Cuando ```terraform init``` se ejecuta, Terraform lee los arhicovs de configuración en el directorio de trabajo para determinar que plugins son necesarios, busca los plugins instalados en distintas localizaciones, otras veces descarga plugins adicionales, decide qué versión debe utilizar y escribe a "lock file" para sasegurar que Terraform utilizará siempre la misma versión de plugin en este directorio hasta que se ejecute nuevamente ```terraform init```.

#### Localizaciones de plugins. 
La [doucmentación de Terraform CLI](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-installation) tiene actualizada y detallada información acerca de dónde Terraform busca los binarios de los plugins como parte de ```terraform init```. Consulta esa documentación para saber [información de dónde situar los binarios durante desarrollo](https://developer.hashicorp.com/terraform/cli/config/config-file#development-overrides-for-provider-developers).

#### Selección de Plugins
Despues de localizar cualquier plugin instalado, ```terraform init``` los compara a las restricciones de la configuración y elige una versión para cada plugin de la siguiente manera:
- Si hay versiones que cumplen los requisitos, Terraform utiliza la versión más reciente que cumpla las restricciones (incluso si el registro de terraform tiene una versión más nueva que sea aceptable).
- Si no hay versionas aceptables instaladas y el plugin es uno de los Providers distribuidos por HashiCorp, Terraform descarga la versión más reciente aceptable del Registro de terraform y la guradda en el subdirectorio ```.terraform/providers/```.
- Si no hay versiones aceptables instaladas y el plugin no es distribuido en el registro de Terraform, la inicialización falla y el usuario debe instalar manualmente la versión apropiada.

#### Actualizar Plugins
Cuando ejecutamos ```terraform init``` on la opción ```-upgrade```, vuelve a comprobar el Registro de Terraform por una versión más reciente aceptable y la descarga si está disponible.

Este comportamiento solo aplica a providers cuyas versiones aceptables se encuentra únicamente en los subdirectiorios correctos bajo ```.terraform/providers/``` (el directorio de descarga automática); si cualquier versión aceptable de un provider está instalada en cualqueir otro sitio, ```terraform init -upgrade``` no descargará una nueva versión del mismo. 

</details>

### [Configuracion de Provider](https://developer.hashicorp.com/terraform/language/providers/configuration)
<details>
Los provider permiten a Terraform interactuar con proveedores Cloud, Saas, y otras APIs.

Algunos provideres requieren que configures mediante endpoint URLs, regiones Cloud y otros ajustes antes de que Terraform los pueda utilizar. Esta página documenta cómo configurar los ajustes de providers.

Adicionalmente, todas las configuraciones Terraform deben declarar qué provider necesitan para que Terraform pueda instalarlas y utilizarlas. En [Provider Requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)  documenta cómo declarar providers de manera que Terraform los pueda instalar. 


#### Configuración del provider
Las configuraciones del Provider pertenecen al directorio raíz de la configuración Terraform. (Los modulos hijos, reciben su configuración de provider del módulo raíz; para más información mira [The Module providers Meta-Argument](https://developer.hashicorp.com/terraform/language/meta-arguments/module-providers) y [Module Development: Providers Within Modules](https://developer.hashicorp.com/terraform/language/modules/develop/providers))

Una configuración de provider se crea utilizando un bloque ```provider```:

```terraform
provider "google" {
  project = "acme-app"
  region  = "us-central1"
}
```

El nombre dado al encabezado del bloque (```"google"``` en este ejemplo) es el [nombre local](https://developer.hashicorp.com/terraform/language/providers/requirements#local-names) de la configuración del provider a configurar. Este provider debe estar ay incluido en un bloque ```required_providers```.

El cuerpo del bloque (lo que está entre ```{``` y ```}```) contiene argumentos de configuración para el provider. La mayoría de argumentos en esta sección está ndefinidos por el mismo provider; en este ejemplo, tanto ```project``` como ```region``` son especificos del provider ```google```.

Puedes usar [expresiones](https://developer.hashicorp.com/terraform/language/expressions) en valores de estos argumentos de configuración, pero solo pueden hacer referencia a valores conocidos antes de que la configuración sea aplicada. Esto significa que puedes hacer referencia de forma segura a variables de entrada, pero no a atributos exportados por recursos (con la excepción de argumentos de recursos que están especificados directamente en la configuración).

La documentación de los provider debe listar qué argumentos de configuración espera. Para providers distribuidos en el Registro de Terraform, la documentación versionada está disponible en la página de cada provider, desde el link Documentación en el encabezado del privider.

Algunos providers puede utilizar variables de entorno Shell (u otras fuentes alternativas, como perfiles de instancia VM) como valores para algunos de sus argumentos; cuando están disponibles, recomendamos utilizar esto como una forma de mantener credenciales fuera de tu código Terraform. 

Además hay otros "meta-arguments" que son definidos por terraform mismo y disponibles para todos los bloques de ```provider```:
- [alias](https://developer.hashicorp.com/terraform/language/providers/configuration#alias-multiple-provider-configurations) para utilizar el mismo provider con diferentes configuraciones para diferentes recursos.

No como muchos otors objetos en el lenguaje TErraform, un bloque ```provider``` puede ser omitido si sus contenidos fueran vacíos. Terraform asume que una configuración vacía por defecto para cualquier povider que no está explicitamente configurado.

##### alias: Configuraciones múltiples de provider
Opcionalmente puedes definir múltiples configuraciones para el mismo provider y selecionar cual utilizar para un recurso o para un módulo. La razón principal de esto es dar soporte a múltiples regiones para una plataforma Cloud; otros ejemplos incluyen hacer objetivo multiples Docker hosts, etc.

Para clerar múltiples configuraciones para un proveedor dado, incluye múltoples bloques de ```provider``` para el mismo nombre de provider. Para cada configuración adicional utiliza el meta-argumento ```alias``` para proveer un nombre de segmento extra. Por ejemplo:

```terraform
# The default provider configuration; resources that begin with `aws_` will use
# it as the default, and it can be referenced as `aws`.
provider "aws" {
  region = "us-east-1"
}

# Additional provider configuration for west coast region; resources can
# reference this as `aws.west`.
provider "aws" {
  alias  = "west"
  region = "us-west-2"
}
```

Para declarar un alias de configuración dentro de un módulo con el fin de recibir una configuración de provider alternativa del modulo padre, añade el argumento ```configuration_aliases``` para la entrada ```required_providers``` de ese provider. El siguiente ejemplo declara tanto los nombre de configuración del porivder  ```mycloud``` como ```mycloud.alrternate``` dentro del módulo contenedor:

```terraform
terraform {
  required_providers {
    mycloud = {
      source  = "mycorp/mycloud"
      version = "~> 1.0"
      configuration_aliases = [ mycloud.alternate ]
    }
  }
}
```

[[[[ Este enfoque permite que el módulo use múltiples configuraciones para el mismo proveedor, posibilitando escenarios avanzados de gestión de infraestructura donde diferentes recursos necesitan interactuar con el proveedor de manera diferente.]]]]

#### Configuraciones por defecto de Provider
Un bloque ```provider``` sin un argumento ```alias``` es la configuración por defecto para ese provider. Los recursos que no establezcan el meta argumento ```provider``` utilizarán el provider por defecto que coincida con la primera palabra del tipo de recurso. (por ejmplo, un recurso  ```aws_instance```  utilizará la configuración por defecto  del provider ```aws```  a menos que otra cosa sea establecida).

Si cada configuración explicita de un provider tiene un alias, Terraform utilizara la configuración vacía implícita como la configuración predeterminada de ese provider. (Si el provider tiene algún argumento de configuración requerido, Terraform generará un error cuando los recursos predeterminen a al configuración vacía.)

#### Referirse a configuraciones alternativas de providers
Cuando Terraform necesita el nombre de una configuración de un provider, espera una referencia de la forma ```<PROVIEDER NAME>.<ALIAS>```. En el ejemplo de arriba, ```aws.west``` haría referencia al provider que tiene la regioón ```us-west-2```. 

Estas referencias son expresioens especiales. Como referencias a otras entidades nombradas (por ejemplo, ```var.image_id```), no son strings y no necesitan estar entre comillas. Pero son solo válidas en meta-argumentos especificos de ```resource```, ```data```, y bloques de ```module```, y no puede ser usadas en expresiones arbitrarias.

#### Seleccionar configuraciones alternativas de Provider
Por defecto, los recursos utilizan una configuración por defecto de un provider (uno sin un argumento ```alias```) inferido de la primera palabra del tipo de recurso.

Para utilizar una configuración de provider alternativa o fuende dee datos, estable su meta-argumento ```provider``` a la referencia ```<PROVIEDER NAME>.<ALIAS>```:

```terraform
resource "aws_instance" "foo" {
  provider = aws.west

  # ...
}
```

Para seleccionar un provider alternativo en un módulo hijo, utiliza su meta argumento ```providers``` para especificar qué configuración provider  debería ser mapeada a qué nombres de providers locales dentro del módulo:
```terraform
module "aws_vpc" {
  source = "./aws_vpc"
  providers = {
    aws = aws.west
  }
}
```

Los módulos tienen unos requisitos especiales al pasar providers, consulta [The module providers Meta-Argument](https://developer.hashicorp.com/terraform/language/meta-arguments/module-providers) para más detalles. En la mayoría de casos, sólomódulos raíz deberían definir configuraciones de provider, con todos los módulos hijo obteniendo sus configuraciones de provider de sus padres.
</details>

### [Bloquear y actualizar versiones de provider](https://developer.hashicorp.com/terraform/tutorials/configuration-language/provider-versioning)
<details>
Los prividers de Terraform administra recursos comunicandose entre Terraform y la API objetivo. Cuando la API objetivo cambia o añade funcionalidad, los mantenedores del provider puede actualizar la versión del provider.

Cuando multiples usuarios o herramientas de automatización ejecutan la misma configuración Terraform, deberían todos utilizar la misma versión de los providers requeridos. Hay dos formas para manejar las versiones de provider en tu configuración.

1. Especificar la versión de provider como restricción en tu configuracción en el bloque ```terraform```.
2. Utilizar el archivo [archivo de bloqueo de dependencias](https://developer.hashicorp.com/terraform/language/files/dependency-lock)

Si no limitas adecuadamente la versión del provider, Terraform descargará la última versión del provider que cumpla con la restricción de versión. Esto puede llevar a cambios inesperados en la infraestructura. Al especificar versiones de proveedor cuidadosamente delimitadas y utilizando el archivo de bloqueo de dependencias, puedes asegurarte de que Terraform esté utilizando la versión correcta del proveedor para que tu configuración se aplique de manera consistente. 

En este tutorial, crearás un S3 bucket de una configuración inicializada de Terraform. Entonces, actualizarás el archivo de bloqueo de dependencias para utilizar la última versión del AWS provider, y editar la configuración de terraform para un nuevo requisito de versión del provider.

#### Prerequisitos
Puedes completar este tutorial usando el mismo flujo de trabajo que Terraform Community Edition o Terraform Cloud. 

Selecciona la pestaña Terraform Cloud para completar este tutorial utilizando Terraform Cloud.

Este tutorial asume que estás familiarizado con el flujo de trabajo de Terraform y Terraform Cloud. Si eres nuevo en Terraform ,completa los tutoriales ["Get Started"](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) antes de realizar este. Si eres nuevo en Terraform Cloud, completa el tutorial [Terraform Cloud Get Started](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started).

Con el fin de completar este tutorial, necesitarás: 
- Terraform v1.1+ Instalado localmente.
- Una cuenta AWS.
- Una cuenta en Terraform Claud, con Terraform Cloud autenticado localmente.
- Una Variable en Terraform Cloud que configure tus credenciales AWS.

#### Clona el repositorio de ejemplo
Clona el repositorio
```sh
git clone https://github.com/hashicorp/learn-terraform-provider-versioning.git
```

Navega al directorio del repositorio en tu terminal
```sh
cd learn-terraform-provider-versioning
```

#### Revisa las configuraciones
Este directorio es un proyecto Terraform pre inicializado con tres archivos: ```main.tf```, ```terrafor.tf``` y ```.terraform.lock.hlc```. HashiCorp ha publicado una nueva versión del AWS provider desde que este espacio de trabajo ha sido inicializado por primera vez.

#### Explora ```main.tf```
Abre el archivo ```main.tf```. Teste archivo utiliza los providers AWS y "random" para desplegar un S3 bucket con un nombre aleatorio en la región ```us-west-2``` .

```terraform
provider "aws" {
  region = "us-west-2"
}

resource "random_pet" "petname" {
  length    = 5
  separator = "-"
}

resource "aws_s3_bucket" "sample" {
  bucket = random_pet.petname.id

  acl    = "public-read"
  region = "us-west-2"

  tags = {
    public_bucket = true
  }
}
```

### Explora ```terraform.tf```
Abre el archivo ```terraform.tf```. Aquí encontrarás el bloque que especifica la versión del provider requerido y la versión requerida de Terraform para esta configuración.

```terraform
terraform {
  required_providers {
    random = {
      source  = "hashicorp/random"
      version = "3.1.0"
    }

    aws = {
      source  = "hashicorp/aws"
      version = ">= 2.0.0"
    }
  }

  required_version = ">= 1.1"
}
```

El bloque ```terraform``` contiene  el bloque ```required_providers```, el cual especifica el nombre del local provider, la dirección de la fuente y la versión. 
Cuando inicializas esta configuración, Terraform descargará:
1. Versión 3.1.0 dle proveedor Random.
2. La última versión del AWS provider que sea superior a 2.0.0. El operador restrictivo ```>=``` especifica la versión mínima del provider que es compatible con la configuración.

El bloque Terraform también especifica que solo los binarios de Terraform más nuevos que la versión v1.1.x pueden ejecutarse utilizando el operador ```>=```.

#### Explora ```terraform.lock.hcl```
Cuando inicializas una configuración terraform por primera vez con Terraform 1.1 o superior, Terraform genera un archivo nuevo ```.terraform.lock.hcl``` en el directorio de trabajo activo. Deberías incluir el archivo de bloqueo de dependencias en tu repositorio de contorl de versiones para asegurarte de que Terraform utilice siempre las mismas versiones de provider entre tu equipo y entornos de ejecución remotos.

Abre el archivo ```terraform.lock.hcl```:

```terraform
# This file is maintained automatically by "terraform init".
# Manual edits may be lost in future updates.

provider "registry.terraform.io/hashicorp/aws" {
  version     = "2.50.0"
  constraints = ">= 2.0.0"
  hashes = [
    "h1:aKw4NLrMEAflsl1OXCCz6Ewo4ay9dpgSpkNHujRXXO8=",
    ## ...
    "zh:fdeaf059f86d0ab59cf68ece2e8cec522b506c47e2cfca7ba6125b1cd06b8680",
  ]
}

provider "registry.terraform.io/hashicorp/random" {
  version     = "3.1.0"
  constraints = "3.1.0"
  hashes = [
    "h1:9cCiLO/Cqr6IUvMDSApCkQItooiYNatZpEXmcu0nnng=",
    ## ...
    "zh:f7605bd1437752114baf601bdf6931debe6dc6bfe3006eb7e9bb9080931dca8a",
  ]
}
```

Los dos providers especificados en tu ```terraform.tf```. el AWS provider version es v2.50.0. El cual cumple  el requisito ```>=2.0.0```. El provider rando está establecido a v3.1.0 y cumple los requisitos de versión.

#### Inicializa y aplica la configuración
Si estás utilizando Apple M1 o M2 CPU no vas a poder inicializar o aplicar la configuración por que el provider de AWS es muy antiguo para esos procesadores. Lee esta sección y sigue las otras, la configuración final funcionará como se espera.

Abre tu ```terraform.tf``` y descomenta el bloque ```cloud```. Reemplaza el nombre de  ```organization``` con tu propio nombre de Terraform Cloud organization.

```terraform
terraform {
  cloud {
    organization = "organization-name"
    workspaces {
      name = "learn-terraform-provider-versioning"
    }
  }
  required_providers {
    random = {
      source  = "hashicorp/random"
      version = "3.0.0"
    }

    aws = {
      source  = "hashicorp/aws"
      version = ">= 2.0.0"
    }
  }

  required_version = ">= 1.1"
}
```

Inicializa tu configuración. Terraform automáticamente creará el espacio de trabajo ```learn-terraform-provider-verioning``` en tu Terraform Cloud Organization.

```sh
terraform init
```
Recuerda que este tutorial asume que ya tienes en tus variables globales establecidas tus credenciales de AWS.

En vez de instalar la última versión del AWS provider conforme a las restricciones de versión, Terraform instala la versión especificada en el Lock file. Mientras inicializas tu espacio de trabajo, Terraform lee el archivo de bloqueo de dependencias y descarga las versiones especificadas de AWS y Random prividers.

Si terraform no encuentra un archivo de bloqueo de dependencias, descargará la última versión de los provider que cumplan las restricciones definidas en el bloque ```required_providers```. La siguiente tabla muestra qué provider Terraform descargaría en este escenario basado en las restricciones y la presencia del archivo de bloqueo de dependencias.


| Provider | Restricción de Versión | ```terraform init``` (no lock file) | ```terraform init``` (lock file) |
|--------------|--------------|--------------|--------------|
| aws | ```0.15.0``` | Versión más actual (por ejemplo 4.45.0)  |  Versión en el archivo de bloqueo de dependencias (2.50.0 |
| random | 3.1.0 | 3.1.0 | versión en el archivo de bloqueo de dependencias (3.1.0) |

El archivo de bloque o de dependencias indica a Terraform instalar siempre la misma versión de provider, asegurando ejecuciones consistentes en tu equipo y sesiones remotas.

Aplica tu configuración mediante el comando ```terraform apply```. Recuerda responder a la solicitud de autorización con un ```yes``` para crear el ejemplo de infraestructura.


#### Actualiza la versión del AWS provider
La opción ```-upgrade``` actualizará todos los providers a la úlitma versión consistente con los requisitos de versión especificados en tu configuración. También puedes utilizar la opción ```-upgrade``` para bajar la versión del provider si los requisitos son modificados para especificar una versión anterior del provider.

Importante: Nunca deberías modificar tdirectamente el Lock File (archivo de bloqueo de dependencias).

Para actualizar el AWS provider ejecuta el comando ```terraform init -upgrade```.

Date cuenta de que terraform instalala última versión del AWS provider.

Abre el archivo ```.terraform.lock.hcl``` y mira cómo la versión del AWS provider está actualizada a la última versión.


Aplica tu configuración con la nueva versión del provider instalada y observa qué efectos puede tener no bloqeuar la versión del provider. El paso apply fallará porque el recurso ```aws_s3_buckt```, el atributo ```region``` es de solo lectura para los provider AWS v3.0.0+. Además el atributo ```acl``` está obsoleto para la versión del provider de AWS 4.0.0+.

Elimina los atributos ```acl``` y ```region``` del recurso ```aws_s3_bucket.sample``` en el archivo main.tf.

Ahora añade el siguiente recurso para establecer ACLs para tu objeto bucket.

```terraform
resource "aws_s3_bucket_acl" "example" {
  bucket = aws_s3_bucket.sample.id
  acl    = "public-read"
}
```

Aplica tu configuración. Responde afirmativamente a la solicitud de confirmación. 
Si el paso apply se completa satisfactoriamente, es seguro guradar la configuración con el lock file actualizado dentro del sistema de control de versiones. Si el plan o el paso apply falla, do guardes el lock file dentro de tu siste ma de control de versiones.

#### Limpia la infraestructura.
Después de comprobar que los recursos se han desplegado correctamente, destruyelos mediante el comando ```terraform destroy```. Recuerda responder ```yes``` a la solicitud de confirmación. 

Si has utilizado Terraform Cloud para este tutorial, después de destruir los recursos, elimina el espacio de trabajo ```learn-terraform-provider-verioning``` de tu organización Terraform Cloud. 

</details>

### [Archivo de Bloqueo de Dependencias (Dependency lock file)](https://developer.hashicorp.com/terraform/language/files/dependency-lock)

Un aconfiguración de Terraform puede referise a dos tipos diferentes de dependencias externas que vienen de fuera del propio código base:
- Providers, que son plugins que amplian Terraform para interactuar con sistemas externos.
- Módulos, los cuales dividen en grupos configuraciones Terraform escirtas en el lenguaje de Terraform, en abstracciones reutilizables.

Los dos tipos de dependencia pueden ser publicados y actulizados de forma independiente del mismo Terraform y de las configuraciones que dependen de ellas. Por esta razón Terraform debe determinaqué versiones de estas dependencias son compatibles con la configuración actual y cual versión están seleccionadas para utilizar.

Las restricciones de versión dentro de la configuración determina qué versiones  de las dependencias son potencialmente compatibles, pero después de seleccionar una versión específica de cada dependencia, Terraform recuerda las decisiones que ha hecho en un archivo de bloqueo de dependencias, de manera que pueda (por defecto) tomar la misma decisión en el futuro. 

En el momento presente, el archivo de bloqueo de dependencias lleva seguimiento únicamente de las dependencias de los provider. Terraform no recuerda versiones de las elecciones de los modulos remotos. De esta manera, Terraform siempre seleccionará la versión más reciente del módulo que cumpla las restricciones de versión para ese módulo. Puedes utilizar una versión exacta para segurar que Terraform siempre seleccionará esa misma versión para ese módulo. 

#### ¿dónde se encuentra el "Lock File"?
El archivo de bloqueo de dependencia es un archivo que pertenece a la configuración en general ás que a cada módulo por separado en la configuración. Por esta razón Terraform lo crea y espera encontrarlo en el actual directorio de trabajo cuando ejecutas Terraform, que es el directorio que contiene los arhcivos ```.tf``` del módulo raíz de tu configuración.

El archivo de bloqueo de depndencias (lock file) se llamará siempre ```.terraform.lock.hcl```, y este nombre significa que es un archivo de bloque para varios objetos que terraform atrapa en el subdirectorio ```.terraform``` de tu directorio de trabajo. 

Terraform automáticamente crea o actualiza el archivo de bloqueo de dependencias cada vez que ejecutas el comando ```terraform init```. Debes incluir este archivo en tu sistema de control de versiones de manera que puedas discutir posibles cambiso en tus dependencias externas a través de la revisión de código, tal como discutirías posibles cambios en tu propia confiuración. 

El archivo de bloqueo de dependencias utiliza la misma sintaxis de bajo nivel que el lenguaje principal de Terraform, pero el archivo de bloqueo de dependencias no es en sí mismo un archivo de configuración del lenguaje de Terraform. Se nombra con el sufijo ```.hcl``` en lugar de ```.tf``` para señalar esa diferencia.

#### Comportamiento de la instalación de dependencias.
Cuando ```terraform init``` está trabajando en instalar todos los prividers necesarios para una configuración, Terraform considera ambos tanto las restricciones de versión en la configuración y la versión seleccionada en el "lock file".

Si un provider en concreto no tiene una versión que haya sido guardada en el "lock file", Terraform seleccionará la versión más reciente disponible que encaje con las restricciones, entonces actualizará el "lock file" para incluir esa selección. 

Si un providider en concreto ya está registrado en el Lock File, Terraform siempre reseleccionará esa versión para la instalación, incluso si una nueva versión está disponible. Puedes sobreescribir este comportamiento añadiendo la opción ```-upgrade``` cuando ejecutas el comando ```terraform init```, en ese caso, Terraform seleccionará las versiones más recientes disponibles que encajen con las restricciones.

Si un ```terraform init``` realiza cambios en el "lock file", Terraform mencionaraá eso como parte de su salida. 

```sh
Terraform has made some changes to the provider dependency selections recorded
in the .terraform.lock.hcl file. Review those changes and commit them to your
version control system if they represent changes you intended to make.
```

Si ves este mensaje, puedes utilizar tu sistema de contorl e versiones para revisar los cambios que terraform ha propuesto en el archivo, y si son cambios que has hecho de forma intencional puedes enviar el cambio a traves de tu proceso habitual de revisión de código de tu equipo. 

#### Checksum verification
Terraform verificará que cada paquete que instala coincida con al menos uno de los "checksums" que previamente ha escrito en el "lock file", si no hay ninguno, devolverá un error indicando que no coincide ninguno de los checksums:

Esta comprobación de checksum está pensada para ............------------------------------------------



