### AppState 🔮

Este AppState es un wrapper el cual nos ayuda a mantener el estado de nuestra aplicación es decir tener todos nuestros Blocs registrados en el context una vez que la aplicación inicie, esto nos da la oportunidad de tener usar diferentes blocs en diferentes pantallas sin el miedo de obtener un error de que nuestro bloc no se ha registrado en el context de nuestra aplicación.

e.j del AppState
```dart
import 'inyector_container.dart' as di;
class AppState extends StatelessWidget {
  final Widget child;
  const AppState({Key? key, required this.child}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MultiBlocProvider(providers: [      
      //BLOC CLIENTES
      BlocProvider<ClienteBloc>(
        create: (context) => ClienteBloc(
            //REPOSITORY DEPENDENCY
            cuentasXCobrarRepository: di.sl(),
            locationService: di.sl(),
            facturacionRepository: di.sl()),
      )],
      child: child);
```
En este preview del `AppState` aquí vemos como registramos el Bloc de clientes y a su vez le suministramos o le pasamos sus dependencia con el `di` o `Dependency Injection`, como se había explicado antes, este servicio nos suministra una instancia ya anteriormente creada cuando nuestra aplicación es lanzada, cuando hacemos la llamada al método `sl()` esto es sinónimo de `Service locator` este método lo que hace es que busca en su stack de instancias creadas una del tipo que le estamos pidiendo en este caso tenemos 3 dependencia con diferentes repositorios nuestro sl sabe que anteriormente ya había creado instancia de este tipo y se la pasa al constructor de `ClienteBloc`, y asi es como registramos un Bloc que puede ser usado de manera global en nuestra aplicación.


### Dependency Injection (DI) 💉

Anteriormente habías visto como se inicializaba en el método main con la siguiente sentencia
``` dart
await di.init();
```
con esta llamada al método `init()` se crean todas las dependencia que tenemos en nuestro proyecto de la siguiente forma, usando el paquete [getit](https://pub.dev/packages/get_it) inicialmente obtenemos una instancia del mismo 
```dart 
final sl = GetIt.instance;
 ```
 una vez que tenemos nuestra instancia de `GetIt`, creamos nuestro método init
 ```dart
 Future<void> init() async {
  //* Shared Preferences
  sl.registerLazySingletonAsync(() => SharedPreferences.getInstance());

  //* Data sources
  sl.registerLazySingleton(() => (RemoteStorageService()));
  sl.registerLazySingleton<LocalStorageService>(() => LocalStorageService());

  //* Location
  sl.registerLazySingleton<LocationService>(() => LocationService());
   //* Repositories
  sl.registerLazySingleton<IProductRepository>(() => ProductRepositoryImp(
      remoteStorageService: sl(), localStorageService: sl()));
....
 ```
como puedes observar aquí registramos nuestras instancias con el método `sl.registerLazySingletonAsync` este nos crea una instancia única en toda la aplicación, es decir un singleton de esta forma con simplemente llamar `sl()` nos devolverá una instancia del tipo que le estamos pidiendo, también puedes especificar el tipo de dato de la instancia `sl<type_instance>()` el cual lo hace mas legible.

en el registro del repositorio es diferente a los servicios que se habían registrado mas arriba, usa la interfaz `IProductRepository`
esto porque podemos tener multiples implementaciones de este repositorio y solo tendríamos que cambiarla en un solo lugar y nuestra aplicación seguiría funcionando igual, cuando pedimos una instancia al `service locator` o `sl` del repositorio de productos este busca el tipo de dato `IProductRepository` pero devuelve una instancia de `ProductRepositoryImp` que es su implementación.

e.j
```dart
final repository = sl<IProductRepository>();
```
esto nos devuelve una instancia de `ProductRepositoryImp` el cual implementa todos los métodos de la interfaz `IProductRepository`, asi si tuviéramos que cambiar la implementación de `IProductRepository` solo tenemos que ir a nuestro service locator y poner nuestra nueva implementación.

```dart
   //* Repositories
  sl.registerLazySingleton<IProductRepository>(() => NuevaImplementacion());
```
[Mas información sobre como trabaja `GetIt`](https://www.burkharts.net/apps/blog/one-to-find-them-all-how-to-use-service-locators-with-flutter/)