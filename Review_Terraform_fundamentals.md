# Fundamentos de Terraform

Completa los tutoriales ["Get Started"](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) para crear modificar y destruir tu primera infraestructura utilizando Terraform y aprender acerca de los Terraform providers y los conceptos básicos de Terraform state.
<details>
<summary> Tutoriales "Get Started" </summary>
  
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

</details>
________________________________________________

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
