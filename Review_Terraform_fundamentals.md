# Fundamentos de Terraform

Completa los tutoriales ["Get Started"](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) para crear modificar y destruir tu primera infraestructura utilizando Terraform y aprender acerca de los Terraform providers y los conceptos básicos de Terraform state.
## Tutoriales
Algunos tutoriales tienen laboratorios interactivos, o tienen su documentación para Windos/Linux/Mac. EN ESTE PROYECTO DE TRADUCCIÓN únicamente cubriré Linux y en concreto Ubuntu/Debian, el resto quedan en los enlaces a la la página original que traduzco. 
### [¿Qué es infraestructura como código con Terraform?](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code)
Este tutorial está traducido en la sección [Aprende acerca de la infraestructura como código (IAC)](https://github.com/daecgu/Terraform-Associate-ES/blob/main/Learn_about_infrastructure_as_Code.md#introducucci%C3%B3n-a-la-infraestructura-como-c%C3%B3digo-con-terraform---httpsdeveloperhashicorpcomterraformtutorialsaws-get-startedinfrastructure-as-code). Por lo tanto no lo traduciré nuevamente en esta sección.

Contiene un Laboratorio Interactivo.

### [Instalación de Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
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



Texto
---------------
```  ```
--------------
```sh

```
