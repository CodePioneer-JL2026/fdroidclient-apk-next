# fdroidclient-apk-next

🇺🇸 [English (US)](README.en-US.md) ｜ 🇨🇳 [简体中文](README.zh-CN.md) ｜ 🇫🇷 [Français](README.fr-FR.md) ｜ 🇪🇸 **Español** ｜ 🇯🇵 [日本語](README.ja-JP.md) ｜ 🇷🇺 [Русский](README.ru-RU.md) ｜ 🇰🇷 [한국어](README.ko-KR.md)

F-Droid dejó de publicar los APK de su cliente en GitHub. Este repositorio mantiene abierta esa puerta.

Soy estudiante de secundaria en China y quiero decirlo con sinceridad: mis conocimientos técnicos aún son limitados. No puedo modificar el código de F-Droid ni añadir funciones, y hay muchas cosas del mundo del software libre que todavía me quedan grandes. Pero me encanta lo que representa el código abierto: la idea de que el software puede ser libre y transparente, y de que personas desconocidas pueden ayudarse y crear juntas, simplemente porque es lo correcto. Esa idea me animó a aportar algo, por pequeño que fuera.

Esta es la parte que puedo hacer: F-Droid está alojado en GitLab y ya no publica nuevas versiones en GitHub. Cuando no se puede acceder a la web oficial o a GitLab —algo que ocurre a menudo desde donde vivo—, la gente no tiene forma de conseguir la última versión del cliente. Si ni siquiera puedes instalar la tienda, nunca llegas a conocer las miles de aplicaciones FOSS que ofrece. Eso me parecía injusto, sobre todo para personas como yo: estudiantes, jóvenes curiosos y cualquiera que esté empezando a aprender. Así que este repositorio es mi respuesta. Aquí replico cada nueva versión oficial del cliente Android de F-Droid, tal y como F-Droid la compila y firma: sin modificaciones ni volver a empaquetarla, nunca. Piensa en ello como una puerta adicional: otro lugar donde encontrar el cliente cuando no se puede pasar por la entrada principal.

## Qué incluye cada versión

Cada vez que sale una nueva versión oficial del cliente, subo **dos archivos esenciales, siempre** y, junto a ellos, una clave oficial para quien quiera profundizar un poco más en la verificación:

- `F-Droid_<version>.apk` — el cliente oficial, sin modificar
- `F-Droid_<version>.apk.sha256.txt` — un archivo de texto sencillo con el **hash del archivo** APK correspondiente
- `f-droid.org-signing-key.gpg` — la clave pública GPG oficial, para comprobaciones avanzadas de la cadena de publicación (ver más abajo)

El `.sha256.txt` es solo un archivo de texto. Ábrelo en cualquier teléfono y verás una línea: el SHA-256 de esta compilación. Lo mantengo deliberadamente como `.txt` —sin trucos de extensiones extrañas— para que se abra en cualquier dispositivo y los principiantes puedan leerlo.

## Sobre el archivo `.sha256.txt` (lee esta parte, por favor)

**El hash que aparece dentro del `.txt` es el hash del propio archivo APK. NO es la huella digital del certificado de firma. Son dos cosas completamente distintas.**

- Cada compilación del APK es diferente, así que **el hash de este archivo cambia con cada versión.** Es normal: nueva versión, nuevo hash.
- Solo demuestra que la descarga no se dañó durante la transferencia. **No demuestra quién firmó el paquete.**
- Para comprobar que realmente es el paquete oficial de F-Droid, verifica el certificado por separado (ver la siguiente sección).

Comprobación rápida en la mayoría de los ordenadores:

```sh
sha256sum -c F-Droid_*.apk.sha256.txt
```

¿No tienes `sha256sum` en el teléfono? Abre el `.txt` y compara el hash que contiene con el que aparece en la página de la versión. Si coinciden, la descarga está intacta.

## Verifica también la firma (no te saltes este paso)

Hay tres niveles de verificación. Para las instalaciones cotidianas, basta con los dos primeros. El tercero es una clave de reserva para quienes quieran comprobarlo todo con más detalle.

| | Hash del archivo (el `.txt`) | Huella del certificado (**lo esencial en Android**) | Clave pública GPG (cadena de publicación, avanzado) |
|---|---|---|---|
| Qué demuestra | Que la descarga no está dañada | Que el paquete realmente es de F-Droid | Que la cadena de publicación oficial no ha sido manipulada |
| ¿Cambia? | **Con cada versión** | **Nunca** (válida hasta 2037) | **La huella no cambia** (válida hasta 2037; la renovación conserva la misma clave) |
| Cómo comprobarlo | `sha256sum -c` o a simple vista | `keytool -printcert -jarfile` | `gpg --verify` (necesita el archivo `.asc`) |
| Prioridad diaria | Útil | ⭐ **Lo esencial: compruébala en cada instalación** | Opcional, en un ordenador |

