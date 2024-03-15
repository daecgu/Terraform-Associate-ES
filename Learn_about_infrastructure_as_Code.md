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
Administrar exitosamente el cilco de vida de la infraestruhttps://registry.terraform.io/browse/providersctura es duro, y el impacto de malas decisiones pueden ser significativos, desde financieros hasta reputacionales, incluso la perdida de vidas considerando infraestructuras militares o guvernamentales. La adopción de herramientas IaC como Terraform, en conjunto con procesos, flujos de trabajo, es un paso necesario en la mitigación de riesgos. 


### Casos de uso de Terraform - https://developer.hashicorp.com/terraform/intro/use-cases
Esta página describe formas habituales en la que Terraform se utiliza, también se proporcionan recursos relacionas que puedes utilizar para crear configuraciones y flujos de trabajo. 

#### Despliegues multi-cloud
Desplegar infraestructura a través de múltiples nubes aumenta la tolerancia a falos, permitiendo una recuperación elegante ante los posibles cortes. Sin embargo, los despliegues multi-nube añaden complejidad porque cada proveedor tiene sus propias interfaces, herramientas y flujos de trabajo. Terraform permite queuses el mismo flujo de trabajo para administrar diversos proveedores y manejar dependencias entre nubes. Esto simplifica la gestión y orquestación a gran escala e infraestructuras multicloud.

