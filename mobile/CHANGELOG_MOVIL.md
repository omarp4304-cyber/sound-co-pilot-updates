# Changelog - Sound Co-Pilot Lite (Android)

## 1.2.0 build 3 (2026-09-23)

Etiqueta: `mobile-v1.2.0+3` - emparejada con PC 2.2.4

- La app se actualiza sola: al abrirla avisa si hay version nueva y la descarga e instala con un boton.
- Nueve temas con vista previa: Oscuro, Claro, AMOLED y los seis de la app de PC.
- Ecualizador nuevo de 6 bandas, igual al de la PC, con grafica tactil y espectro en vivo.
- Motor de audio nuevo: el ecualizador ya no corta lo que pasa de 12 kHz y los graves se ajustan con precision.
- Ecualizador por stem: voz, bateria, bajo y otros por separado.
- DJ RAM ya distingue el sub de los graves.
- Arreglo: la app podia cerrarse sola al salir.

## 1.1.0 build 2 (2026-09-23)

Etiqueta: `mobile-v1.1.0+2` - emparejada con PC 2.2.4

- Nueve temas con vista previa: Oscuro, Claro, AMOLED y los seis de la app de PC, con opcion de seguir al telefono.
- Ecualizador nuevo de 6 bandas, igual al de la PC: grafica tactil, espectro en vivo, A/B, deshacer y presets.
- Motor de audio nuevo: el ecualizador ya no corta lo que pasa de 12 kHz y los graves se ajustan con precision.
- Ecualizador por stem: voz, bateria, bajo y otros por separado, sin desfasar los stems.
- DJ RAM mide mejor: ya distingue el sub de los graves.
- Arreglo: la app podia cerrarse sola al salir.

## 1.0.0 build 1 (2026-09-19)

Etiqueta: `mobile-v1.0.0+1` - emparejada con PC 2.2.4

Primera version con **numeracion propia**. Antes la app del telefono se
anunciaba como 2.2.4 porque copiaba el numero de la app de PC; al abrir este
canal se separaron, porque son dos programas que se publican por separado.

- Biblioteca en el almacenamiento privado de la app, con portadas y metadatos
  importados de la PC.
- Reproductor multipista sobre `flutter_soloud` con voice group: los cuatro
  stems arrancan y se reposicionan en el mismo instante. **Deriva medida: 0 ms**
  con WAV y FLAC.
- Mezcla por stem (volumen, mute, solo) y presets, que se guardan por cancion.
- La biblioteca, las playlists, la mezcla y la ultima posicion sobreviven a
  cerrar la app, reiniciar el telefono e instalar un APK nuevo encima. Solo
  desinstalar las borra.
- Salida de audio abierta con la frecuencia y el tamano de buffer **reales del
  aparato**, leidos por `AudioManager`, en vez de 44 100 / 2048 fijos.
- Interfaz adaptada de 288 a 640 dp y a escala de texto hasta 1.6, sin
  desbordes en las 11 configuraciones de pantalla probadas.
- Modo Ambiente: cliente remoto de DJ RAM corriendo en la PC.
- Sound Co-Pilot Device: monitor remoto del equipo.
- Puente manual desde la PC con `tools/pc_to_phone.py` (FLAC u Ogg).
- Arreglo: el ecualizador tumbaba el arranque. `_applyEqualizer()` llamaba
  `deactivate()` sobre un filtro que no estaba puesto, y en flutter_soloud
  3.5.4 eso lanza `SoLoudFilterNotFoundException` en vez de no hacer nada.
  Como corre al restaurar los ajustes, la app se quedaba en el splash con
  "No se pudo iniciar el motor de audio" en cualquier instalacion cuyo
  `settings.json` fuera anterior al ecualizador.