La **huella del certificado de Android** que debes buscar es **siempre** esta; es la que Android comprueba al instalar y actualizar:

```
43238d512c1e5eb2d6569f4a3afbf5523418b82e0a3ed1552770abb9a9c9ccab
```

(F-Droid documenta este certificado de firma de larga duración como válido hasta 2037.)

- **¿No coincide el hash del APK?** Es normal: cada versión tiene el suyo.
- **¿No coincide el certificado?** Detente. **No lo instales**, por muy correcto que parezca el nombre del archivo o por muy «bonito» que sea el hash.
- En Linux / macOS, ejecuta este comando:

  ```sh
  keytool -printcert -jarfile F-Droid_*.apk
  ```

  Debería mostrar el mismo valor del certificado. Android también rechazará cualquier actualización futura cuya firma no coincida, así que hacer bien la **primera instalación** mantiene seguras todas las posteriores.

### Avanzado: firma GPG de la publicación (opcional, pero conviene conocerla)

El archivo `f-droid.org-signing-key.gpg` incluido en este repositorio contiene la clave pública oficial. Es **una clave distinta del certificado de firma de Android** anterior: son dos claves separadas, no las confundas.

- **Certificado de Android** (columna central de arriba): el que Android comprueba al instalar. Evita la suplantación. Hasta 2037, su huella siempre es `43238d…ccab`.
- **Clave pública GPG** (esta sección): sirve para verificar desde un ordenador. Comprueba el archivo de firma oficial `.asc` que acompaña al APK y confirma que el paquete procede de la propia cadena de publicación de F-Droid.

**Cómo usar el archivo `.gpg`:**

1. Importa la clave una sola vez:

   ```sh
   gpg --import f-droid.org-signing-key.gpg
   ```

   Aquí tienes la huella de la clave principal oficial; compárala al importarla:

   ```
   37D2 C987 89D8 3119 4839  4E3E 41E7 044E 1DBA 2E89
   ```

   Misma huella = misma clave, solo renovada.

2. Verifica la firma de la publicación (necesitas el archivo `.asc` oficial que acompaña al APK):

   ```sh
   gpg --verify F-Droid_<version>.apk.asc F-Droid_<version>.apk
   ```

   El mensaje `Good signature from "F-Droid ..."` significa que la cadena de publicación está intacta.

**⏳ Aviso sobre el vencimiento de 2037:**

Esta clave GPG es válida hasta **2037**. Después, la verificación podría mostrar `key expired`. Es un **vencimiento normal, no una manipulación**: el equipo oficial renueva la clave y, si la huella no cambia, sigue siendo la misma clave.

> Para instalar la aplicación en el teléfono a diario, basta con la **huella del certificado de Android** anterior y un `.sha256.txt` coincidente: eso ya ofrece una protección muy sólida. GPG es una precaución adicional. La clave ya está aquí; estará cuando la necesites.

## Seguiré con esto

Este no es un repositorio para subir algo una sola vez, ni voy a desaparecer después de la primera versión. Seguiré mientras pueda, a pesar de las redes inestables y los días ajetreados de clase. No puedo prometer que siempre seré rápido, pero sí que seguiré aquí. ¿Sale una nueva versión oficial? Aquí estarán el APK, su `.sha256.txt` y la clave `.gpg`: los mismos archivos y las mismas reglas.

## Una aclaración sincera

**No estoy afiliado a F-Droid ni cuento con su respaldo.** Solo soy un joven que tuvo la suerte de descubrir el código abierto y quiere que más gente lo descubra también. Puede que sea joven y mis capacidades sean limitadas, pero este espejo contiene todo el cuidado que puedo ofrecer. Si ayuda aunque sea a una persona a descubrir el software libre —a un joven que siente curiosidad o a alguien que aprende algo nuevo—, cada sincronización atascada y cada espera hasta altas horas de la noche habrá merecido la pena. Mi deseo es sencillo: ayudar a que algunas personas más crucen esa puerta. Si has llegado hasta aquí, quizá también puedas transmitir esa ayuda a tu manera. **Así es como crecemos todos.** 🤝