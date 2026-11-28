# Ingeniería inversa · <pedidosya>

> Historia H1.1. Estudia el sitio con las DevTools del navegador (F12) y documenta al menos
> 10 hallazgos con capturas. Guarda las capturas en `docs/img/`.

> Reglas: se recrea la experiencia, no la marca. No descargues sus archivos; no recrees
> pantallas de inicio de sesión ni de pago.

## 1. Estructura (pestaña Elements)

- ¿Qué etiquetas semánticas usa (`header`, `nav`, `main`, `article`…)? ¿Cuáles faltan?El sitio utiliza etiquetas estructurales clave como header para la barra de navegación superior fija, main para el contenedor principal de contenido, y section para agrupar bloques temáticos como categorías y listados de comercios. Sin embargo, se observa una falta de granularidad semántica, ya que muchas tarjetas individuales de restaurantes se construyen con div genéricos en lugar de etiquetas article, lo que resta accesibilidad.
- ¿Cómo está organizado el layout (Flexbox, Grid)?El diseño combina ambos modelos de CSS. Se implementa Flexbox (declarado como display: flex) principalmente para la alineación de elementos en el encabezado, menús de navegación y filas de categorías. Por otro lado, la grilla de restaurantes y locales comerciales se estructura mediante CSS Grid (display: grid), permitiendo una adaptación fluida desde una columna en vistas móviles hasta múltiples columnas en escritorio.

## 2. Sistema de diseño (pestaña Elements → Computed)

| Elemento | Valor |
| --- | --- |
| Color principal | | Rojo|/Fosforescente (#E61937 o similar de la marca) 
| Color de fondo | |   Blanco (#FFFFFF) y gris muy claro (#F8F9FA)  
| Tipografía de títulos | | TT Commons Pro (Sans-serif)  
| Tipografía de texto | | TT Commons Pro (Sans-serif secundaria)
| Tamaño base del texto | |12px (o 14px según el viewport)
| Espaciado típico | | Múltiplos de 8px (padding/margin: 8px, 16px, 24px)
| Radio de bordes | | border-radius: 12px a 16px en tarjetas

## 3. Red (pestaña Network → Fetch/XHR)

- ¿Qué peticiones hace al cargar? ¿A qué URL? Al cargar la página, el navegador dispara múltiples peticiones asíncronas de tipo XHR y fetch. Entre las principales, destacan llamadas a /home-stage-stat-feed (para obtener el catálogo de comercios según la geolocalización), /location (para validar la dirección del usuario) y diversos endpoints de telemetría como /collector y /data/ para métricas de rendimiento.
- ¿Qué formato tienen los datos (JSON)? Pega un ejemplo resumido. La información se transfiere exclusivamente en formato JSON. Un ejemplo simplificado de la estructura de datos recibida para los bloques de la tienda es:
{
  "country": "VE",
  "city": "Caracas",
  "status": 200,
  "stages": [
    {
      "id": "popular_restaurants",
      "title": "Tiendas",
      "items_count": 12
    }
  ]
}
- ¿Qué pasa en la red cuando filtras o buscas?   Al interactuar con los filtros por categoría o utilizar el buscador, la página evita recargar el documento HTML completo. En la pestaña Network se registra un nuevo disparo fetch/xhr enviando los parámetros de búsqueda o el ID del filtro. El servidor responde con un nuevo objeto JSON y el JavaScript del frontend actualiza dinámicamente solo el contenedor de la grilla de restaurantes. 

## 4. Estado (pestaña Application)

- ¿Qué guarda en la URL (parámetros)
?## 4. Estado (pestaña Application)
  La URL conserva parámetros de contexto geográfico y de sesión indispensables para la experiencia, tales como el código del país (country=VE), el identificador o nombre de la ciudad (city=Caracas) y parámetros de seguimiento de marketing que aseguran la persistencia del contexto al navegar entre secciones.
-  ¿Qué guarda en cookies y localStorage?s
  En el Local Storage se almacena información persistente del cliente, incluyendo la dirección de entrega exacta seleccionada, coordenadas y el estado temporal de los productos agregados al carrito de compras. En las Cookies, el sistema retiene identificadores de sesión cifrados (session_id), preferencias de idioma/región y tokens de seguridad destinados a proteger la navegación contra ataques CSRF.

## 5. Rendimiento y accesibilidad (pestaña Lighthouse)

| Métrica | Sitio original | Mi versión |
| --- | --- | --- |
| Rendimiento | 27 | Pendiente |
| Accesibilidad |90 | Pendiente |
| Buenas prácticas | 50| Pendiente |
![Auditoría Lighthouse](docs/img/09-lighthouse.png)
## 6. Hallazgos

| # | Hallazgo | Captura |
| --- | --- | --- |
  1  Uso de contenedores genéricos en lugar de etiquetas semánticas	
  2 Implementación de CSS Grid para grilla de restaurantes	
  3	Tipografía corporativa TT Commons Pro	
  4	Carga asíncrona de datos mediante peticiones XHR/Fetch	
  6	Uso de cookies de sesión para retención de mercado	
  7  	Optimización de imágenes mediante formatos modernos WebP	
  8	Comportamiento sticky del header durante el scroll	
  9	Aplicación de border-radius consistente en UI	
10	Disparo de eventos de telemetría en segundo plano 

## 7. Tres mejoras que hice en mi versión (H1.3)

1.Implementación estricta de etiquetas semánticas (article, section) en todas las tarjetas de restaurantes para mejorar la accesibilidad web.
2.Optimización del tiempo de carga inicial eliminando scripts de terceros y telemetría innecesaria presentes en el sitio original
3.Mejora en el contraste de los textos secundarios para cumplir con los estándares WCAG AA, superando la legibilidad del diseño de referencia.S