Un _shape_ (silueta o figura) es un modelo din�mico de datos
Pues pensar en los shapes como en contenedores de datos que son manipulados por plantillas para el renderizado.
La funci�n de un shape es reemplazar los modelos de vista est�ticos de ASP.NET MVC usando
un modelo que puede actualizarse en tiempo de ejecuci�n&#151;esto es, usando un shape din�mico.

Este art�culo introduce el concepto de shapes y explica c�mo trabajar con �l.
Est� pensado para desarrolladores de m�dulos y temas (theme) quienes poseen, al menos, un conocimiento b�sico de m�dulos de Orchard.
Para informaci�n sobre crear m�dulos, echa un vistazo a [Construyendo un m�dulo Hola Mundo](Building-a-hello-world-module).
Para informaci�n sobre objetos din�micos, mira [Creando y usando objetos din�micos, fuente MSDN Library](http://msdn.microsoft.com/en-us/library/ee461504.aspx).

> **Nota**`
`A lo largo del art�culo se usa indistintamente las palabras plantilla y tema para referirse al mismo concepto en ingl�s theme y nunca al concepto pattern.

# Introducci�n a Shapes

Los shapes son modelos de datos din�micos que usan plantillas de shape para hacer los datos visibles para su uso de la forma que quieras.
Las plantillas de shapes son fragmentos de etiquetado (markup) para renderizar shapes.
Algunos ejemplos de shapes incluyen men�s, items de men�, items de contenido, documentos y mensajes.

Un shape es un objeto modelo de datos que deriva de la clase `Orchard.DisplayManagement.Shapes.Shape`.
La clase `Shape` nunca es instanciada. En su lugar, los shapes son creados en tiempo de ejecuci�n por un shape factory.
La shape factory por defecto es `Orchard.DisplayManagement.Implementation.DefaultShapeFactory`.
Los shapes creados por la shape factory son objetos din�micos

> **Nota**`
`Los objetos din�micos son nuevos de .NET Framework 4.
Como objeto din�mico, un shape expone sus miembros en tiempo de ejecuci�n en lugar de tiempo de compilaci�n.
Esto contrasta con el objeto model de ASP.NET MVC que es un objeto est�tico definido en tiempo de compilaci�n.

La informaci�n sobre el shape se encuentra en la propiedad `ShapeMetadata` del propio shape.
Esta informaci�n inlucye el tipo de shape, la forma en que ser� mostrado, la posici�n, el prefix, los wrappers, alternates
el contenido de objetos hijo (child content) y un valor boleano `WasExecuted`.

Puedes acceder a los metadatos de un shape como se muestra en el siguiente ejemplo:
    
    var shapeType = shapeName.Metadata.Type;

Despu�s de que el objeto shape haya sido creado, el shape es renderizado con ayuda de una plantilla de shape (shape template).
Una plantilla de shape es una porci�n de etiquetado HTML (vista parcial) que es responsable de mostrar el shape.
Otra posibilidad es usar un atributo (`Orchard.DisplayManagement.ShapeAttribute`)
que te permite escribir c�digo que crea y muestra el shape sin usar una plantilla.

# Creando Shapes

Para los desarrolladores de m�dulos, la necesidad m�s com�n a la hora de desarrollar shapes es c�mo transportar los datos desde un driver a una plantilla para renderizarlo.
Un driver deriva de la clase `Orchard.ContentManagement.Drivers.ContentPartDriver` y normalmente sobreescribe (overrides) los m�todos `Display`y `Editor` de esa clase.
Los m�todos `Display` y `Editor` devuelven un objeto `ContentShapeResult`, el cual es an�logo al objeto `ActionResult` devuelvo por los action method de ASP.NET MVC.
El m�todo `ContentShape` te ayuda a crear el shape y devolverlo dentro de un objeto `ContentShapeResult`.

Aunque el m�todo `ContentShape` est� sobrecargado, el uso m�s com�n es pasarle dos
par�metros&#151;el tipo de shape y una expresi�n con una funci�n din�mica que define el shape.
El tipo de shape nombra el sape y une el shape a la plantilla que debe usarse para renderizarlo.
Las premisas para decidir los nombres (naming conventions or naming guidelines) son discutidas m�s adelante en "Decidiendo los nombres de Shapes y Plantillas"(Accessing-and-rendering-shapes#NamingShapesandTemplates).

La expresi�n de funci�n puede describirse mejor si usamos un ejemplo.
El siguiente ejemplo muestra el modo `Display` de un driver que devuelve un resultado shape (shape result),
el cual ser� usado para mostrar una parte `Map`.

    protected override DriverResult Display(
        MapPart part, string displayType, dynamic shapeHelper)
    {
    return ContentShape("Parts_Map",
                         () => shapeHelper.Parts_Map(
                               Longitude: part.Longitude, 
                               Latitude: part.Latitude));
    }

La expresi�n usa un objeto din�mico(`shapeHelper`) para definir un shape `Parts_Map`y sus atributos.
La expresi�n a�ade la propiedad `Longitude` al shape y establece su contenido igual a la propiedad `Latitude` del part.
La expresi�n tambi�n a�ade una propietad `Latitude` con el mismo valor que la propiedad `Latitude` del part.
El m�todo `ContentShape`crea un objeto resultado que es devuelto por el m�todo `Display`.

El pr�ximo ejemplo muestra una clase driver completa que env�a un shape result a una plantilla tanto para 
ser mostrada como editada en una parte `Map`. El m�todo `Display`es usado para mostrar el mapa.
El m�tido `Editor`marcado con "GET" es usado para mostrar el shape result en una vista de edici�n para recoger las entradas de datos del usuario.
El m�tido `Editor`marcado con "POST" es usado para actualizar la vista de edici�n usando los vatos proporcionados por el usuario.
Estos m�todos usan distintas sobrecargas del m�todo `Editor`.


    
    using Maps.Models;
    using Orchard.ContentManagement;
    using Orchard.ContentManagement.Drivers;
    
    namespace Maps.Drivers
    {
        public class MapPartDriver : ContentPartDriver<MapPart>
        {
            protected override DriverResult Display(
                MapPart part, string displayType, dynamic shapeHelper)
            {
                return ContentShape("Parts_Map",
                                    () => shapeHelper.Parts_Map(
                                          Longitude: part.Longitude, 
                                          Latitude: part.Latitude));
            }
    
            //GET
            protected override DriverResult Editor(
                MapPart part, dynamic shapeHelper)
            {
                return ContentShape("Parts_Map_Edit",
                                    () => shapeHelper.EditorTemplate(
                                          TemplateName: "Parts/Map", 
                                          Model: part));
            }
    
            //POST
            protected override DriverResult Editor(
                MapPart part, IUpdateModel updater, dynamic shapeHelper)
            {
                updater.TryUpdateModel(part, Prefix, null, null);
                return Editor(part, shapeHelper);
            }
        }
    }

El m�todo `Editor`marcado "GET" usa el m�todo `ContentShape` para crear un shape para una plantilla de edici�n.
En este caso, el nombre del tipo es `Parts_Map_Edit`  y el objeto `shapeHelper` crea un shape del tipo `EditorTemplate`.
Este es un shape especial que tiene una propiedad `TemplateName` y una propiedad `Model`.
La propiedad `TemplateName` almacena una ruta parcial a la plantilla.
Esta vez `"Parts/Map"` hace que Orchard busque una plantilla dentro de tu m�dulo que la siguiente ruta:

_Views/EditorTemplates/Parts/Map.cshtml_

La propiedad `Model` toma el nombre del archivo del modelo part, pero quitando la extensi�n de archivo.

# Decidiendo los nombres de shapes y de los temas (themes)

Como habr�s notado, el nombre dado a un tipo shape une el shape a la plantilla que ser� usada para renderizarlo.
Por ejemplo, supongamos que creas un part llamado `Map`y que este muestra un mapa para unas longitud y latitud dadas.
El nombre del tipo sape deber�a ser `Parts_Map. Por convenci�n, todos los shape part empiezan con `Parts_`seguidos del nombre del part (en este caso `Map`). Proporcionando este nombre (`Parts_Map`), Orchard busca el tema en tu m�dulo a trav�s de la siguiente ruta:

_views/parts/Map.cshtml_

La siguiente tabla resume las convenciones usadas para llamar a los tipo shape y los temas.

Aplicado a             | Convenci�n de nombrado de Shape                                          | Tipo Shape Example                                   | Tema Example
---------------------- | ----------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------
Content shapes         | Content\_\_\[ContentType\]                                        | Content\_\_BlogPost                                  | Content-BlogPost
Content shapes         | Content\_\_\[Id\]                                                 | Content\_\_42                                        | Content-42
Content shapes         | Content\_\_\[DisplayType\]                                        | Content\_\_Summary                                   | Content.Summary
Content shapes         | Content\_\[DisplayType\]\_\_\[ContentType\]                       | Content\_Summary\_\_BlogPost                         | Content-BlogPost.Summary
Content shapes         | Content\_\[DisplayType\]\_\_\[Id\]                                | Content\_Summary\_\_42                               | Content-42.Summary
Content.Edit shapes    | Content\_Edit\_\_\[DisplayType\]                                  | Content\_Edit\_\_Page                                | Content-Page.Edit
Content Part templates | \[ShapeType\]\_\_\[Id\]                                           | Parts\_Common\_Metadata\_\_42                        | Parts/Common.Metadata-42
Content Part templates | \[ShapeType\]\_\_\[ContentType\]                                  | Parts\_Common\_Metadata\_\_BlogPost                  | Parts/Common.Metadata-BlogPost
Field templates        | \[ShapeType\]\_\_\[FieldName\]                                    | Fields\_Common\_Text\_\_Teaser                       | Fields/Common.Text-Teaser
Field templates        | \[ShapeType\]\_\_\[PartName\]                                     | Fields\_Common\_Text\_\_TeaserPart                   | Fileds/Common.Text-TeaserPart
Field templates        | \[ShapeType\]\_\_\[ContentType\]\_\_\[PartName\]                  | Fields\_Common\_Text\_\_Blog\_\_TeaserPart           | Fields/Common.Text-Blog-TeaserPart
Field templates        | \[ShapeType\]\_\_\[PartName\]\_\_\[FieldName\]                    | Fields\_Common\_Text\_\_TeaserPart\_\_Teaser         | Fields/Common.Text-TeaserPart-Teaser
Field templates        | \[ShapeType\]\_\_\[ContentType\]\_\_\[FieldName\]                 | Fields\_Common\_Text\_\_Blog\_\_Teaser               | Fields/Common.Text-Blog-Teaser
Field templates        | \[ShapeType\]\_\_\[ContentType\]\_\_\[PartName\]\_\_\[FieldName\] | Fields\_Common\_Text\_\_Blog\_\_TeaserPart\_\_Teaser | Fields/Common.Text-Blog-TeaserPart-Teaser
LocalMenu              | LocalMenu\_\_\[MenuName\]                                         | LocalMenu\_\_main                                    | LocalMenu-main
LocalMenuItem          | LocalMenuItem\_\_\[MenuName\]                                     | LocalMenuItem\_\_main                                | LocalMenuItem-main
Menu                   | Menu\_\_\[MenuName\]                                              | Menu\_\_main                                         | Menu-main
MenuItem               | MenuItem\_\_\[MenuName\]                                          | MenuItem\_\_main                                     | MenuItem-main
Resource               | Resource\_\_\[FileName\]                                          | Resource\_\_flower\.gif                              | Resource-flower.gif
Style                  | Style\_\_\[FileName\]                                             | Style\_\_site\.css                                   | Style-site.css
Widget                 | Widget\_\_\[ContentType\]                                         | Widget\_\_HtmlWidget                                 | Widget-HtmlWidget
Widget                 | Widget\_\_\[ZoneName\]                                            | Widget\_\_AsideSecond                                | Widget-AsideSecond
Zone                   | Zone\_\_\[ZoneName\]                                              | Zone\_\_AsideSecond                                  | Zone-AsideSecond

Puedes poner tus temas en el proyecto de acuerdo a las siguientes reglas:

El contenido de cada item en las plantillas de shapes deben estar en el directorio _views/items_
* Los temas de los shape `Parts_` deben estar en _views/parts_ .
* Los temas de los shape `Fields_` deben estar en  _views/fields_ .
* Las plantillas del shape `EditorTemplate` deben estar en _views/EditorTemplates/_`template`+nombre .
Por ejemplo, un `EditorTemplate` con el nombre de tema _Parts/Routable.RoutePart_  tiene su tema en 
_views/EditorTemplates/Parts/Routable.RoutePart.cshtml_.
* Por �ltimo, todos los dem�s temas de shapes deben estar en la carpeta _views_ .

> **Nota**`  `La extensi�n de temas puede ser cualquier extensi�n soportada por cualquier view engine activo, como son _.cshtml_,_.vbhtml_ o _.ascx_.

## Desde nombre de archivo de un Tema hacia el nombre de un Shape

Las reglas para mapear desde el nombre de archivo de un tema a su correspondiente nombre de shape son las siguientes:

* El Punto (.) y la barra invertida han de cambiarse por el gui�n bajo (_).
Ten en cuenta que esto no significa que un archivo _example.cshtml_ en un subdirectorio _myviews_ del directorio _Views
sea equivalente a un archivo _myviews_example.cshtml en la carpeta _Views_.
El tema de shape deber�a estar en el directorio esperado (consulta las reglas anteriores)
* El gui�n (-) cambia al doble gui�n bajo (\_\_)

Por ejemplo, _Views/Hello.World.cshtml_ ser� usado para renderizar un shape llamado `Hello_World`,
y _Views/Hello.World-85.cshtml_ ser� usado para renderizar un shape llamado `Hello_World__85`.

## Otra alternativa para renderizar Shapes

Como te habr�s dado cuenta, un widget HTML en la zona `AsideSecond` (por ejemplo) podr�a renderizarse 
por un tema _widget.cshtml_, por un tema _widget-htmlwidget.cshtml_ o por otro _widget-asidesecond.cshtml_
si este existiera en el tema actual.
Cuando existen varias posibilidades para renderizar el mismo contenido,
esto hace referencia a las _alternates_ de un shape,
lo cual permite una gran variedad de nuevos escenarios para sobreescribir temas.

Las alternativas forman un grupo que corresponde al mismo shape si difieren s�lo por un doble gui�n bajo al final.
Por ejemplo, `Hello_World`, `Hello_World__85`, y `Hello_World__DarkBlue` son un grupo de alternativas para 
un shape del tipo `Hello_World`. `Hello_World_Summary`, sin embargo, no pertenece a este grupo, y podr�a corresponder a un shape `Hello_World_Shape`, no a un shape `Hello_World`.

## �Qu� alternativa deber�a ser renderizada?

Aunque no tenga alternativas, un shape siempre es creado con su nombre base, por ejemplo `Hello_World`.
Las anternativas ofrecen al desarrollador de temas opciones de nombres adicionales para sus temas m�s all�
del nombre por defecto como en el ejemplo: _hello.world.cshtml_.
El sistema escoger� entre las alternativas las plantillas m�s especializadas, de esta manera 
si _hello.world-orange.cshtml_ existe tendr� preferencia sobre _hello.world.cshtml_.

## Items de contenido de alternativas incorporados

La tabla superior muestra los nombres posibles de temas para los items de contenido.
Ahora deber�a estar claro que el nombre de un shape es construido a partir de `Content`
y el tipo para mostrar el shape (por ejemplo `Content_Summary`).

El sistema tambi�n a�ade autom�ticamente el tipo de contenido y el ID del contenido as� como las alternativas
(por ejemplo `Content_Summary__Page` y `Content_Summary__42`).

Para m�s informaci�n sobre el uso de las alternativas, mira [Alternates](Alternates).

# Renderizar Shapes usando temas

A shape template is a fragment of markup that is used to render the shape.
The default view engine in Orchard is the Razor view engine.
Therefore, shape templates use Razor syntax by default.
For an introduction to Razor syntax, see [Template File Syntax Guide](Template-file-syntax-guide).
Un tema de sepa es un fragmento de c�digo etiquetado que es usado para renderizar el shape.
El view engine por defecto en Orchard es Razor.
De la misma manera, los temas de shapes usan la sintaxis de Razor por defecto.
Para una introducci�n a la sintaxis de Razor echa un vistazo a [Template File Syntax Guide](Template-file-syntax-guide).

El siguiente ejemplo nos muestra a un tema para renderizar una part `Map` como una imagen.

    <img alt="Location" border="1" src="http://maps.google.com/maps/api/staticmap? 
         &zoom=14
         &size=256x256
         &maptype=satellite&markers=color:blue|@Model.Latitude,@Model.Longitude
         &sensor=false" />

Este ejemplo muestra un elemento `img` en el cual el atributo `src` contiene una URL
y un conjunto de par�metros pasados como valores querystring.
En este querystring, `@Model` representa al shape que se pas� junto al tema.
De la misma manera, `@Model.Latitude` es la propiedad  `Latitude` del shape, y
 `Latitude` es la propiedad `Longitude` del shape.

El pr�ximo ejemplo muestra la plantilla para el editor.
Esta plantilla permite al usuario introducir los valores de la latidud y la longitud.
    
    @model Maps.Models.MapPart

    <fieldset>
        <legend>Map Fields</legend>
                
        <div class="editor-label">
            @Html.LabelFor(model => model.Longitude)
        </div>
        <div class="editor-field">
            @Html.TextBoxFor(model => model.Latitude)
            @Html.ValidationMessageFor(model => model.Latitude)
        </div>
                
        <div class="editor-label">
            @Html.LabelFor(model => model.Longitude)
        </div>
        <div class="editor-field">
            @Html.TextBoxFor(model => model.Longitude)
            @Html.ValidationMessageFor(model => model.Longitude)
        </div>
    </fieldset>


La expresi�n `@Html.LabelFor` crea elementos label usando el nombre de las propiedades del shape.
La expresi�n `@Html.TextBoxFor` crea elementos de caja de texto, los input type="text", donde los usuario introducen los valores para las propiedades del shape.
La expresi�n `@Html.ValidationMessageFor` crea mensajes formateados que se muestran cuando el usuario ha introducido valores incorrectos.

## Wrappers

Los Wrappers te permiten personalizar la renderizaci�n de un shape a�adiendo etiquetado alrededor del shape.
Por ejemplo, _Document.cshtml_ es un wrapper para el shape `Layout`, porque este especifica
el c�digo de marcado que envuelve al shape `Layout`.
Para m�s informaci�n sobre las relaciones entre `Document`y `Layout`,
echa un vistazo a [Gu�a de sintaxis del archivo de tema](Template-file-syntax-guide).

Normalmente, a�ades un archivo wrapper a la carpeta _Views_ de tu theme.
Por ejemplo, para a�adir un wrapeer para `Widget`, a�adir�s un archivo _Widget.Wrapper.cshtml_
a la carpeta _Views_ de tu tema.
Si activas la funcionalidad **Shape Tracing**, ver�s los nombres disponibles para los wrapper de un shape.
Puedes tambi�n especificar un wrapper en el archivo _placement.info_.
Para m�s informaci�n de c�mo especificar un wrapper,
mira [Comprendiendo el archivo placement.info](Understanding-placement-info).


# Creando un m�todo de Shape

Otro modo de crear y renderizar un shape es crear un m�todo que define y renderiza al mismo tiempo un shape.
El m�todo debe marcarse con el atributo `Shape` (la clase `Orchard.DisplayManagement.ShapeAttribute`).
El m�todo devuelve un objeto `IHtmlString` en lugar de usar un tema.
El objeto devuelvo contiene el etiquetado que renderiza el shape.

El siguiente ejemplo muestra el shape `DateTimeRelative`.
Este shape toma un valor `DateTime` en el pasado y devuelve un string que relaciona el valor con el tiempo actual. 
    
    public class DateTimeShapes : IDependency {
        private readonly IClock _clock;
    
        public DateTimeShapes(IClock clock) {
            _clock = clock;
            T = NullLocalizer.Instance;
        }
    
        public Localizer T { get; set; }
    
        [Shape]
        public IHtmlString DateTimeRelative(HtmlHelper Html, DateTime dateTimeUtc) {
            var time = _clock.UtcNow - dateTimeUtc;
    
            if (time.TotalDays > 7)
                return Html.DateTime(dateTimeUtc, T("'on' MMM d yyyy 'at' h:mm tt"));
            if (time.TotalHours > 24)
                return T.Plural("1 day ago", "{0} days ago", time.Days);
            if (time.TotalMinutes > 60)
                return T.Plural("1 hour ago", "{0} hours ago", time.Hours);
            if (time.TotalSeconds > 60)
                return T.Plural("1 minute ago", "{0} minutes ago", time.Minutes);
            if (time.TotalSeconds > 10)
                return T.Plural("1 second ago", "{0} seconds ago", time.Seconds);
    
            return T("a moment ago");
        }
    }
