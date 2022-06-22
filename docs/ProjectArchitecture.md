# Estructura de Carpeta

**La estructura que he usado para este proyecto es muy parecida a la de Mango pero con ciertas variaciones,**
**cabe destacar que estamos trabajando con el Gestor de estado BloC sinónimo de Business Logic Component para mas información sobre este gestor [aquí](https://bloclibrary.dev/#/gettingstarted)**
**las cuales se mostrarán  a continuación**.
> Ejemplo de la estructura
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

Se ha usado una estructura de 3 niveles las cuales son **bloc** **data** y **presentation**, cada una tiene dentro de si sub-carpetas
para mantener todo lo mas organizado posible ya que en flutter los archivos tienden a tener muchas lineas de codo y son imposible de debugger.

### Como saber en donde va cada archivo según su funcionalidad

> Aquí se explica como posicionar los archivos de manear correcta según sea la finalidad del mismo
#### bloc o cubit
Es donde irán todos los archivos que manejen alguna lógica de negocio, como por ejemplo en el caso de consumir una API y quiero manejar alguna lógica mientras espero la respuesta como poner un ActivityIndicator mientras espero o mostrar algún mensaje según sea la respuesta de la API, aquí en donde deberían ir