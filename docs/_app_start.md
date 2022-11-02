## Inicio de la Aplicación Mango Movil ✨

Toda aplicación flutter inicial como en la mayoría de las aplicaciones tiene un método `main` el cual lanza nuestra aplicación
este es el método main en mango Movil
``` dart
Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await di.init();
  await SharedPreferenceService.initPreferences();
  await initializeDateFormatting('es_ES');
  runApp(
    AppState(
      child: MyApp(),
    ),
  );
}
```

Inicializamos algunos servicios como las `SharedPreferences` este servicio nos permite usar pa cache del Movil para guardar información
y acceder a ella en cualquier punto de nuestra aplicación mas información sobre este paquete [aquí](https://pub.dev/packages/shared_preferences).

También hacemos la inicializamos el `di` o `Dependency Injector` en ingles este servio nos da la posibilidad de inyectar nuestras interfaces o clases abstractas en dart, en nuestros Bloc, Widgets, y Repositorios con la finalidad de tener un código flexible al momento de tener diferentes implementaciones
mas información sobre este paquete [aquí](https://pub.dev/packages/get_it).

luego inicializamos nuestro `dateformatting` el cual nos va a formatear nuestras fechas al español cuando usemos el `DateFormat`, y finalmente tenemos el 
`runApp()` este método básicamente inicia la app y el árbol de widgets poniendo en la raíz nuestro `MaterialApp` mas info sobre MaterialApp [aquí](https://api.flutter.dev/flutter/material/MaterialApp-class.html), a su vez puedes ver que también tenemos un `AppState` este widget es un wrapper para tener todos nuestros Blocs Globales, cada vez que vayas a crear un bloc que sea global, es decir que vaya a tener presencia en mas de una pantalla lo recomendable es que lo registres en este `AppState` mas info [aquí](app_state.md).

e.j del AppState
```dart
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



