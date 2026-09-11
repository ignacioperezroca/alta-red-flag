# CABA Control Patterns

Dashboard público de evidencia histórica y agregada sobre fiscalización vehicular en CABA.

**Producción:** https://caba-control-patterns-ignacio-product.vercel.app

- No muestra controles activos ni posiciones policiales.
- No predice presencia policial actual ni ofrece rutas para evitar fiscalización.
- La geolocalización es opcional y se procesa sólo en el navegador.
- Si no hay permiso de ubicación, usa una referencia general de Núñez.
- La app muestra clusters históricos generalizados, ranking por distancia, contexto temporal histórico, heatmap semanal, calendario anual y fuentes oficiales.

## Infraestructura

Frontend estático self-contained desplegado en Vercel. Las fuentes oficiales y el dataset seed están versionados en GitHub.

La persistencia server-side Supabase quedó bloqueada por una limitación externa del workspace de Lovable: el workspace está sin créditos y el conector actual no tiene el scope `projects:write` necesario para ejecutar SQL. Por eso esta versión usa IndexedDB local para persistencia de preferencias/cache y expone la limitación dentro de la UI.

## Seguridad

Evidence Score = fuerza de evidencia histórica, no probabilidad de encontrar un control.

La ubicación del usuario no se envía ni se persiste. Los puntos del mapa son centroides analíticos generalizados, no puestos operativos.
