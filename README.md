```
# Reporte de Auditoría: Seguridad en Redes Wi-Fi Públicas

**Estudiante:** Lautaro Williams  
**Rol:** Auditor de Seguridad Junior  
**Materia:** Ciberseguridad – Pre-Entrega 6  
**Fecha:** Septiembre 2026  

---

## 1. Introducción y Sitio Analizado
En este informe se evalúan los riesgos de seguridad asociados a la navegación en redes Wi-Fi públicas no protegidas [1, 5]. Como auditor de seguridad junior, se analizó el comportamiento del tráfico generado al ingresar a un sitio web sin cifrado desde un navegador [5, 6].

* **Sitio analizado:** `http://neverssl.com` [5, 6]
* **Objetivo:** Identificar la exposición de datos en tránsito y fundamentar la necesidad de mecanismos de cifrado a nivel de red (VPN) [5, 6].

---

## 2. Evidencia Observada (Herramientas de Desarrollador - F12)
Al inspeccionar la solicitud mediante la pestaña **Network (Red)** de las herramientas de desarrollador, se registraron los siguientes parámetros de red [5, 6]:

* **URL solicitada:** `http://neverssl.com` [6]
* **Método HTTP:** `GET` [2, 6]
* **Host:** `neverssl.com` [2, 6]
* **Protocolo utilizado:** `HTTP` (sin cifrar) [6, 7]
* **Headers enviados:** `User-Agent` (detalla navegador, sistema operativo y cliente) [2, 6].

&gt; **Observación técnica:** La comunicación se realizó en texto plano sin ninguna capa de transporte seguro (TLS/SSL), dejando los metadatos y la navegación completamente visibles en el cable/aire de la red [6, 7].

---

## 3. Riesgos de Seguridad en Wi-Fi Pública
Navegar en redes abiertas (cafeterías, aeropuertos) mediante el protocolo inseguro HTTP expone la conexión a múltiples vectores de ataque [1, 8]:

1. **Intercepción de Datos (*Sniffing*):** Un atacante conectado a la misma red puede usar analizadores de tráfico (como Wireshark) para capturar los paquetes de datos transmitidos y leer en texto claro información confidencial o credenciales [2, 8].
2. **Ataques de Intermediario (*Man-in-the-Middle - MitM*):** Un cibercriminal puede interponerse entre el dispositivo del usuario y el router de la cafetería, alterando la información transmitida o redirigiendo al usuario a sitios fraudulentos [2, 8].
3. **Redes Gemelas Malignas (*Evil Twins*):** Creación de un punto de acceso Wi-Fi falso con el mismo nombre del local para atraer usuarios y capturar todo su tráfico de red [8].

---

## 4. Protección mediante Red Privada Virtual (VPN)
Al activar una VPN en una red Wi-Fi pública, el escenario de riesgo cambia radicalmente [2, 7]:

* **Túnel Seguro y Encapsulamiento:** La VPN crea un túnel cifrado de extremo a extremo que encapsula todo el tráfico enviado desde el dispositivo hacia el servidor de la VPN [2, 7].
* **Cifrado de Datos:** Aunque un atacante realice *sniffing* o se conecte a la misma Wi-Fi, solo capturará paquetes con ruido ininteligible debido al cifrado robusto [2, 7, 8].
* **Privacidad de IP y Ubicación:** La dirección IP real del usuario queda oculta, reemplazándose por la IP del servidor VPN, evitando el rastreo de actividad e IP por parte del proveedor local y sitios de destino [2, 7].

---

## 5. Las 3 Reglas de Oro para Redes Wi-Fi Públicas

1. **Usar siempre una VPN activa:** Activar el túnel cifrado antes de conectarte o realizar cualquier tipo de navegación en redes públicas o abiertas [2, 7].
2. **Navegar exclusivamente por sitios HTTPS:** Verificar que los sitios tengan el candado de seguridad y el protocolo `https://` activo antes de ingresar datos [2, 7].
3. **Evitar transacciones sensibles:** No acceder a home banking, billeteras virtuales o correo institucional cuando estés conectado a redes Wi-Fi públicas compartidas [2, 7].

```
