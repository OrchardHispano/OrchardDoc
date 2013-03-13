Hay un montón de características disponibles para Orchard que están listas para usarse.
Y en muchas ocasiones más características todavía desde la [gallery][1].
Este artículo proporciona una breve descripción de cada característica desarrollada por Orchard.

Esta es una lista organizada alfabéticamente por el nombre de los módulos, en la que dos secciones
principales separan los módulos que forman parte del núcleo y los que no.
Las características principales no pueden ser deshabilitadas y todas están siempre activadas,
pero hay características que están implementadas en módulos del núcleo que pueden activarse y desactivarse.
Estas características simplemente no están en la categoría "Core".

Cada módulo detalla sus características, y siempre del mismo modo si está disponible desde el paquete de WebPI, o solo desde la galería o a través de las release del código fuente.

# Módulos principales (Core modules)

## Common

Las tres partes principales del contenido Body,Common e Identity, así como también el campo de texto Text field, está implementadas en el módulo Common.

La parte Body representa un bloque de texto enriquecido. Este está configurado por defecto para usar HTML
a través de TinyMCE, pero puede ajustarse para usar texto plano, Markdown o cualquier otro tipo de texto personalizado.

La parte Common captura las fechas de creación, modificación y publicación, así como también el propietario del content item.
El contenedor del item (container item) también puede encontrarse en este part, de manera que en este part,
también podemos implementar las relaciones de jerarquía del tipo padre / hijo como pueden ser blog / entrada o carpeta / archivo.

La parte identidad también es útil para proporcionar una identidad única a un content item que no tiene
una de forma natural. Por ejemplo, las páginas son identificadas de forma natural por su ruta relativa dentro del sitio web, o por ejemplo
los usuarios son identificados por el nombre de usuario, sin embargo, los widgets no tienen una identidad natural como éstas.
Con el objetivo de permitir la importación y exportación de estos items, la part Identity puede ser añadida.
Ésta part crea GUIDs (números de identificación) que son usados como una identidad única que puede relacionar los intercambios de contenidos entre distintas instancias de Orchard.

El campo de texto (Text field) es muy similar a la part Body, excepto que es un campo en lugar de una part.
El Text field activa el uso de múltiples instancias de un mismo item individual. Tiene otros pequeños beneficios
como la caja de una línea individual o de texto sin formato.

## Containers

Este módulo introduce cuatro partes que son útiles para crear jerarquías de contenido sencillas.
Esto es la base para el módulo Orchard.Lists que ha llegado a ser algo así como las Listas obsoletas que han sido
desestimadas en favor de una clasificación de modelos en el contenido como las taxonomías o unos mejores
mecanismos de búsqueda como por ejemplo las proyecciones.

La part Container puede ser agregada al tipo para marcar su habilidad de contener ciertos tipos de content items.
Éste también tiene propiedades que especifican el concepto por el que pueden ordenarse y la paginación.

La part Containable especifica que un tipo puede ser contenido. Esta trabaja mano a mano con la part Container
y permiten que el editor de contenido pueda escoger un contenedor para el item que se quiere editar.

El Container Widget part es similar al Container part, excepto que está hecho para ser usado dentro del Container Widget,
además posee habilidades especiales para filtrar para ser capaz de mostrar sólo un subconjunto de items presentes en el Container.
No es solo un contenedor en sí mismo, sino que además referencia y re-usa un container item existente.

La part de propiedades personalizadas ofrece tres propiedades personalizadas de texto que son útiles para implementar el filtrar y ordenar.
Esta part está desfasada ya que las proyecciones ahora permiten filtrar y ordenar en campos (fields).

## Contents

Este módulo principal crea la infraestructura para content types personalizados (tipos de contenido personalizados).

### Características

* Contents (Core): la infraestructura para tipos personalizados (custom types).
* Content Control Wrapper (desactivado por defecto): añade un botón de editar en el front-end.

## Dashboard

Implementa el panel de administración como un caparazón extensible.

## Feeds

El módulo principal Feeds proporciona la infraestructura que otros módulos pueden usar para distribuir a través de enlaces RSS, Atom o cualuier otro tipo de feeds.

## Navigation

Desde Orchard 1.5, la aplicación se distribuye con un menú de navegación jerárquico y extensible.

Pueden llamarse más de una instancia de menú, que pueden construirse con este módulo. Los items del menú pueden ser proporcionados por cualquier número de proveedores. El módulo ofrece items personalizados que pueden apuntar a cualquier URL, y items de contenido de menú que apuntan a un item de contenido específico.

