# Sound Co-Pilot Updates

Repositorio **publico** de actualizaciones de Sound Co-Pilot.
No contiene el codigo fuente completo del proyecto.

Hay **dos canales**, uno por plataforma, y no se mezclan:

```
updates.json                 <- PC     parches .scpup   etiquetas  v2.2.3
mobile/
  mobile_update.json         <- movil  APK completo     etiquetas  mobile-v1.0.0+1
  CHANGELOG_MOVIL.md
  README.md
```

## Por que es publico

El updater de la app lee su manifiesto y descarga el asset del release usando la
API de GitHub. Al ser publico no hace falta ninguna credencial: `read_token` va
vacio en `data/update_remote_config.json` y el instalador no distribuye ningun
token.

---

## Canal de PC — `updates.json`

- **`updates.json`** -- el manifiesto que la app consulta. `latest_version` es la
  version publicada y `from_version_hint` la version **exacta** desde la que se
  puede aplicar el parche.
- **Releases** -- cada uno trae un `.scpup`, que es un zip con
  `update_manifest.json` y una carpeta `payload/`.

### Sobre los parches delta

Un `.scpup` solo trae los archivos que cambiaron respecto de SU version de
origen. Por eso unicamente se puede aplicar sobre esa version exacta: sobre otra
base deja la instalacion mezclada (una parte nueva, el resto vieja). Para saltar
desde una version anterior hay que usar el instalador completo.

Desde la v2.1.2 la app comprueba esto sola: si `from_version_hint` no coincide con
lo instalado, ni siquiera ofrece el parche y avisa que corresponde el instalador
completo.

---

## Canal del movil — `mobile/`

Sound Co-Pilot **Lite** (Android) se actualiza de otra forma, porque en Android
no existen los parches: el codigo va compilado dentro de `libapp.so`, el APK va
firmado, y el sistema no ejecuta nada que no venga dentro de el. Aqui el
artefacto es **siempre un APK entero**.

El movil lleva **su propia serie de versiones, desde 1.0.0**. Hasta el
2026-09-19 copiaba el numero de la PC (2.2.4); se separo al abrir este canal,
porque dos programas que se publican en fechas distintas no pueden compartir
numero sin mentir. La version de escritorio con la que se probo el enlace viaja
aparte, en `pairedPcVersion`.

El manifiesto del movil admite ademas un campo **`aviso`**: un recado que la app
muestra en Configuracion **sin necesidad de publicar APK**. Sirve para mandar
una instruccion, una advertencia o avisar de un Pack nuevo.

Detalle completo en [`mobile/README.md`](mobile/README.md).
