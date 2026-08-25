# OhSINT (Español)

Hecho por: Maria Julia Souza
Categoría: OSINT Sitio: tryhackme.com
Enlace: <https://tryhackme.com/room/ohsint>

## Introducción

El desafío consiste en descubrir la mayor cantidad de información posible utilizando únicamente una imagen de Windows XP.

[![GoogleXp](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/googlexp-OhSINT1.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/googlexp-OhSINT1.png)

Para facilitar el proceso, utilicé la máquina virtual proporcionada por el propio sitio; sin embargo, dentro de la room también es posible descargar la imagen para realizar la investigación de forma local. La imagen del desafío en la máquina de TryHackMe se encuentra en `/Rooms/OhSint`.

[![Diretorio](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT2.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT2.png)

### Inicio de la investigación

Como el desafío menciona una imagen, el primer paso natural es verificar si contiene metadatos EXIF (información como fecha, hora, autor, GPS, etc.). Abrí el directorio en la terminal usando el comando `cd Rooms/OhSINT` para localizar la imagen. Después de eso, usé la herramienta exiftool (una herramienta específica para leer, escribir y editar metadatos), lo que me permitió leer la información EXIF de la imagen.

[![exiftool](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT3.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT3.png)

```
Copyright                     : OWoodflint  

GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W  
```

Entre las líneas devueltas, encontré información muy útil, como el autor de la imagen y una posible ubicación de donde fue tomada. Para mayor claridad, recorté el resultado dejando solo las líneas relevantes; el resto son metadatos técnicos estándar que no serían útiles en este contexto.

Al encontrar el nombre del autor (OWoodflint), hice una búsqueda simple en Google y encontré perfiles con el mismo nombre de usuario en dos sitios: X.com y github.com (en el cual profundizaremos más adelante).

[![busca](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT4.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT4.png)

Al encontrar el perfil de X del usuario, pude responder las preguntas solicitadas por la room:

[![perfil x](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT5.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT5.png)

### What is this user's avatar of?
Traducción: *¿De qué es el avatar de este usuario?*

Al investigar el perfil del usuario en X, encontré que su foto de perfil es un gato.

**Respuesta:** `cat`

### What is the SSID of the WAP he connected to?
Traducción: *¿Cuál es el SSID del punto de acceso (WAP) al que se conectó?*

En una de sus publicaciones en X podemos encontrar el BSSID de su red `(B4:5D:50:AA:86:41)`.

[![gato do perfil](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT6.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT6.png)

Inicialmente intenté la búsqueda básica (basic search) de Wigle usando solo el BSSID, pero no obtuve ningún resultado en el sitio. Después de varios intentos, me di cuenta de que era necesario usar la búsqueda avanzada (Advanced Search). Al usar la búsqueda avanzada de la plataforma Wigle (una plataforma que mapea e indexa redes Wi-Fi, Bluetooth y estaciones de radio base) e ingresar el BSSID, encontré la siguiente información:

[![BSSID](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT7.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT7.png)

Donde podemos verificar el nombre del SSID: `UnileverWiFi`.

**Respuesta:** `UnileverWiFi`

### What is his personal email address?
Traducción: *¿Cuál es la dirección de correo electrónico personal de él?*

Para descubrir la respuesta a esta pregunta y a las siguientes, investigué el GitHub encontrado en la búsqueda: <https://github.com/OWoodfl1nt/people_finder>, donde encontré un repositorio llamado `people_finder`. En este repositorio público pude encontrar un archivo README que permitió obtener más información sobre el usuario, como su correo electrónico, su blog personal (que exploraremos más adelante) y de dónde es.

[![GitHub](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT8.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT8.png)

**Respuesta:** `OWoodflint@gmail.com`

### What site did you find his email address on?
Traducción: *¿En qué sitio encontraste su dirección de correo electrónico?*

**Respuesta:** `Github`

### What city is this person in?
Traducción: *¿En qué ciudad se encuentra esta persona?*

**Respuesta:** `London`

### Where has he gone on holiday?
Traducción: *¿A dónde fue de vacaciones?*

Al acceder al sitio encontrado anteriormente en el repositorio de GitHub, pude acceder al blog personal del usuario. Se trata de un blog muy simple, y la única información que podemos obtener es un texto en el que el propio usuario menciona su viaje a Nueva York. Podemos entonces asumir que sus vacaciones fueron en Nueva York.

[![Site do usuário](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT9.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT9.png)

**Respuesta:** `New York`

### What is the person's password?
Traducción: *¿Cuál es la contraseña de la persona?*

Después de revisar las funcionalidades del blog, decidí inspeccionar el código fuente de la página. Allí fue posible encontrar un párrafo escrito en texto blanco: "pennYDr0pper.!" en el código HTML (probablemente un intento de ocultar la contraseña camuflándola con el fondo). Al probar esta respuesta, se confirmó que era la contraseña del usuario.

[![Codigo-fonte](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT10.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT10.png)

**Respuesta:** `pennYDr0pper.!`

## Conclusión

Este desafío mostró cómo es posible reunir un perfil prácticamente completo de una persona a partir de una sola imagen, sin utilizar ninguna técnica de intrusión: solamente información pública y metadatos que la mayoría de las personas ni siquiera sabe que existen. A partir de un simple campo de "Copyright" en una foto, fue posible rastrear redes sociales, correo electrónico, ciudad, red Wi-Fi, contraseña e incluso el destino de un viaje.

Esto refuerza un punto importante sobre la seguridad digital: los metadatos de las imágenes (como GPS y autor) y la información compartida en redes sociales pueden parecer inofensivos por separado, pero juntos forman un rastro capaz de comprometer la privacidad de alguien. Como buena práctica, se recomienda eliminar los metadatos de las fotos antes de publicarlas y evitar compartir detalles sensibles (como el BSSID de una red o contraseñas, aunque estén "ocultas" en el código fuente de un sitio) de forma pública.