Otros módulos, como son los taxonomies o los projections, pueden proporcionar sus propios proveedores de items de menú dinámicos.

Los menús pueden pueden ser renderizados como unos widgets menú, o como unos breadcrum o menús locales.


Un content item puede ser añadido a los menús suando el Navigation part. El Menu part, que cubría satisfactoriamente esta función antes de Orchard 1.5, está desfasado pero todavía proporciona compatibilidad con versiones anteriores (back-compatibility) de los datos de Orchard.

Este módulo principal también proporciona el menú de administración.

## Reports

El módulo principal Reports (reportes) establece la infraestructura para generar y mostrar reportes básicos.
Es usado durante la primera configuración para hacer un log de varias operaciones de inicio, incluidas las operaciones de base de datos. 

## Scheduling

Las APIs para este módulo principal pueden ser usadas por otros módulos como el Publish Later (publica después) para planificar operaciones que serán ejecutadas en el futuro.

## Settings

La mayoría de las configuraciones del sitio en Orchar se almacenan en un content item.
El content type Site (tipo de contenido sitio) es el que es usado para esto, y como cualquier otro content type en Orchard, puede extenderse. Esto permite que los módulos puedan contribuir a su propia configuración. El módulo Settings es el que permite este escenario. 

## Shapes

Shapes en Orchard son unidades básicas de interfaz de usuario (UI) a partir de las cuales es construido todo el HTML. Es posible añadir nuevos shapes a través de módulos dinámicamente, pero el módulo Shapes ya proporciona algunas shapes básicas y estándard.

Las shapes principales (Core Shapes) están definidas a través de código en CoreShapes.cs. Otras shapes están definidas en vistas como archivos ".cshtml".
Cualquier shape, tranto principal como no, pueden ser sobreescritas por plantillas (templates) en un tema (theme).

### Core shapes (shapes principales)

* **ActionLink:** usa valores por ruta (route values) para construir un link HTML.
* **DisplayTemplate, EditorTemplate:** son usando internamente para constrir las vistas públicas (display view) y de edición de los content items.
* **Layout**: es el shape más exterior, que junto con su Wrapper y su Document, define la estructura básica de HTML para ser renderizada. También define las zonas de nivel superior.
* **List**: es un shape estandard para renderizar listas de shapes.
* **Menu, MenuItem, LocalMenu, LocalMenuItem y MenuItemLink:** son las sapes que renderiza la navegación.
* **Pager (paginador) y shapes asociados y alternates:** los shapes usados para renderizar la paginación.
* **Partial:** un shape que permite renderizar una plantilla como una vista parcial, usando un modelo específico. Crear un shape dinámico es normalmente la técnica más adecuada.
* **Resource, HeadScripts, FootScripts, Metas, HeadLinks, StyleSheetLinks, Style:** shapes usados para renderizar recursos como etiquetas de scripts u hojas de estilo.
* **Zone:** un shape especial que está hecho para contener otros shapes, normalmente contienen widgets pero no están limitados a ellos.

### Templated shapes

* **Breadcrumb**: una representación jerárquica de una lista de enlaces de items de menú.
* **ErrorPage and NotFound:** las páginas que son renderizadas en el caso de que ocurra un error de servidor o un error 404 de recurso no encontrado.
* **Header:** la cabecera visual de la página (no la etiqueta head del documento).
* **Message:** este shape es usado para rendereizar información, alertas o mensajes de error.
* **User:** muestra los enlaces de login, logout, panel de usuario y cambio de contraseña.

## Title (título)

Este módulo sencillo inserta el part Title que es usado por la mayoría de tipos de contenido (content types).

## XmlRpc

Este módulo implementa las APIs necesarias para crear contenido como páginas y entradas de blog para aplicaciones como Windows Live Writer.
El módulo Orchard.Blogs se construye sobre el módulo XmlRpc para permitir específicamente la creación de entradas de blog.

# Módulos secundarios

## Markdown (WebPI, desactivado por defecto)

[Markdown][2] es un formato de texto con una sintaxis fácil de leer por las personas (human-readable) usado para describir texto enriquecido sin la complejidad del HTML. Algunas personas prefieren escribir Markdown en lugar de utilizar un editor de texto WYSYWYG, como el editor de texto por defecto que viene con Orchard, TinyMCE.

Una vez has activado la característica Markdown, puedes crear nuevos content types o editar los existentes para usar el formato Markdown en lugar de HTML abriendo la configuración del part Body y en la configuración flavor cambiando de "html" a "markdown". El editor de Markdown se mostrará en lugar del TinyMCE en el editor del content item.

