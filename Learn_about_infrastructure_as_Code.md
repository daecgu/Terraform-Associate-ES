# Aprende sobre Código como Infraestructura - Learn about infrastructure as Code (IaC)
Terraform es una herramienta que permite definir infraestructura en código legible tanto por personas como por máquinas. Revisa los siguientes recursos para comenzar a aprender las ventas de la Infraestructura como código (IaC), y las ventagas de Terraform. 

### Infraestructura como código (Video introductorio) - https://www.hashicorp.com/resources/what-is-infrastructure-as-code
La infraestructura como código es un patrón dominante para administrar infraestructura mediante archivos de configuración. Esto permite consturir, cambiar, administrar la infraestructura de forma segura, consistente,rastreable, y reproducible definiendo configuraciones que puedes versionar, reutilizar y compartir.  

### Introducción a IaC - https://developer.hashicorp.com/terraform/intro
Terraform es una herramienta de infraestructura como código (IaC) que permite construir, cambiar, y versionar los recursos Cloud y On-prem de forma segura y eficiente. Lo realiza mediante archivos de configuración que son legibles por personas y que se pueden versionar, reutilizar y compartir. Entonces se puede utilizar un flujo de trabajo para aprovisionar y controlar toda la infraestuctura a lo largo de su ciclo de vida. Terraform puede controlar los componentes de bajo nivel como capacidad de cómputo, almacenamiento y los recursos de red. Así mismo también puede controlar los componentes de alto nivel como DNS y características Saas. 

¿cómo funciona Terraform?
Terraform Ccrea y controla los recursos en la nube y otros servicios a través de sus APIs. Los proveedores habilitan Terraform para trabajar con cualquier plataforma o servicio mediante una API. HashiCorp y la comunidad de Terraform han escrito miles de proveedores para controlar distintos tipos de recursos y servicios. Los puedes encontrar disponibles en https://registry.terraform.io/ 

<img src="https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dterraform%26version%3Drefs%252Fheads%252Fv1.7%26asset%3Dwebsite%252Fimg%252Fdocs%252Fintro-terraform-apis.png%26width%3D2048%26height%3D644&w=2048&q=75" width="900" height="350">

El flujo de trabajo principal de terraform tiene tres etapas: 
- Escribir: Defines los recursos, que pueden estar en multiples proveedores cloud y servicios. Por ejemplo, puedes crear una configuración para desplegar una aplicación en máquinas virtuales dentro de una Nube Privada Virtual, con grupos de seguridad y un balanceador de carga.
- Plan: Terraform crea un plan de ejecución en el que describe la infraestructura que creara, actualizará o destruirá basado en la infraestructura existente y la configuración.
- Aplicar: cuando se confirma, Terraform ejecuta las operaciones propuestas en el orden correcto, respetando las dependencias entre recursos. Por ejemplo: si actualizamos las propiedades de una Nube privada Virtual (VPC) y modificamos el número de máquinas virtuales en esa VPC, Terraform recreará la VPC antes de escalar las máquinas virtuales.
  
<img src="https://github.com/daecgu/Terraform-Associate-ES/assets/100792073/918d16ec-b78e-40fe-83dd-ff79b272e8e1" width="900" height="800">


