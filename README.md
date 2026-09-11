# CABA Control Patterns

Mapa público de evidencia histórica y agregada sobre fiscalización vehicular en CABA.

**Producción:** https://caba-control-patterns-ignacio-product.vercel.app

## Experiencia del mapa

- La home es map-first, inspirada en patrones de navegación de Waze/Google Maps: posición del usuario, mapa vial, filtros y ranking de proximidad.
- Al tocar **Ubicarme**, se activa `watchPosition` y empieza un ciclo de actualización de fuentes cada **5 minutos**.
- La ubicación precisa se procesa únicamente en el navegador; no se envía al endpoint ni se persiste.
- El panel cercano ordena por distancia y muestra metros/km, rumbo, tipo y fuerza de evidencia.
- Se muestran anillos de 1, 3 y 5 km para dar escala espacial.

## Semáforo

El semáforo mide **fuerza de evidencia histórica/estructural**, NO la probabilidad de que haya un operativo móvil ahora:

- 🔴 **Alta**: Evidence Score 85–100
- 🟡 **Media**: Evidence Score 60–84
- 🟢 **Limitada**: Evidence Score <60
- 🔵 **Infraestructura fija oficial**: cámaras/fiscalizadores publicados por Buenos Aires Data

Los fiscalizadores fijos se muestran separados porque son infraestructura permanente publicada oficialmente. Los operativos móviles siguen representándose como clusters históricos generalizados, nunca como puestos activos.

## Actualización

`/api/map-data` consulta fuentes oficiales, incluyendo el dataset de cámaras fijas de Buenos Aires Data y páginas GCBA de alcoholemia, accesos, operativos vehiculares y fiscalización. El endpoint:

- vuelve a consultar cada 5 minutos desde la UI una vez activada la ubicación;
- devuelve fecha/hora de chequeo y estado de las fuentes;
- usa hashes para detectar cambios de contenido;
- cachea hasta 240 s con `stale-while-revalidate` para mantener una frecuencia coherente con el refresh de 5 minutos.

El dataset oficial de cámaras fijas se descarga dinámicamente y no está limitado al seed del frontend.

## Seguridad y límites

- No muestra posiciones policiales actuales.
- No genera reportes crowdsourced de controles.
- No predice si un control móvil estará activo en los próximos minutos.
- No ofrece rutas para evitar fiscalización.
- Evidence Score = evidencia histórica/estructural, no probabilidad de presencia actual.

## Referencias de producto

El diseño toma patrones útiles de productos cartográficos consolidados sin replicar sus funciones de reporte policial en tiempo real:

- **Waze:** priorización y filtrado de alertas para reducir ruido visual.
- **Google Maps:** lenguaje visual tipo semáforo para comunicar intensidad relativa.
- **Mapbox:** enfoque de capas y densidad/heatmap para datos geoespaciales.
- **Buenos Aires Data:** fuente oficial para fiscalizadores/cámaras fijas.

## Infraestructura

Frontend self-contained desplegado en Vercel + función serverless `/api/map-data`. Código y metodología versionados en GitHub.

La base server-side Supabase inicialmente planificada quedó bloqueada por una limitación externa del workspace de Lovable. El mapa no necesita persistir la ubicación del usuario y el endpoint consulta fuentes oficiales directamente.