## Orchard.Alias (WebPI)

El módulo Alias establece la infraestructura para mapear direcciones amigables a content items y rutas personalizadas. Este módulo es la base sobre la que está construido Autoroute.

### Características

* **Alias:** esta es la pieza principal de la infraestructura para que aliases (pseudónimos) funcione.
* **Alias UI (desactivado por defecto):** proporciona la interfaz de usuario para modificar, crear y eliminar aliases.

## Orchard.AntiSpam (WebPI, desactivado por defecto)

El módulo AntiSpam proporciona varias funcionalidades para la cucha contra la publicidad, correo y mensajes no deseados (spam) y las piezas de la infraestructura. Hace posible prevenir el spam en contenido arbitrario ( las versiones previas de Orchard sólo ofrecían servicios anti-spam en los comentarios).

Con el módulo AntiSpam, puedes añadir un captcha, filtros anti-spam externos o establecer límites de aceptación de contenido añadiendo algunos part a tus tipos, incluyendo formularios personalizados.

### Características

* **Anti-Spam:** las piezas principales de la infraestructura anti-spam. También proporciona la parte [ReCaptcha][3] que puede ser añadida a content types (tipos de contenido) para añadir CAPTCHA a su formulario de edición.
* **Akismet Anti-Spam Filter:** permite usar el servicio de terceros [Akismet][4] con los content types de Orchard.
* **TypePad Anti-Spam Filter:** permite usar el servicio de terceros [TypePad][5] con los content types de Orchard.

## Orchard.ArchiveLater (Archiva después)

Usando el part proporcionado por este módulo, puedes programar que un content item sea archivado.

Este módulo está disponible desde los paquetes de código fuente o [desde la galería][28].

## Orchard.Autoroute (WebPI)

Esta poderosa funcionalidad hace posible que los creadores de tipos de contenido (content type) puedan especificar una arquitectura de enlaces basada en tokens.

Por ejemplo, si quieres que una URL de tu entrada de blog sea de la forma posts/2012/7/el-mejor-post-que-has-leido-nunca puedes ir al editor del content type para entradas de blog, desarrollar la configuración para el part Autorute y establecer el patrón a "posts/{Content.Date.Format:yyyy}/{Content.Date.Format:MM}/{Content.Slug}".

Autoroute está construido sobre la característica Alias.

## Orchard.Blogs (WebPI)

El módulo Blogs proporciona las características y funciones de blog para Orchard. Este módulo está fuertemente relacionado con la composición de content type y otras características como los Comments.

### Ver también

* [Añade un blog a tu sitio web][6]
* [Crea entradas de blog con Windows Live Writer][7]

## Orchard.CodeGeneration

Este módulo proporciona a los desarrolladores comandos de scaffolding que ayudan a la creación de nuevos módulos y temas.

El módulo CodeGeneration está disponible desde los paquetes de código fuente o [desde la galería][29].

## Orchard.Comments (WebPI)

Puedes usar el part Comments proporcionado por este módulo o cualquier otro content type para permitir a los usuarios de tu sitio web opinar y comentar el contenido.

### Ver También

* [Moderar comentarios][8]

## Orchard.ContentPermissions (WebPI, desactivado por defecto)

Sin este módulo, Orchard solo proporciona permisos configurables para todos los content types. El módulo Orchard.ContentPermissions proporciona un part que puede ser añadido a cualquier tipo de contenido para restringir permisos de visualización por cada item de contenido en lugar de por el tipo de contenido.

## Orchard.ContentPicker

Este módulo proporciona un selector de items de contenidos extensible que puede usarse para construir relaciones entre content items.

El módulo ContentPicker está disponible desde los paquetes de código fuente o [desde la galería][30].

## Orchard.ContentTypes (WebPI, desactivado por defecto)

Activa este módulo para permitir la creación y modificación de tipos de contenido desde el panel de administración.

### Ver También

* [Crear tipos de contenido personalizados][9]

## Orchard.CustomForms (WebPI, desactivado por defecto)

Los formularios personalizados (custom forms) están construidos como tipos de contenido, normalmente usando campos (fields).
Una vez has construido el content type para tu formulario personalizado, puedes activar sus instancias para ser creadas en el front-end por usuarios anónimos.

