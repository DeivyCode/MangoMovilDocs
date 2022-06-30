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


### DI 💉