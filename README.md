# SafeNotes - Documentación Profesional

📌 **Índice**
- [Descripción](#descripción)
- [Características Clave](#características-clave)
- [Requisitos Técnicos](#requisitos-técnicos)
- [Configuración Inicial](#configuración-inicial)
- [Guía de Uso](#guía-de-uso)
- [Arquitectura de Seguridad](#arquitectura-de-seguridad)
- [Despliegue en Producción](#despliegue-en-producción)
- [Contribución y Reporte de Vulnerabilidades](#contribución-y-reporte-de-vulnerabilidades)
- [Licencia](#licencia)

## 📝 Descripción

SafeNotes es una aplicación móvil de notas cifradas de extremo a extremo (E2EE) desarrollada en Flutter, diseñada para garantizar la máxima seguridad y privacidad de los usuarios. Utiliza Firebase Auth para autenticación, AES-256 para cifrado local y Firestore con reglas de seguridad estrictas.

🔐 Cumple con OWASP Mobile Top 10 para prevenir vulnerabilidades comunes.

## ✨ Características Clave

- ✔ Autenticación segura con Firebase (correo/contraseña + opción biométrica).
- ✔ Cifrado AES-256 en el dispositivo antes de almacenar en la nube.
- ✔ Detección de root/jailbreak para bloquear dispositivos comprometidos.
- ✔ Validación de inputs para prevenir inyecciones.
- ✔ Reglas de Firestore que restringen acceso solo al dueño de las notas.
- ✔ Ofuscación de código en builds de producción.

## ⚙️ Requisitos Técnicos

- Flutter SDK ≥ 3.16.0
- Dart ≥ 3.2.0
- Firebase Project (configurado para Android/iOS)

**Dispositivos compatibles:**
- Android 8.0+ (API 26+)
- iOS 12+

**No soporta dispositivos rooteados/jailbreakeados**

## 🚀 Configuración Inicial

1. **Clonar el repositorio**
    ```bash
    git clone https://github.com/tu-usuario/safenotes.git
    cd safenotes
    ```

2. **Instalar dependencias**
    ```bash
    flutter pub get
    ```

3. **Configurar Firebase**

    Crea un proyecto en Firebase Console.

    Descarga los archivos de configuración:
    - `google-services.json` (Android)
    - `GoogleService-Info.plist` (iOS)

    Colócalos en:
    - `android/app/google-services.json`
    - `ios/Runner/GoogleService-Info.plist`

4. **Ejecutar la app**
    ```bash
    flutter run --release  # Modo producción (ofuscado)
    ```

## 📲 Guía de Uso

1. **Registro e Inicio de Sesión**
    - Ingresa un email válido y una contraseña segura (mínimo 8 caracteres).
    - La app verificará automáticamente si el dispositivo es seguro.

2. **Crear una Nota**
    - Haz clic en "Nueva Nota".
    - Escribe tu contenido.
    - Al guardar, se cifrará automáticamente antes de enviarse a Firestore.

3. **Ver Notas Cifradas**
    - Las notas solo se descifran localmente al abrirlas.
    - No se almacena texto plano en ningún momento.

4. **Cerrar Sesión**
    - Los datos locales se borran al cerrar sesión (excepto credenciales en Keychain/Keystore).

## 🛡️ Arquitectura de Seguridad

### Flujo de Cifrado
![Diagrama de flujo de cifrado](Diagrama)

### Medidas OWASP Implementadas

| **OWASP Top 10**                        | **Implementación en SafeNotes**                      |
|----------------------------------------|-----------------------------------------------------|
| M1 (Uso incorrecto de plataforma)      | Firebase Auth + Permisos mínimos                    |
| M2 (Almacenamiento inseguro)           | Cifrado AES-256 + Secure Storage                    |
| M3 (Comunicación insegura)             | HTTPS + Certificate Pinning (Firebase)             |
| M4 (Autenticación insegura)            | Validación frontend/backend + Límite de intentos    |
| M5 (Criptografía débil)                | Claves AES-256 + IV único por nota                  |

## 🔧 Despliegue en Producción

1. **Generar APK Ofuscado**
    ```bash
    flutter build apk --obfuscate --split-debug-info=./debug_info
    ```

2. **Configurar Firebase App Check**
    ```dart
    // En main.dart
    await FirebaseAppCheck.instance.activate(
      androidProvider: AndroidProvider.playIntegrity,
      appleProvider: AppleProvider.appAttest,
    );
    ```

3. **Monitorear Logs Seguros**
    Usa `debugPrint` en lugar de `print` para evitar leaks.

    Ejemplo:
    ```dart
    debugPrint('Usuario autenticado: ${user.email?.obfuscate()}');
    ```

## 🤝 Contribución y Reporte de Vulnerabilidades

Si encuentras un problema de seguridad, repórtalo de manera responsable:

- Abre un issue en GitHub con la etiqueta `[SECURITY]`.
- No lo divulgues públicamente hasta que se parchee.

## 📜 Licencia

Este proyecto está bajo MIT License.

## 🔗 Enlaces Útiles

- [OWASP Mobile Top 10](https://owasp.org/www-project-top-ten/)
- [Firebase Flutter Docs](https://firebase.flutter.dev/docs/overview)
- [Flutter Secure Storage](https://pub.dev/packages/flutter_secure_storage)

## 🎯 ¿Listo para usar SafeNotes?

¡Descarga la app y mantén tus notas seguras! 🚀

## 📸 Capturas de Pantalla

- **Login Seguro**
- **Lista de Notas Cifradas**