Esto es útil, por ejemplo, para crear formularios de contacto: activa la característica, crea un tipo de contenido "Formulario de Contacto", añade los campos de texto de nombre, e-mail y mensaje (selecciona TextArea como opción de vista en la configuración de campo), haz click en "Formularios" en el menú de administración, haz click en "Añadir un nuevo formulario personalizado", selecciona "Contact Us" como el tipo de contenido para el formulario y publícalo. Si activas la característica Rules, puedes crear una norma que envía un e-mail cuando un ítem del tipo "Contact Us" es publicada. Deberías también garantizar el permiso "Submit Contact Form" a role anónimo desde la pantalla de administración "Usuarios/Roles", bajo "Formularios personalizados" de manera que permitas a los usuarios anónimos enviar formularios de contacto.

## Orchard.DesignerTools

Este módulo contiene algunas características que ayudan con el desarrollo de temas.

### Características

* Shape Tracing: proporciona una aplicación al estilo de Firebug que puede usarse para explorar la estructura shape del lado del servidor de una página, genera alternativas, e inspecciona el modelo, la ubicación y las plantillas para cada shape.
* URL Alternates: añade alternativas para todos los shapes basados en la URL actual, de la forma "algunshape-url-laurlactual" o "algunshape-url-homepage".
* Wdidget Alternates: añade alternativas para widgets y capas específicos.

### Ver También:

* [Personalizar Orchard usando las herramientas de ayuda al desarrollador][31]

El módulo DesignerTools está disponible desde los paquetes de código fuente o [desde la galería][32].

## Orchard.Email

Este módulo implementa un canal de envío de mensajes por e-mail que puede ser usado para enviar notificaciones de correo desde las normas (rules).

El módulo Email está disponible desde los paquetes de código fuente o [desde la galería][38].

## Orchard.Fields (WebPI)

Orchard.Fields proporciona campos de input, booleano, fecha, numérico, enlace, enumeración y selector de medios.

## Orchard.Forms (WebPI)

Este módulo destinado a desarrolladores proporciona shapes que son útiles para construir formularios dinámicamente desde código.

Los módulos Projector y Rules dependen de Orchard.Forms

## Orchard.ImportExport

Este módulo permite exportar e importar la definición de tipos de contenido, así como también el contenido en sí mismo entre dos instancias de Orchard.
El formato que es usado para transferir la información es el mismo formato XML que es usado en los recipes.

El módulo ImportExport está disponible desde los paquetes de código fuente o [desde la galería][33].

## Orchard.Indexing, Orchard.Search and Lucene

Estos tres módulos forman la infraestructura por defecto para las búsquedas de full-text en Orchard.
El módulo Indexing agrega información al índice de búsquedas extrayéndola de los ítems de contenido.
El módulo Lucene proporciona la implementación del índice de búsqueda que usa el módulo Indexing y que realiza consultas de búsqueda.
El módulo Search busca en el índice y da formato a los resultados.

Estos módulos están disponibles desde los paquetes de código fuente o desde la galería: [Search][34],
[Indexing][35] y [Lucene][36].

## Orchard.jQuery (WebPI)

Este módulo proporciona los scripts de jQuery y de jQueryUI. Otros módulos lo usan como dependencia.


## Orchard.Lists (desfasado)

Este módulo sirve una implementación sencilla para las listas de ítems de contenido, siguiendo una relación carpeta/archivo donde un ítem de contenido solo puede pertenecer a una lista.

Este módulo está desfasado y recomendatos a los usuarios cambiarlo por Taxonomies y Projections, que permiten  una mayor amplitud de escenarios y usos.

## Orchard.Localization

Este módulo proporciona un part que puede ser añadido a un tipo de contenido para hacerlo localizable.
Los ítems de los tipos podificados pueden tener varias versiones que difieron por la cultura.

Este módulo está disponible desde los paquetes de código fuente o desde la [galería][37].

## Orchard.Media (WebPI)

Orchard.Media administra el contenido de la carpeta /media.

### Ver También

* [Añadir y administrar contenido multimedia][10]

## Orchard.MediaPicker (WebPI)

MediaPicker añade el selector de medios al editor del content body.

## Orchard.Messaging

This module provides the messaging infrastructure that module such as Rules can
use. It is only useful as a dependency for other modules that implement
specific channels, such as e-mail or Twitter.

This module is available from source code packages or [from the gallery][39].

## Orchard.Modules (WebPI)

This is the module that provides the admin UI to enable and disable Características.

### Ver También

* [Installing and upgrading modules][11]

## Orchard.MultiTenancy

