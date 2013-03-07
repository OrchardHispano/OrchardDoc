>Este artículo ha sido actualizado cuando se ha liberado Orchard 1.1

En Orchard, un _widget_ es un trozo de interfaz de usuario reutilizable que puede ser arbitrariamente colocada en las páginas de un sitio web. Ejemplos de widgets podrían ser una nube de etiquetas, un buscador, o un feed de Twitter. Un widget es un Content Type, lo que le permite reutilizar el código y la interfaz de usuario.

En este artículo se describe cómo escribir un widget creando primero un ContentPart y luego convertiéndola en un widget.

# Crando un Content Part
Para este ejemplo, usaremos el 'Map' creado en [creando un content part](Writing-a-content-part)
. Si no has creado el 'Map', hazlo.

# Convirtiendo un Part en un Widget
Para convertir un content part en un widget, debes actualizar la base de datos con la definición del widget. Para hacer esto añade un método 'UpdateFrom1' en el fichero _Migrations.cs_ del part.

El siguiente ejemplo muestra el fichero _Migrations.cs del 'Map' part con el método 'UpdateFrom1' añadido.
    
    using System.Data;
    using Maps.Models;
    using Orchard.ContentManagement.MetaData;
    using Orchard.Core.Contents.Extensions;
    using Orchard.Data.Migration;
    
    namespace Maps
    {
        public class Migrations : DataMigrationImpl
        {
            public int Create()
            {
                // Creating table MapRecord
                SchemaBuilder.CreateTable("MapRecord", table => table
                    .ContentPartRecord()
                    .Column("Latitude", DbType.Single)
                    .Column("Longitude", DbType.Single)
                );
    
                ContentDefinitionManager.AlterPartDefinition(typeof(MapPart).Name, cfg => cfg
                    .Attachable());
    
                return 1;
            }
    
            public int UpdateFrom1()
            {
                // Create a new widget content type with our map
                ContentDefinitionManager.AlterTypeDefinition("MapWidget", cfg => cfg
                    .WithPart("MapPart")
                    .WithPart("WidgetPart")
                    .WithPart("CommonPart")
                    .WithSetting("Stereotype", "Widget"));
    
                return 2;
            }
        }
    }
 

En este ejemplo, el método 'UpdateFrom1' crea 'MapWidget' combinando 'MapPart', 'WidgetPart' y 'CommonPart' y con las opción estereotipo establecido como widget. El método devuelvo dos, es el nuevo número de versión.

El part ha sido ahora transformado en un widget.

# Mostrando el widget

Después de crear el nuevo widget, abre el **Dashboard** de orchard y haz clic en **Widgets++. Puedes seleccionar la capa y zona en la que quieras que se muestre el widget. La siguiente imagen muestra la página **ManageWidgets**.

![](../Upload/screenshots_675/manage_widgets_675.png)

Para más información sobre cómo mostrar tu widget, mira [Administrando Widgets](Managing-widgets).

# Compartiendo tu widget
Para compartir el widget con otros o subirlo a la galería, primero necesitas empaquetar tu widget en un módulo, que es un fichero _.zip. Se hace de la misma forma que empaquetar un módulo. Para más información, mira [Empaquetando y compartiendo un módulo](Packaging-and-sharing-a-module).
