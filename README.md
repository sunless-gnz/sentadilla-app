# App móvil Sentadilla IMU

Aplicación web instalable (PWA) que se conecta por Bluetooth Low Energy al ESP32 del proyecto.
Funciona con el firmware `Sentadilla_IMU` tal como está: usa el mismo servicio, las mismas
características y los mismos mensajes de texto.

## Compatibilidad

| Teléfono | Cómo usarla |
| --- | --- |
| Android | Chrome (versión reciente). Funciona el Bluetooth y se puede instalar como app |
| iPhone | Safari no permite Bluetooth web. Use el navegador gratuito **Bluefy** |
| Computador | Chrome o Edge en Windows, macOS o Linux con Bluetooth |

El Bluetooth web solo funciona en páginas servidas por **https** (o `localhost`), por eso hay que publicarla.

## Publicarla gratis con GitHub Pages (5 minutos)

1. Cree una cuenta en github.com y un repositorio nuevo, por ejemplo `sentadilla-app` (público).
2. En el repositorio: **Add file → Upload files** y suba los 6 archivos de esta carpeta
   (`index.html`, `manifest.webmanifest`, `sw.js`, `icono-192.png`, `icono-512.png`, `README.md`).
3. **Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save**.
4. Al minuto la app queda en `https://SU-USUARIO.github.io/sentadilla-app/`.
5. Ábrala en Chrome del teléfono → menú ⋮ → **Instalar aplicación** (o *Agregar a pantalla principal*).

Para probar en el computador sin publicar: en esta carpeta ejecute `python -m http.server 8000`
y abra `http://localhost:8000` en Chrome.

## Uso

1. Encienda el ESP32. En la app pulse **Conectar por Bluetooth** y elija `Sentadilla-IMU`.
2. En **Perfil** ingrese las medidas articulación a articulación, la dorsiflexión del tobillo,
   la profundidad objetivo y la carga. Pulse **Enviar límites al dispositivo**.
3. Pulse **Iniciar sesión**, quédese de pie y quieto 2 s (calibración) y empiece.
4. En **Repeticiones** cada repetición aparece separada, con sus parámetros, el rango esperado
   para usted y lo que quedó fuera de rango. En **Historial** puede exportar cada sesión a CSV.

Sin el dispositivo, **Probar sin dispositivo** simula el ESP32 con el mismo protocolo.

## Modelo personalizado

Modelo de equilibrio en el plano sagital con tres segmentos (tibia, fémur y tronco con cabeza y
brazos), fracciones de masa y centros de masa de Winter (2009) y estimación de longitudes desde
la estatura de Drillis y Contini. Para cada inclinación del muslo calcula la inclinación del tronco
que deja el centro de masa (cuerpo + carga) sobre el pie, teniendo en cuenta la dorsiflexión
máxima del tobillo. El rango aceptable es ese valor ± la tolerancia.

Es un modelo simplificado: debe validarse contra goniómetro o video (REQ-09). Para corregir su
sesgo con cada usuario, marque como **técnica correcta** algunas repeticiones supervisadas por un
profesional: la app calcula el ajuste personal promedio y lo aplica a las siguientes.

## Datos

Todo se guarda en el propio teléfono (almacenamiento del navegador). Borrar los datos del
navegador borra el perfil y el historial: exporte a CSV lo que necesite conservar.