Hosting multiple Orchard sites on separate applications means duplicating everything
for each site. The multi-tenancy module enables the hosting of multiple Orchard sites
within a single IIS application, thus saving a lot of resources, and reducing maintenance
costs. Each site's data is strictly segregated from the others through a table prefix
or complete database separation.

### Ver También

* [Setting up a multi-tenant Orchard site][40]

This module is available from source code packages or [from the gallery][41].

## Orchard.Packaging (WebPI)

This module handles the packaging of themes and modules.

### Características

* Packaging commands: core services and command-line commands to package and install modules.
* Gallery: integrates the [gallery][1] into Orchard.
* Package Updates: enables module updates from the admin dashboard.

### Ver También

* [Installing modules and themes from the gallery][12]

## Orchard.Pages (WebPI)

The Pages modules adds the Page content type, and associated commands.

### Ver También

* [Adding pages to your site][13]

## Orchard.Projections (WebPI)

This tremendously useful module enables the creation of arbitrary queries over
the contents of the site, and then to present the results in flexible layouts,
without leaving the admin dashboard.

### Ver También

* [Presentation video on Projections][14]

## Orchard.PublishLater (WebPI, desactivado por defecto)

The PublishLater part can be added to draftable content types and allows scheduled
publication of contents.

### Ver También

* [Saving, scheduling and publishing drafts][15]

## Orchard.Recipes (WebPI)

Recipes are XML files that describe a set of operations on the contents and configuration
of the site. Recipes are used at setup to describe predefined initial configurations
(Orchard comes with default, blog and core recipes). They can also be included with
modules to specify additional operations that get executed after installation.
Finally, the import/export feature uses this same recipe format to transfer contents.

### Ver También

* [Making a web site recipe][16]

## Orchard.Roles (WebPI)

Security APIs in Orchard do not make many presuppositions about authentication, membership
and permissions, but we do ship role-based security as a default security implementation.
Users can belong to one or many groups, and permissions are granted to groups rather than
users.

### Ver También

* [Managing users and roles][17]
* [Understanding permissions][18]

## Orchard.Rules (WebPI, desactivado por defecto)

Orchard events can be picked up by rules and trigger actions. For example, the publication
event on the comment content type can be picked-up by a user-defined rule and trigger
the action of sending an e-mail to the owner of the blog.
The Rules module provides simple admin UI to create and manage rules.

## Orchard.Scripting (WebPI)

In order to enable simple programmability of the application without requiring the
development of a whole module, certain key areas of Orchard expose extensibility through
scripting. For example, widget layer visibility is defined by rules that are written
as simple script expressions. The scripting infrastructure is language-agnostic, and
new languages could be added by a module. Orchard comes with one implementation that
is a simple expression language whose syntax is a subset of Ruby.

### Características

* Scripting: the scripting infrastructure.
* Lightweight Scripting: a simple expression language that is a subset of Ruby.
* Scripting Rules: makes it possible for rules to be triggered by arbitrary scripted expressions.

## Orchard.Scripting.DLR

This module, built on Orchard.Scripting, enables the possibility to use DLR languages
such as Ruby and Python as scripting languages.

## Orchard.Setup (WebPI, off after setup)

This module is always disabled except before the application has been setup. It is responsible
for implementing the setup mechanism. It contains the original recipes in its Recipes subfolder.

### Ver También

* [Installing Orchard][20]
* [Making a web site recipe][16]

## Orchard.Tags (WebPI)

Tags are a very simple way to categorize contents. It is a flat and easily extensible structure.
For more elaborate classifications, we strongly recommend the use of the [Contrib.Taxonomies][21]
module.

### Ver También

* [Organizing content with tags][22]

## Orchard.TaskLease

In web farm environments, it's often useful to send messages across all servers in the farm. This
module implements a way for code to communicate tasks to the whole server farm.

This module is available from source code packages or [from the gallery][42].

## Orchard.Themes (WebPI)

This module provides the infrastructure for easy customization of the look and feel of the site
through the definition of themes, which are a set of scripts, stylesheets and template overrides.

### Ver También

* [Installing themes][23]
* [Previewing and applying a theme][24]
* [Customizing the default theme][25]

## Orchard.Tokens (WebPI, desactivado por defecto)

Tokens are contextual environment variables that are used in dynamic expressions. For example,
the Autoroute feature makes it possible to define URL patterns for content items of a given
type. Those patterns rely on tokens that will be dynamically evaluated in a specific context.
The "{Content.Date.Format:yyyy}/{Content.Slug}" would be evaluated for the specific content item
it applies to and would be resolved to something like "2012/the-title".

