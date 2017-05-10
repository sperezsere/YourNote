# Your Note
Creación de una APP que permite ver y crear notas desde la API de Evernote.

Lo primero a realizar es obtener nuestras API keys para poder utilizar la API de evernote https://dev.evernote.com/
Cuando se crea la API key por primera vez cabe recordar que estamos en modo **pre-produccion** por lo que las notas creadas solo se podran crear a los usuarios de prueba registrados en el sandBox de Evernote https://sandbox.evernote.com/. Para que se puedan crear las notas en la cuenta real hay que ponerse en contacto con https://dev.evernote.com/support/ y decirles que nos la activen a modo produccion, **cabe recordar que esta APP esta en modo PRODUCCION**

Una vez hecho esto añadimos la libreria de evernote a nuestro proyecto
```
dependencies {
    compile 'com.evernote:android-sdk:2.0.0-RC4'
}
```

Añadimos las claves publicas y privadas a gradle.properties
```
EVERNOTE_CONSUMER_KEY= Your consumer key
EVERNOTE_CONSUMER_SECRET= Your private key
```
### App.java

Creamos una clase que será de tipo Aplication, dentro de la clase creamos las variables de nuestras claves de acceso y el sandbox que nos permitira hacer login, si lo queremos en modo preproduccion tenemos que indicarlo en el **EVERNOTE_SERVICE** que seria **EvernoteSession.EvernoteService.SANDBOX**, yo lo voy a dejar en modo produccion.
```
private static final String CONSUMER_KEY = "Your consumer key";
private static final String CONSUMER_SECRET = "Your private key";
private static final EvernoteSession.EvernoteService EVERNOTE_SERVICE = EvernoteSession.EvernoteService.PRODUCTION;
```
Ademas en el metodo onCreate() tendremos que inicializar la sesion
```
new EvernoteSession.Builder(this)
                .setEvernoteService(EVERNOTE_SERVICE)
                .setSupportAppLinkedNotebooks(SUPPORT_APP_LINKED_NOTEBOOKS)
                .setForceAuthenticationInThirdPartyApp(true)
                .build(consumerKey, consumerSecret)
                .asSingleton();
```
### ComprobarLogin.java

En esta clase lo que vamos a hacer es comprobar el login, si se ha iniciado sesion anteriormente o no, para ello creamos el añadimos el siguiente metodo.

```
if (!EvernoteSession.getInstance().isLoggedIn() && !isIgnored(activity)) {
            mCachedIntent = activity.getIntent();
            LoginActivity.launch(activity);

            activity.finish();
        }
 ```
