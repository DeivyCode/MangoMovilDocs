## Estructura de Carpeta

**La estructura que he usado para este proyecto es muy parecida a la de Mango pero con ciertas variaciones,**
**cabe destacar que estamos trabajando con el Gestor de estado BloC sinónimo de Business Logic Component para mas información sobre este gestor [aquí](https://bloclibrary.dev/#/gettingstarted) tome la decision de usar este gestor de estado porque brinda gran robustez al momento de crear aplicaciones escalables, y también la gran comunidad que tiene asi que por falta de documentación no estarás perdido.**

> He aquí un Ejemplo de como esta la estructura del proyecto
```
└── modules
    ├─ Facturacion(modulo de ejemplo)
    ├───├── bloc o cubit
        └── (aquí van los todos los blocs de Facturacion)   
    ├───├── data
        ├── models
        └── repositories        
    ├──├─── presentation   
       ├── pages
       └── widgets
```

Se ha usado una estructura de 3 niveles las cuales son `bloc` `data` y `presentation`, cada una tiene dentro de si sub-carpetas
para mantener todo lo mas organizado posible ya que en flutter los archivos tienden a tener muchas lineas de código y son imposible de debugger.

### Como saber en donde va cada archivo según su funcionalidad

?> Aquí se explica como posicionar los archivos de manear correcta según sea la finalidad del mismo

#### 🎯 bloc o cubit
Es donde irán todos los archivos que manejen alguna lógica de negocio, como por ejemplo en el caso de consumir una API y quiero manejar alguna lógica mientras espero la respuesta como poner un `ActivityIndicator` mientras espero o mostrar algún mensaje según sea la respuesta de la API.

### 📂 Data
#### Models
 Aquí es donde pondrás todos tus modelos según el modulo que te encuentres, por ejemplo en Facturacion podrás encontrar algunos de los modelos referente a la factura y al calculo de la misma como por ejemplo `mfactrx` `dfactrx` entre otras.
Los modelos no son mas que la representación de data en un objecto `Dart` por ejemplo cuando hacemos una llamada a una API esta nos retorna un **response** este trae consigo muchas informaciones pero lo que nos interesa es su body, en el se encuentra la data que hemos solicitado pero esta en un formato `JSON` de esta forma esta data no nos he de mucha ayuda asi que pasamos a hacer un mapeo de este json a un objecto `Dart` en la mayoría de los modelos encontraras algunos métodos para hacer esta deserialization como el `ToMap` y `FromJson`para mas información sobre como trabajar con json y deserialization [aquí](https://docs.flutter.dev/development/data-and-backend/json)

#### Repositories

Son nuestra capa de acceso a datos, aquí podrás encontrar 2 archivos uno que es una clase abstracta y otro que es su implementación por ejemplo en el modulo de Facturacion podrás ver `IFacturacionRepository` que es una clase abstracta la **I** es de Interfaz ya que en dart una clase abstracta son como las interfaces en C#, luego tendrás otro archivo `facturacion_repository` el cual  implementa la clase abstracta `IFacturacionRepository`, la finalidad de los repositorios es poder tener toda nuestra capa de acceso a los datos desacoplada de la capa de negocio.

### ✨ Presentation 
En esta capa podrás ver 2 sub-carpetas `pages` y `widgets` es muy sencillo en pages veras las pantallas principales y en widgets los componentes que componen dicha pantalla, también podrás ver un archivo `pages.dart` este archivo sirve para importas todas nuestras pages en una sola instrucción e.j 
```dart
export 'CalculosPedido.dart';
export 'ClienteCard.dart';
export 'ClientesList.dart';
export 'CustomAppbar.dart';
export 'DatePicker.dart';
export 'Descuentos_y_Cargos.dart';
export 'FacturaPage.dart';
export 'OrderDetailsPage.dart';
export 'ProductsCard.dart';
export 'products_search.dart';
export 'product_details.dart';
```
aquí estamos exportando todas nuestras pantallas y al momento de hacer el import solo tenemos que hacer lo siguiente
``` dart
import 'package:mango_movil/Modules/Facturacion/presentation/pages.dart';
```
en widgets es la misma idea solo que aquí van todos los componentes que una pantalla vista o como le quieras llamar.