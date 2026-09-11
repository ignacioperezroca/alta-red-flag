# CABA Control Patterns

Dashboard público de evidencia histórica y agregada sobre fiscalización vehicular en CABA.

- No muestra controles activos ni posiciones policiales.
- No predice presencia policial actual ni ofrece rutas para evitar fiscalización.
- La geolocalización es opcional y se procesa sólo en el navegador.
- Si no hay permiso de ubicación, usa una referencia general de Núñez.
- La app muestra clusters históricos generalizados, ranking por distancia, contexto temporal histórico, heatmap semanal, calendario anual y fuentes oficiales.

## Infraestructura

Frontend estático self-contained preparado para Vercel. Las fuentes oficiales se versionan dentro del bundle. La persistencia server-side Supabase quedó bloqueada por una limitación externa del workspace de Lovable (sin créditos y sin scope SQL en el conector), por lo que esta versión usa IndexedDB local para persistencia de preferencias/cache y expone esa limitación en la UI.

## Seguridad

Evidence Score = fuerza de evidencia histórica, no probabilidad de encontrar un control.
