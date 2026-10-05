# proxies rotativos: cómo funcionan, cuánto cuestan por GB y cómo configurarlos sin contratar una suscripción

Quien escribe "proxies rotativos" en el buscador normalmente tiene un problema muy concreto: una tarea que necesita muchas IP distintas y un presupuesto que no da para planes mensuales de 200 o 300 dólares. Puede ser scraping, monitoreo de precios, verificación de anuncios o gestionar varias cuentas a la vez. Lo que casi nadie busca es una clase teórica sobre redes.

Así que vamos a lo práctico: qué rota exactamente, cómo se conecta, qué se paga hoy por GB y dónde están las trampas de facturación que sí te cambian el presupuesto a fin de mes.

## Qué es un proxy rotativo y en qué se diferencia de uno sticky

Un proxy rotativo es un punto de entrada al que te conectas siempre con la misma dirección y que te devuelve una IP de salida distinta en cada solicitud. Tú no gestionas la lista de IPs, no compruebas cuáles están vivas y no reescribes tu código cada vez que una se cae. El proveedor se encarga del pool y tú solo apuntas a un host y un puerto.

El sticky funciona al revés: la IP se mantiene durante un periodo definido, normalmente entre 1 y 120 minutos, y luego cambia. Sirve cuando el sitio que visitas necesita continuidad: un carrito, un login, una sesión de navegador donde un salto de IP a mitad de camino levanta sospechas.

La diferencia práctica es esta:

- **Rotativo:** distribución masiva de solicitudes, crawling de muchas URLs, evitar límites de velocidad.
- **Sticky:** flujos con estado, donde romper la sesión implica repetir el trabajo.

Los dos suelen venir incluidos en el mismo plan y se activan cambiando el puerto o el sufijo del usuario. Ese detalle técnico es el que más confusión genera, así que vale la pena verlo con un ejemplo real.

## Cómo se ve una configuración rotativa real

En DataImpulse, que es el proveedor que vamos a usar como referencia en el resto del artículo, la puerta de entrada es `gw.dataimpulse.com` y el tipo de sesión se define con el puerto:

- **Rotativo HTTP/HTTPS:** puerto 823
- **Rotativo SOCKS5:** puerto 824
- **Sticky:** puertos entre 10000 y 20000

El formato del usuario carga los parámetros de sesión y geolocalización directamente en la cadena de autenticación. Un ejemplo típico queda así:


http://usuario:contraseña@gw.dataimpulse.com:823


Y si quieres fijar país o ciudad, se añaden como sufijos del usuario:


# Solo país
http://usuario:contraseña_country-us@gw.dataimpulse.com:823

# País + ciudad
http://usuario:contraseña_country-us_city-newyork@gw.dataimpulse.com:823

# Sesión sticky identificada
http://usuario:contraseña_session-abc123@gw.dataimpulse.com:823


Esto funciona en Python con requests o Scrapy, en Playwright, en Puppeteer, en Selenium y en cualquier navegador antidetect. No hay que instalar un agente ni una extensión: es autenticación usuario/contraseña estándar, o alternativamente lista blanca por IP si prefieres no exponer credenciales en el código.

Un detalle que puede arruinar una sesión sin que te des cuenta: si defines una sesión sticky sin intervalo o lo pones en "0", el periodo por defecto es de 30 minutos. La duración real, además, depende de que el dispositivo del usuario detrás de esa IP siga conectado. Si se desconecta, la sesión termina antes.

## Cuánto debería costar un proxy rotativo por GB

El precio de los proxies residenciales se ha movido bastante en los últimos años. La media del mercado para tráfico residencial rotativo sigue estando entre 3 y 8 dólares por GB, y muchos proveedores lo esconden detrás de suscripciones mensuales con cuotas y tráfico que caduca al final del ciclo. Ese último punto es el que genera gasto invisible: pagas 100 GB, usas 40 ese mes y los otros 60 desaparecen.