## Orchard.Users (WebPI)

This is the module that implements the default user management in Orchard.

### Ver También

* [Managing users and roles][17]

## Orchard.Warmup (WebPI, desactivado por defecto)

Cold starts in ASP.NET applications can be slow, and shared hosting environments create the
conditions for frequent such cold starts. In order to mitigate this situation, the warmup
feature can prepare static versions of the most common pages of the site so those can be served
as soon as possible even if the application is not entirely done warming up.

## Orchard.Widgets (WebPI)

Widgets are reusable pieces of UI that can be positioned on any page of the site. Which widgets
get displayed on what pages is determined by layer rules.

### Características

* Widgets: the core widget feature and admin UI.
* Page Layer Hinting: adds a message when publishing a new page that prompts the user to create a new layer for that page.
* Widget Control Wrapper: Adds an edit button on the front-end for easier modification.

### Ver También

* [Managing widgets][26]
* [Writing a widget][27]

## TinyMCE (WebPI)

Used as a dependency by other Características, this provides the scripts necessary to implement the TinyMCE WYSYWYG HTML editor.

## UpgradeTo15 / UpgradeTo14 (WebPI, desactivado por defecto)

Orchard 1.4 brought breaking changes in the way URLs and titles are managed. 1.3 and previous versions
were using the Route part to handle a static URL and title. 1.4 deprecated this in favor of the new
alias, autoroute and title part. The upgrade module contains special scripts that upgrade old content
to the new way of doing things.
Orchard 1.4 also introduced new field types (see Orchard.Fields), and because some users may have
used equivalent Contrib.* fields from gallery modules, the upgrade module provides an upgrade path
to the new fields.

  [1]: http://gallery.orchardproject.net
  [2]: http://daringfireball.net/projects/markdown/
  [3]: http://www.google.com/recaptcha
  [4]: http://akismet.com/
  [5]: http://antispam.typepad.com/
  [6]: http://docs.orchardproject.net/Documentation/Adding-a-Blog-to-Your-Site
  [7]: http://docs.orchardproject.net/Documentation/Blogging-with-LiveWriter
  [8]: http://docs.orchardproject.net/Documentation/Moderating-comments
  [9]: http://docs.orchardproject.net/Documentation/Creating-custom-content-types
  [10]: http://docs.orchardproject.net/Documentation/Adding-and-managing-media-content
  [11]: http://docs.orchardproject.net/Documentation/Installing-and-upgrading-modules
  [12]: http://docs.orchardproject.net/Documentation/Installing-modules-and-themes-from-the-gallery
  [13]: http://docs.orchardproject.net/Documentation/Adding-Pages-to-Your-Site
  [14]: http://www.youtube.com/watch?feature=player_embedded&v=SP912eWoOoo
  [15]: http://docs.orchardproject.net/Documentation/Saving-scheduling-and-publishing-drafts
  [16]: http://docs.orchardproject.net/Documentation/Making-a-Web-Site-Recipe
  [17]: http://docs.orchardproject.net/Documentation/Managing-users-and-roles
  [18]: http://docs.orchardproject.net/Documentation/Understanding-permissions
  [20]: http://docs.orchardproject.net/Documentation/Installing-Orchard
  [21]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Contrib.Taxonomies
  [22]: http://docs.orchardproject.net/Documentation/Organizing-content-with-tags
  [23]: http://docs.orchardproject.net/Documentation/Installing-themes
  [24]: http://docs.orchardproject.net/Documentation/Previewing-and-applying-a-theme
  [25]: http://docs.orchardproject.net/Documentation/Customizing-the-default-theme
  [26]: http://docs.orchardproject.net/Documentation/Managing-widgets
  [27]: http://docs.orchardproject.net/Documentation/Writing-a-widget
  [28]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.ArchiveLater
  [29]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.CodeGeneration
  [30]: http://gallery.orchardproject.net
  [31]: http://docs.orchardproject.net/Documentation/Customizing-Orchard-using-Designer-Helper-Tools
  [32]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.DesignerTools
  [33]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.ImportExport
  [34]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.Search
  [35]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.Indexing
  [36]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Lucene
  [37]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.Localization
  [38]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.Email
  [39]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.Messaging
  [40]: http://docs.orchardproject.net/Documentation/Setting-up-a-multi-tenant-Orchard-site
  [41]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.MultiTenancy
  [42]: http://gallery.orchardproject.net/List/Modules/Orchard.Module.Orchard.TaskLease