#### Administra culaquier infraestructura
Encuentra proveedores para muchas de las plataformas y servicios que ya utilizas en: https://registry.terraform.io/
También puedes escribir tu propio "Terraform Provider" (https://developer.hashicorp.com/terraform/plugin). Es preciso tener en cuenta que Terraform toma un enfoque de infraestructura inmutable, reduciendo la complejidad de actualización y modificación de serviceios e infraestructura. (https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure)

#### Seguimiento de la Infraestructura.
Terraform genera un plan y te solicita autorización antes de modificar la infraestructura. También realiza seguimiento de la infraestructura en un archivo de estado ("state file"), que actúa como referencia esencial y confiable del entorno. 

#### Automatización de cambios
Los archivos de configuración de terraforme son declarativos. Esto quiere decir que describen el estado final de la infraestructura. No es preciso escribir paso a paso las instrucciones para crear recursos, ya que Terraform maneja lo que hay por debajo. 

#### Estandariza las configuraciones.
Terraform permite reutilizar componentes de configuración denominados módulos, los cuales, definen colecciones configurables de infraestructura, ahorrando tiempoo y animando a las mejores prácticas. Se puede usar los módulos públicos disponibles en el registro de terraform, o escribir propios. 

#### Colabora
Ya que la configuración está escrita en un archivo, se puede utilizar en un Sistema de Control de Versiones (Version Control System - VCS). y utilizar Terraform Cloud para realizar el flujo de trabajo mediante equipos. Terraform Cloud ejecuta Terrafornm de manera consistente, en un entorno confiable y proporciona acceso seguro a datos que no deben conocerse, permisos basados en roles, un registro privado para compartir modulos y providers, y más. 

### Introducucción a la infraestructura como código con Terraform - https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code
Terraform es una herramienta de infraestructura de HashiCorp. Te permite definir recursos e infraestructura de forma legible para personas, archivos de configuración declarativos, y administrar el ciclo de vida de tu infraestructura. Utilizar Terraform tiene diversas ventajas sobre administrar tu infraestructura manualmente:
- Terraform puede administrar la infraestructura en multiples plataformas.
- El lenguaje de configuración legible para las personas ayuda a escribir el código rápidamente.
- El estado de Terraform permite realizar un seguimiento de cambios en los recursos a lo lartgo de los despliegues.
- Puedes utilizar un control de versiones para los archivos de configuración y trabajar conjuntamente en la infraestructura.

#### Administra cualquier infraestructura.
Los plugins de Terraform, llamados "providers", permiten a Terrafom interactuar con las plataformas en la nube y otros servicios mediante APIs. HashiCorp y la comunidad de Terraform ha nescrito más de 1000 "providers" para administrar recursos en Amazon Web Servcies (AWS), Azure, Google Cloud Platform (GCP), Kubernetes, Helm, GitHub, Splunk, y DataDog, por mencionar unos pocos. Encuentra proveedores para muchas de las plataformas y serviceios que ya utilizas en https://registry.terraform.io/browse/providers. Si no encuentras el que buscas, puedes escribir uno por tí mismo. 

#### Estandariza el flujo de trabajo de tus despliegues.
Los Providers definen unidaes individuales de infraestructura, por ejemplo, las instancias de cómputo o redes privadas, como recursos. Puedes componer recursos de de diferentes Provideres en una configuración reutilizable de Terraform llamada módulo, y administralos mediante un lengujae consistente y un flujo de trabajo. 

El lenguaje de configuración de Terraform es declarativo, lo que significa que describe el estado deseado para tu infraestructura. Al contrario que los lenguajes de programación habituales que requieren indicar paso a paso las instrucciones para realizar una tarea. Los Terraform Provideres automáticamente calculan las dependencias entre recursos para crearlos o eliminarlos en el orden correcto. 

<img style="background-color:white" src="https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dtutorials%26version%3Dmain%26asset%3Dpublic%252Fimg%252Fterraform%252Fterraform-iac.png%26width%3D2400%26height%3D870&w=3840&q=75" width="900" height="350" >

Para desplegar infraestructura con terraform:
- Ámbito/Alcance : indentifica la infraestructura de tu proyecto.
- Autor: Escribe la configuración de tu infraestructura.
- Inicialización: instala los plugins que utiliza Terraform para gestionar la infraestructura.
- Plan: previsualización de los cambios que Terraform realizará para que coincida con tu configuración.
- Aplicar: realizar los cambios planeados.

#### Seguimiento de la infraestructura.
Terraform lleva un seguimiento de la infraestructura real en un fichero de estado. Este actua como fuente de verdad para en entorno. Terrarm utiliza el fichero de estado (State file) para determinar los cambios que debe realizar a la infraestructura, de manera que coincida con la configuración. 

#### Colabora
Terrafrom permite colaborar en tu infraestructura con sus "backends " de estado remoto. Cuando utilizas Terraform Cloud (Gratis hasta 5 usuarios), puedes compartir de forma segura tu estado con tus compañeros, proporconar un entorno estable para que Terraform lo ejecute, y prevenir posibles incidencias cuando varias personas realizan cambios de configuración al mismo tiempo. 
Además puedes conectar Terraform Cloud al sistema de control de versiones (VCS) como GitHub, GitLab, y otros, permitiendo que proporcione cambios en la infraestructura cuando realizas un "Commit" en el VCS. Esto te permite administrar los cambios en la infraestructura a través del control de versiones, tal y como lo harías en con el código de una aplicación. 

### Infraestructura como codigo en nubes privadas y públicas - https://www.hashicorp.com/blog/infrastructure-as-code-in-a-private-or-public-cloud/
Según la tecnología avanza, las herramientas cambian. Como la mayoría de personas ressiten al cambio esto conlleva algún tipo de fracaso hasta que nos decidimos a cambiar nuestra forma de administrar y dirigir. 

Por ejemplo, conforme las empresas migran a la nuve, tienden a gestionar su infraestructura cloud de la misma manera que lo hacían on-premise, accediendo desde GUI o CLI. Estos usuarios no han adoptado la Infraestructura como Código (IaC) a través del uso de herramientas como Terraform de HashiCorp. 

¿Por qué cambiar como definimos y construimos la infraestructura?
"Virtual compute" nos permitió consruir y aplicar cambios en la configuración mediante comandos software. Mientras que estos comandos, habitualmente son scripts, son dificiles para las personas de entender. Herramientas más moderans aceptan codigo que es más inteligible para personas y máquinas, proporcionando beneficios. Han simplificado el testing, aplicar y llevar seguimiento entre iteraciones, y más importante, ha npermitido que equipos reutilicen componentes (por ejemplo, módulos) a través de distintos proyectos. No hay duda que IaC ha haya obtenido una gran adopción. 

#### IaC y el ciclo de vida de la infraestructura.
¿Cómo encaja IaC dentro del ciclo de vida de la infraestructura? Puede ser aplicado a lo largo de todo el "lifecycle", tanto en el despliegue inicial como durante la vida de la infraestructura. Habitualmente han sido referidas como actividades del Día 0 y Día 1. El código del día 0 prepara y configura la infraestructura inicial. 

si tu infraestructura no cambia nunca después del despliegue inicial, probablemente no necesites herramientas que ofrezcan actualizaciones, cambios y expansiones. Día 1 se refiere a configuraciones en el sistema operativo y aplicación despues de haber desplegado la estructura inicial. 

IaC hace facil entender la intencionalidad  de cambios en la infraestructura, porque puede abarcar múltiples archivos, permitiendo a las personas organizar el código según la intención. Por ejemplo, se podría crear diferentes ficheros para definir diferentes componentes de infraestructuras, o separar definiciones de variables de los bloques de ejecución sin afectar a la ejecución. 

Aquí hay un ejemplo de código que la herramienta de IaC Terraform utilizaría para aprovisionar un Amazon VPC. Observa cómo el código es legible tanto para personas como para máquinas.

```terraform
resource "aws_vpc" "default" {
  cidr_block = "10.0.0.0/16"
}
```

Herramientas  como Terraform habitualmente incluyen librerías de proveedores y módulos para hacer fácil escribir el código para realizar las configuraciones. Con térraform, especialmente en el Día 0, es habitual aplicar configuraciones como las que se muestra a continuación, como pueden ser instalar e iniciar un web server:

```terraform
provisioner "remote-exec" {
  inline = [
    "sudo apt-get -y update",
    "sudo apt-get -y install nginx",
    "sudo service nginx start"
    ]
}
```
Si es necesario aplicar Día 1 a través de Día N configuraciones, el código puede aprovechar herramientas como Chef, Ansible, Docker, etc.

```terraform
provider "chef" {
  server_url = "https://api.chef.io/organization/example"
  run_list = [ "recipe[example]" ]
}
```

#### IaC hace la infraestructura más confiable
IaC hace que los cambios sean idempotentes, consistentes, reproducibles, predecibles. Sin IaC, escalar infraestructura para un incremento de la demanda puede requereri un operador remoto para conectarse a cada máquina y luego manualmente configurar muchos servidores ejecutando una serie de comandos o scripts. Puede abrir multiples sesiones y moverse entre ventanas, lo cual sulee resultar en saltarse pasos o haya pequeñas variaciones y que el trabajo se complete o que sea necesario un "rollback". 

Este proceso puede terminar en inconsistencias que deriven en diferencias entre servidores en rendimiento, utilidad o seguridad. Si un equipo está aplicando cambios, el reisgo incrementa, ya que no todos los individuos siguen siempre las instrucciones de la misma manera. 

Mediante IaC,se puede testear el código y revisar los resultados antes de que el código sea aplicado en nuestros entornos. Si un resultado no se ajusta a nuestras expectativas, iteramos sobre el código hasta que los resultados pasen nuestras pruebas y estén alineados con nuestras expectativas. Seguir este modo de trabajar permite que el resultado sea predecido antes de que el código sea aplicado en un entorno de producción.  Una vez listo para utilizar, entonces podemos aplicar el código vía automatización, escalar, y asegurar consistencia y que sea repruducible en cómo se ha aplicado.

Desde que el código ha sido alojado en un sistema de version de controles tal como GitHub, GitLab, BitBucket, etc. Es posible revisar cómo la infraestructura evoluciona en el tiempo. La característica de ser idempotente (la propiedad de que siempre que se multiplique por sí mismo da él mismo, es decir no hay cambio), proporcionada por IaC asegura que, incluso si el mismo codigo es aplicado muchas veces, el resultado se mantiene igual. 

#### IaC hace que la infraestructura sea más manejable.
Aprovechar la Infraestructura como Código (IaC) de HashiCorp Terraform ofrece beneficios que permiten el cambio a través del código. Considera un entorno que ha sido provisionado y que contiene dos servidores y un balanceador de carga. Para abordar el aumento de carga, se necesitan servidores adicionales. La IaC puede ser revisada, con cambios mínimos, para poner en línea nuevos servidores utilizando la configuración previamente definida.

Durante la ejecución, Terraform examinará el estado de la infraestructura en funcionamiento, determinará las diferencias entre el estado actual y el estado deseado actualizado. Entonces indicará los cambios necesarios que deben aplicarse. Cuando se aprueba, se aplicarán los cambios necesarios, dejando la infraestructura existente sin cambios.

#### La Infraestructura como código Tiene Sentido
Administrar exitosamente el cilco de vida de la infraestructura es duro, y el impacto de malas decisiones pueden ser significativos, desde financieros hasta reputacionales, incluso la perdida de vidas considerando infraestructuras militares o guvernamentales. La adopción de herramientas IaC como Terraform, en conjunto con procesos, flujos de trabajo, es un paso necesario en la mitigación de riesgos. 


### Casos de uso de Terraform - https://developer.hashicorp.com/terraform/intro/use-cases

#### Casos de uso

#### Despliegues multi-cloud

#### Despliegue de infraestructura de aplicaciones, escalado y herramientas de monitorización. 

#### Clusters propios.

#### Cumplimiento y gestión de Políticas.

#### Configuración de aplicaciones PaaS

#### Redes definidas por Software

#### Kubernetes

#### Entornos Paralelos

#### Demos de software. 