Recursos: 
- Realiza el tutorial [Deploy Federated Multi-Cloud Kubernetes Clusters](https://developer.hashicorp.com/terraform/tutorials/networking/multicloud-kubernetes) para aprovisionar clusters de Kubernetes en Azure y AWS, configura la federación Consul con gateways de malla a través de los dos clústers, y despliega microservicios a lo largo de los dos clusters para verificar la federación.
- Navega en el [Terraform Registry](https://registry.terraform.io/browse/providers) para encontrar miles de "Providers" disponibles. 

#### Despliegue de infraestructura de aplicaciones, escalado y herramientas de monitorización. 
Puedes usar Terraform para desplegar, publicar, escalar y monitorizar infraestructura para aplicaciones de múltiples capas. La arquitectura de aplicaciones de N-capas permite escalar los componentes de la aplicación de manera independiente. Una aplicación podría consistir de un conjunto de servidores web que utilizan una capa de base de datos, y con capas adicionales para servidores API, servidores cache, y mallas de enrutamiento. Terraform permite administrar los recursos en cada capa juntos, y automáticamente gestiona las dependencias entre niveles. Por ejemplo Terraform desplegara una base de datos antes que ofrecer los servidores web que dependen del a base de datos. 

Recursos: 
- Realiza el tutorial [Automate Monitorin with the Terraform Datadog Provider](https://developer.hashicorp.com/terraform/tutorials/applications/datadog-provider) para desplegar una demo de aplicación Nginx dentro de un Cluster de Kubernetes con Helm e instalar el agente Datadog en el cluster. El Agetne Datadog reporta el estado del cluster al panel Datadog.
- Realiza el tutorial [Use Application Load Balancer for Blue-Green and Canary Deployments](https://developer.hashicorp.com/terraform/tutorials/aws/blue-green-canary-tests-deployments). Implementaras los entornos azul y verde, añadirás interruptores a confiuración de terrafom para definir una lista de estrategias de despliegue potenciales, realizarás una "canary test", e incrementarás tu entorno verde. 

#### Clusters propios.
En una gran organización, el equipo de operaciones centralizado, puede tener muchas peticiones de infraestructura repetitivas. Puedes utilizar Terraform para construir un modelo de infraestructura "self-serve", que permita a los equipos de producción manejar su infraestructura de forma independiente.  Puedes crear y utilizar Terraform modulos que codifiquen los estandares para el desplegar y gestionar servicios en tu organización, permitiendo que los equipos desplieguen eficientemente servicios, en conformidad con las prácticas de la organización. Terraform Cloud también se puede ntegrar con sistemas de ticketing para automáticamente generar solicitudes de infraestructura. 

Recursos: Intenta realizar el utotrial [Use Modules from the Registry](https://developer.hashicorp.com/terraform/tutorials/modules/module-use) para iniciarte en el uso de modulos públicos en tu configuración de Terraform. Intenta el tutorial [Build and Use a Local Module](https://developer.hashicorp.com/terraform/tutorials/modules/module-create) para crear un modulo para administrar AWS S3 Buckets. 

#### Cumplimiento y gestión de Políticas.
Terraform puede facilitar obligar el cumplimiento de las políticas en el tipo de recursos qeu los equipos pueden facilitar y utilizar. Los procesos de revisión basados en tickets son un cuello de botella que puede ralentaizar el desarrollo. En su lugar, puedes usar Sentinel, un marco de políticas como código, para hacer cumplir automáticamente las políticas de cumplimiento y gobernanza antes de que Terraform realice cambios en la infraestructura. Las políticas de Sentinel están disponibles en Terrafom Enterprise y Terraform Cloud.

Recursos:
- Realiza el tutorial [Control Costs with Policies](https://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cost-estimation) para estimar el costo del cambio de infraestructura y definir la política para limitarlo.
- La [documentación Sentinel](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement) facilita en mayor profundiad información y una lista de ejemplos de políticas que se pueden adaptar a tus casos de uso.

#### Configuración de aplicaciones PaaS
Los proveedores de Plataforma como servicio (PaaS) como Heroku, permiten crear aplicaciones web y añadir add-ons, como bases de datos o proveedores de email. Heroku puede escalar elásticamente el número de dynos o trabajadores, pero la mayoría de las aplicaciones no triviales necesitan mucho complementos y servicios externos. Puedes usar terraform para escribir la configuración requerida para una aplicación Heroku, configurar un DNSimple opara establecer un CNAME y configurar Cloudflare como una Red de Entreta de contenido (CDN) para la aplicación. Terraform puede rápidamente y consistentemente hacer todo esto sin una interfaz web.

Recursos:
- Realiza el tutorial [Deploy,manage, and Scale an Application on Heroku](https://developer.hashicorp.com/terraform/tutorials/applications/heroku-provider) para administar el ciclo de vida de una aplicación con Terraform.

  
#### Redes definidas por Software
Terraform puede interactuar con Software Defined Networks (SDNs) para automáticamente configurar la red de acuerdo a las necesidades de la aplicación que está corriendo en ella. Esto permite pasar de un flujo de trabajo basado en tickets a uno automátizado, reduciendo el tiempo de despliegue. 

Por ejemplo, cuando un servicio se registra en [HashiCorp Consul](https://www.consul.io/), [Consul-Terraform-Sync](https://developer.hashicorp.com/consul/docs/nia) puede automáticamentegenerar una configuración Terraform para exponer los puertos apropiados y ajustar la configuración de la red para para cualquier SDN que tenga asociado un Terraform Provider. La infraestructura de red automatizada (Network Infraestructur Automatio - NIA) permite aprobar cambios de forma segura sin tener que traducir manualmente los tickets de los desarrolladores en los cambios que crees que su aplicación necesita.

Recursos:
- Realiza el tutorial [Network Infrastructure Automation with Consul-Terraform-Sync Intro](https://developer.hashicorp.com/consul/tutorials/network-infrastructure-automation/consul-terraform-sync-intro) para instalar el Consul-Terraform-Sync en un nodo. Lo configurarás para que se comunique con un centro de datos Consul, reaccionar a los cambios en el servicio, y ejecutar una tarea de ejemplo.
- Realiza el tutorial [Consul-Terraform-Sync And terraform Enterprise/Cloud Integration](https://developer.hashicorp.com/consul/tutorials/network-infrastructure-automation/consul-terraform-sync-terraform-enterprise) para configurar Consul-Terraform-Sync para que interactue con Terraform Enterprise y Terraform Cloud.


#### Kubernetes
Kubernetes es un programador de cargas de trabajo de ćodigo abierto para aplicaciones contenerizadas. Terraform permite tanto desplegar un clúster de Kubernetes como administrar sus recursos (por ejemplo: pods, deployments, services, etc.) También se puede utilizar el [Kubernetes Operator for Terraform](https://github.com/hashicorp/terraform-k8s) para gestionar infraestructura cloud y on-prem a través de un Kubernetes Custom Resource Definition (CRD) y Terraform Cloud.

Recursos:
- Realiza el tutorial [Manage Kubernetes Resources via Terraform](https://developer.hashicorp.com/terraform/tutorials/kubernetes/kubernetes-provider). Utilizarás terraform para programar y exponer un despliegue de Nginx en un cluster de Kbuernetes.
- Realiza el tutorial [Deploy INfrastructure with the Terraform Cloud Operator for Kubernetes](https://developer.hashicorp.com/terraform/tutorials/kubernetes/kubernetes-operator). Configurarás y desplegaras el Operador en un cluster Kubernetes y lo utilizaras para crear un espacio de trabajo Terraform Cloud y y provisionar un mensaje de cola para un ejemplo de aplicación. 

#### Entornos Paralelos
Es probable que tengas entornos de prueba o de control de calidad (QA) que utilizas para probar nuevas aplicaciones antes de lanzarlas en producción. A medida que el entorno de producción se vuelve más grande y complejo, puede ser cada vez más dificl mantener un entorno actualizado para cada etapa del proceso de desarrollo. Terraform Permite crear y desmantelar infrastructura para desarrollar purebas, QA y producción. Terraform permite crear entornos desechables según sea necesario, sienod más eficiente en términos de costo que mantener cada uno indefinidamente.

#### Demos de software. 
Puedes utilizar Terraform para crear, provisionar e inicializar una demostración en varios proveedores de nube. Esto permite a los usuarios finales probar fácilmente el software en su propia infrastructura e incluso les habilita para ajustar parámetros como el tamaño del clúster para probar las herramientas de manera más rigurosa a cualquier escala. 
