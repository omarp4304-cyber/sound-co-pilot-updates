# Sound Co-Pilot Updates

Repositorio **publico** de actualizaciones (`.scpup`) para Sound Co-Pilot APP.
No contiene el codigo fuente completo del proyecto: solo parches delta versionados.

## Por que es publico

El updater de la app lee `updates.json` y descarga el asset del release usando la
API de GitHub. Al ser publico no hace falta ninguna credencial: `read_token` va
vacio en `data/update_remote_config.json` y el instalador no distribuye ningun
token.

## Contenido

- **`updates.json`** -- el manifiesto que la app consulta. `latest_version` es la
  version publicada y `from_version_hint` la version **exacta** desde la que se
  puede aplicar el parche.
- **Releases** -- cada uno trae un `.scpup`, que es un zip con
  `update_manifest.json` y una carpeta `payload/`.

## Sobre los parches delta

Un `.scpup` solo trae los archivos que cambiaron respecto de SU version de
origen. Por eso unicamente se puede aplicar sobre esa version exacta: sobre otra
base deja la instalacion mezclada (una parte nueva, el resto vieja). Para saltar
desde una version anterior hay que usar el instalador completo.

Desde la v2.1.2 la app comprueba esto sola: si `from_version_hint` no coincide con
lo instalado, ni siquiera ofrece el parche y avisa que corresponde el instalador
completo.
