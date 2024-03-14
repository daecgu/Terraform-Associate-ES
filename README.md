# Terraform-Associate-ES
Traducción al Español de la Study Guide y Review Guide para la certificación de HashiCorp
(Todas las imágenes pertenecen a la documentación oficial de HashiCorp. No hay ninguna imagen propia.)

# Study Guide - Terraform Associate certification

Esta guía lista los recursos que debes estudiar si estás preparandote para la certificación Terraform Associate. Los recursos aparecen en orden de dificultad para que progreses conforme avanzas en la lista. Los recursos relacionados con un objetivo particular del examen, es preciso ir a Exam Revie Guide. 

##  Aprende sobre Código como Infraestructura - Learn about infrastructure as Code (IaC)
Terraform es una herramienta que permite definir infraestructura en código legible tanto por personas como por máquinas. Revisa los siguientes recursos para comenzar a aprender las ventas de la Infraestructura como código (IaC), y las ventagas de Terraform. 

### Infraestructura como código (Video introductorio): https://www.hashicorp.com/resources/what-is-infrastructure-as-code
La infraestructura como código es un patrón dominante para administrar infraestructura mediante archivos de configuración. Esto permite consturir, cambiar, administrar la infraestructura de forma segura, consistente,rastreable, y reproducible definiendo configuraciones que puedes versionar, reutilizar y compartir.  

### Introducción a IaC
Terraform es una herramienta de infraestructura como código (IaC) que permite construir, cambiar, y versionar los recursos Cloud y On-prem de forma segura y eficiente. Lo realiza mediante archivos de configuración que son legibles por personas y que se pueden versionar, reutilizar y compartir. Entonces se puede utilizar un flujo de trabajo para aprovisionar y controlar toda la infraestuctura a lo largo de su ciclo de vida. Terraform puede controlar los componentes de bajo nivel como capacidad de cómputo, almacenamiento y los recursos de red. Así mismo también puede controlar los componentes de alto nivel como DNS y características Saas. 

¿cómo funciona Terraform?
Terraform Ccrea y controla los recursos en la nube y otros servicios a través de sus APIs. Los proveedores habilitan Terraform para trabajar con cualquier plataforma o servicio mediante una API. HashiCorp y la comunidad de Terraform han escrito miles de proveedores para controlar distintos tipos de recursos y servicios. Los puedes encontrar disponibles en https://registry.terraform.io/ 

<img src="https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dterraform%26version%3Drefs%252Fheads%252Fv1.7%26asset%3Dwebsite%252Fimg%252Fdocs%252Fintro-terraform-apis.png%26width%3D2048%26height%3D644&w=2048&q=75" width="800" height="300">

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