En el extremo barato del mercado están los proveedores de pago por uso. DataImpulse es uno de los que marcó ese piso, con tarifa plana de 1 dólar por GB de tráfico residencial, sin suscripción y sin caducidad del saldo. Índices de precios de terceros que se actualizan revisando páginas públicas de tarifas colocan a DataImpulse en 1,00 $/GB de pago por uso y 0,80 $/GB en su tramo más bajo, que es de los más económicos que se publican.

La comparación honesta no es "quién tiene el número más bajo", sino cuál es el costo por solicitud exitosa con tus objetivos. Si necesitas IPs residenciales contra sitios que revisan reputación, un pool de 1 dólar por GB con menos reintentos puede salir más barato que uno de 0,60 dólares con bloqueos constantes. Y si tu tarea funciona bien con IPs de datacenter, pagar tarifa residencial es tirar dinero.

## Planes y precios de DataImpulse

El proveedor organiza su catálogo en cuatro tipos de proxy, y cada tipo tiene su propia escalera de tramos. Todos comparten el mismo modelo: pagas por uso, no hay cuota mensual y el tráfico comprado no caduca.

| Tipo de proxy | Tramo | Precio por GB | Inversión mínima | Ciclo de facturación | Enlace |
| --- | --- | --- | --- | --- | --- |
| Residencial (90M+ IPs, 195+ países) | Intro | 1,00 $/GB | 5 $ (5 GB) | Pago por uso, sin caducidad | [Ver el plan Intro de residenciales](https://bit.ly/dataimPulse) |
| Residencial | Basic | 1,00 $/GB | 50 $ (50 GB) | Pago por uso, sin caducidad | [Empezar con el tramo Basic](https://bit.ly/dataimPulse) |
| Residencial | Advanced (1 TB+) | ~0,80 $/GB | 800 $ (1 TB) | Pago por uso, sin caducidad | [Consultar el precio por volumen](https://bit.ly/dataimPulse) |
| Residencial | Custom (5 TB+) | Precio negociado | Desde 4.000 $ | Acuerdo personalizado | [Solicitar configuración personalizada](https://bit.ly/dataimPulse) |
| Datacenter | Intro / Basic | 0,50 $/GB | 5 $ (10 GB) | Pago por uso, sin caducidad | [Ver planes de datacenter](https://bit.ly/dataimPulse) |
| Datacenter | Advanced (1 TB+) | ~0,45 $/GB | 450 $ (1 TB) | Pago por uso, sin caducidad | [Ver tarifa por volumen de datacenter](https://bit.ly/dataimPulse) |
| Datacenter | Custom (5 TB+) | Precio negociado | Desde 2.250 $ | Acuerdo personalizado | [Pedir cotización](https://bit.ly/dataimPulse) |
| Móvil (4G/5G/LTE) | Intro | 2,00 $/GB | 5 $ (2,5 GB) | Pago por uso, sin caducidad | [Ver planes de proxies móviles](https://bit.ly/dataimPulse) |
| Móvil | Basic | 2,00 $/GB | 50 $ (25 GB) | Pago por uso, sin caducidad | [Ver tramo Basic móvil](https://bit.ly/dataimPulse) |
| Móvil | Advanced (1 TB+) | ~1,60 $/GB | 1.600 $ (1 TB) | Pago por uso, sin caducidad | [Consultar volumen móvil](https://bit.ly/dataimPulse) |
| Móvil | Custom (5 TB+) | Precio negociado | Desde 8.000 $ | Acuerdo personalizado | [Solicitar precio a medida](https://bit.ly/dataimPulse) |
| Residencial premium | Intro | 5,00 $/GB | 5 $ (1 GB) | Pago por uso, sin caducidad | [Ver residencial premium](https://bit.ly/dataimPulse) |
| Residencial premium | Basic | 5,00 $/GB | 50 $ (10 GB) | Pago por uso, sin caducidad | [Empezar con premium](https://bit.ly/dataimPulse) |
| Residencial premium | Custom (5 TB+) | Precio negociado | Desde 20.000 $ | Acuerdo personalizado | [Contactar con el equipo](https://bit.ly/dataimPulse) |

Un mínimo de 5 dólares en cada tipo de producto es lo que hace que probar sea barato. Con 5 dólares compras 5 GB de residencial, 10 GB de datacenter o 2,5 GB de móvil, y ese saldo sigue ahí el mes que viene si no lo gastas.

### Qué obtienes en cada tipo de proxy

**Residencial** es el caballo de batalla. Pool anunciado de más de 90 millones de IPs en 195+ países, sesiones rotativas y sticky, HTTP/HTTPS y SOCKS5, segmentación por país incluida y tráfico que nunca caduca. La segmentación por estado, ciudad, ZIP y ASN está disponible pero se factura al doble de la tarifa base en este producto.

**Datacenter** es la opción económica y rápida: 0,50 $/GB, uptime anunciado del 99,9% y acceso aleatorio a subredes. Va bien para crawling de gran volumen sobre sitios que no bloquean agresivamente IPs de servidor.

**Móvil** usa IPs reales de operadores 4G/5G/LTE a 2 $/GB. Es el tipo más resistente a bloqueos porque el NAT del operador hace que muchísimos usuarios compartan la misma IP. También es el más caro por GB, así que usarlo para tareas que un datacenter resolvería es tirar presupuesto.

**Residencial premium** es el tramo alto: 5 $/GB, todas las opciones de segmentación sin recargo y gestor de cuenta dedicado. Tiene sentido cuando el objetivo es especialmente duro y los reintentos del pool estándar ya te están costando más que la diferencia de precio.

## Errores y costos ocultos que conviene revisar antes de pagar

Esto es lo que suele sorprender a quien contrata proxies rotativos por primera vez.

**La segmentación avanzada no siempre está incluida.** En el producto residencial estándar, filtrar por estado, ciudad, ZIP o ASN se cobra al doble de la tarifa base. Si tu configuración pide ciudad en cada solicitud, tu GB efectivo deja de costar 1 dólar. El país sí viene incluido.

**Las sesiones sticky tienen techo.** Aquí el máximo es de 120 minutos, con una media real de unos 30, y depende de que el usuario siga en línea. Si necesitas coherencia de sesión durante horas, este no es el proveedor adecuado.

**No hay prueba gratuita sin pago.** Todo acceso empieza con una compra mínima de 5 dólares. Lo que sí existe es una garantía de devolución de 7 días en los planes Intro, aplicable a pagos con tarjeta y siempre que hayas consumido menos del 80% del tráfico. Los pagos con criptomonedas no son reembolsables.

**No hay un código de descuento público.** Varios sitios publican "cupones" de DataImpulse, pero no existe un código promocional oficial. El precio ya es la oferta: 1 dólar por GB con tráfico que no caduca es más bajo que las tarifas promocionales de la mayoría de proveedores grandes. Cualquier página que te pida un código para "desbloquear" un descuento está vendiendo humo.

## Cómo empezar sin quemar presupuesto

El orden que funciona, sobre todo si nunca has usado un pool rotativo, es este:

1. **Crea la cuenta y compra el plan mínimo.** Cinco dólares, no cien. Con eso validas el flujo completo.
2. **Define el tipo de sesión según la tarea.** Rotativo en el puerto 823 para crawling masivo; sticky en el rango 10000–20000 si el sitio exige continuidad.
3. **Activa solo la segmentación que necesitas.** El país es gratis. Si añades ciudad sin justificarlo, pagas el doble por tráfico.
4. **Mide el costo por solicitud exitosa, no por GB.** Ejecuta una tanda contra tu objetivo real y mira cuánto tráfico gastaste por resultado útil.
5. **Compra volumen solo después.** Ahí sí tiene sentido subir al tramo de 1 TB y su descuento del 20% en residencial y móvil, porque el saldo no se pierde.

👉 [Empezar con 5 dólares en proxies rotativos](https://bit.ly/dataimPulse)

Ese paso 4 es el que separa a quien ahorra de quien no. Un proveedor de 1 dólar por GB que acierta en el primer intento sale más barato que uno de 0,50 que reintenta cinco veces con la misma URL.

## Casos de uso donde el rotativo marca la diferencia

**Web scraping a escala.** Cada solicitud con IP distinta reparte la carga y evita los límites de velocidad por dirección. Es el caso más común y el que mejor aprovecha el modelo rotativo.

**Monitoreo de precios y SERP.** Necesitas ver el precio o el ranking tal como lo ve un usuario de un país concreto. La segmentación por país incluida resuelve esto sin costos extra.

**Verificación de anuncios.** Comprobar qué creatividad se muestra en cada región requiere muchas IPs locales y sesiones cortas. Rotativo puro.

**Gestión de múltiples cuentas.** Aquí el rotativo sirve para repartir la actividad, pero para cada cuenta necesitas una IP estable. La combinación habitual es un pool rotativo para el trabajo general y sesiones sticky identificadas para cada perfil.

**Pruebas de aplicaciones móviles.** Solo tiene sentido con el producto móvil, y solo si la app realmente se comporta distinto según la red. Si no, estarías pagando 2 $/GB por nada.

## Preguntas frecuentes

### ¿Los proxies rotativos son legales?

Rotar IPs es una técnica, no una actividad. Su legalidad depende de qué hagas con ella y de las condiciones del sitio que visitas. En el caso de este proveedor, el pool se describe como de origen ético, con usuarios que aceptan ceder ancho de banda mediante un SDK divulgado y reciben pago por ello, cumplimiento GDPR y certificación ISO. Es una diferencia relevante frente a pools de origen opaco, porque un pool construido sin consentimiento es un problema legal y reputacional para quien lo usa.

### ¿Cuántas IPs distintas puedo esperar?

El pool anunciado es de más de 90 millones de IPs en 195+ países y ubicaciones. En la práctica, cuántas IPs concretas veas depende del país, de la hora y de la demanda del resto de usuarios. Para países grandes como Estados Unidos, Alemania o Brasil, la rotación es amplia. Para mercados pequeños, menos.

### ¿Cuánto tarda en configurarse?

Con autenticación usuario/contraseña, lo que tardas en pegar dos líneas en tu script. Hay tutoriales oficiales para Python, Selenium, Playwright y Puppeteer, además de guías para GoLogin, Octo Browser, Multilogin y MoreLogin. No hay aprobación de cuenta ni llamada de ventas.

### ¿Y el rendimiento?

El proveedor publica una tasa de éxito del 99,51% y un tiempo de respuesta inferior a un segundo, con una valoración de 4,8 sobre 5 en G2 y una base declarada de más de 500.000 clientes. Son cifras oficiales, no una medición independiente, así que lo sensato es tratarlas como punto de partida y validarlas con tu propio objetivo durante los primeros 5 GB.

### ¿Cuándo NO conviene este proveedor?

Si necesitas proxies ISP estáticos, una API de scraping totalmente gestionada o acceso a sitios bancarios y gubernamentales, este no es el producto. Su foco son pools rotativos residenciales, móviles y de datacenter para recopilar datos públicos y acceder a contenido. Tampoco es la mejor opción si tus flujos dependen de sesiones sticky de varias horas.

## Cierre

La decisión práctica con proxies rotativos se reduce a tres preguntas: cuántos GB vas a consumir de verdad, cuánta segmentación geográfica necesitas en cada solicitud y si tu volumen es estable o irregular. Si es irregular, cualquier suscripción con tráfico que caduca te va a cobrar por nada. Un modelo de pago por uso a 1 dólar por GB, con saldo que no expira y un mínimo de 5 dólares para probar, deja el riesgo donde debe estar: en cinco dólares y una tarde de configuración.

👉 [Ver precios actuales de proxies rotativos](https://bit.ly/dataimPulse)
