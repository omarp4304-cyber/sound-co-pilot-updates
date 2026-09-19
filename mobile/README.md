# Canal de actualizaciones del movil

Esta carpeta es la **seccion del telefono** dentro del repositorio de
actualizaciones de Sound Co-Pilot. Convive con el canal de la PC sin mezclarse
con el:

```
sound-co-pilot-updates/
  updates.json                 <- PC     parches .scpup, etiquetas v2.2.3
  mobile/
    mobile_update.json         <- movil  APK,            etiquetas mobile-v1.0.0+1
    CHANGELOG_MOVIL.md
    README.md                  <- esto
```

## Por que dos canales y no uno

Un `.scpup` reemplaza archivos `.py` y `.html` que Python lee en cada arranque.
En Android el codigo va compilado a maquina dentro de `libapp.so`, el paquete
va firmado, y el sistema no ejecuta nada que no venga dentro del APK. Un parche
de la PC no se puede aplicar a un telefono ni al reves.

Si los dos canales compartieran manifiesto, cada usuario de telefono veria
avisos de versiones que no puede instalar. Por eso: mismo repositorio, dos
manifiestos.

## Versiones: el movil ya no copia el numero de la PC

Hasta el 2026-09-19 la app del telefono se anunciaba como **2.2.4**, el numero
de escritorio. Se abandono al abrir este canal: son dos programas que se
publican por separado, y compartir numero obliga a mentir en cuanto uno de los
dos saca version sin el otro.

El movil empieza su propia serie en **1.0.0**. La version de PC con la que se
probo el enlace viaja aparte, en `pairedPcVersion`, y es informativa.

| | PC | Movil |
|---|---|---|
| Manifiesto | `updates.json` | `mobile/mobile_update.json` |
| Etiqueta | `v2.2.4` | `mobile-v1.0.0+1` |
| Artefacto | `Sound-Co-Pilot-…​.scpup` | `sound-copilot-lite-1.0.0-build1.apk` |
| Serie | 2.x | 1.x, propia |

## El manifiesto

```json
{
  "version": "1.0.0",
  "mobileBuild": 1,
  "pairedPcVersion": "2.2.4",
  "apkUrl": "https://github.com/.../sound-copilot-lite-1.0.0-build1.apk",
  "apkSha256": "…",
  "releasedAt": "2026-09-19",
  "avisoTitulo": "…",
  "aviso": "…",
  "notes": ["…"]
}
```

- `version` + `mobileBuild` — lo que la app compara. Primero `version`; a
  igualdad, `mobileBuild`. **Sube `mobileBuild` en cada APK publicado.**
- `apkUrl` — descarga directa. Vacio mientras no haya binario publicado.
- `apkSha256` — para comprobar que lo descargado es lo que se publico.
- `aviso` / `avisoTitulo` — **recado suelto**. Es lo que permite mandar una
  actualizacion especial (una instruccion, una advertencia, avisar de un Pack
  nuevo) **sin publicar APK**. La app lo muestra en Configuracion, en cian,
  separado del aviso de version nueva.
- `notes` — lista de cambios de esta version.

## Como se publica

Desde la carpeta del proyecto del movil:

```bash
py tools/publicar_movil.py --ver                      # que hay publicado
py tools/publicar_movil.py --aviso "texto del recado" # solo un recado
py tools/publicar_movil.py --notas "lo que cambio"    # version nueva completa
```

El script saca la version de `lib/data/app_info.dart`, comprueba que
`pubspec.yaml` diga lo mismo, compila el APK de release, **se niega a publicar
si va firmado con la llave de depuracion**, crea el Release, sube el APK y
reescribe este manifiesto.

Detalle completo en `docs/ACTUALIZACIONES_MOVIL.md` del proyecto.

## Lo que la app NO hace

No instala el APK sola: muestra el enlace. Instalar desde la app exigiria el
permiso `REQUEST_INSTALL_PACKAGES`, que Google Play revisa aparte y que no
compensa para un pack beta cerrado.
