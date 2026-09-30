# Reporte de Auditoría: Seguridad en Redes Wi-Fi Públicas

**Estudiante:** Lautaro Williams  
**Rol:** Auditor de Seguridad Junior  
**Materia:** Ciberseguridad – Pre-Entrega 6  
**Fecha:** Septiembre 2026  

---

## 1. Introducción y Sitio Analizado
En este informe de auditoría se evalúan las vulnerabilidades y riesgos de seguridad asociados al tráfico no cifrado en redes Wi-Fi públicas o abiertas (ej. cafeterías, aeropuertos). Como auditor de seguridad junior, se analizó el comportamiento de la capa de aplicación e inspecionó la exposición de datos al interactuar con un sitio web sin HTTPS.

* **Sitio analizado:** `http://neverssl.com` (sitio diseñado deliberadamente sin TLS para evitar el bloqueo de portales cautivos).
* **Objetivo técnico:** Identificar la exposición de metadatos y contenido en tránsito, demostrando la necesidad de implementar mecanismos de cifrado de red como una **VPN (Virtual Private Network)**.

---

## 2. Evidencia Observada y Análisis de Cabeceras (DevTools - F12)

Al inspeccionar la solicitud mediante las herramientas de desarrollador del navegador (**Pestaña Network / Red**), se capturaron los siguientes parámetros de red reales:

* **Request URL:** `http://oldslowlushstars.neverssl.com/online/`
* **Request Method:** `GET`
* **Status Code:** `200 OK`
* **Remote Address (IP/Puerto):** `[2600:1f13:37c:1400:ba21:7165:5fc7:736e]:80`
* **Host Header:** `oldslowlushstars.neverssl.com`
* **User-Agent Header:** `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/154.0.0.0 Safari/537.36`
* **Protocolo:** `HTTP/1.1` (no cifrado / texto claro)

### Evidencia Capturada:
<img width="1920" height="1035" alt="03" src="https://github.com/user-attachments/assets/61aa5a93-6da9-402b-a201-d5005cf922dd" />


&gt; **Diagnóstico técnico de auditoría:** La comunicación carece por completo de la capa de transporte seguro (TLS/SSL). Los encabezados HTTP y el cuerpo de la respuesta viajan en texto claro por el aire de la red local, siendo susceptibles de interceptación pasiva o manipulación activa.

---

## 3. Vectores de Ataque y Riesgos en Wi-Fi Pública

Navegar mediante el protocolo HTTP en un entorno de red compartido expone al usuario a diversos vectores de amenaza:

1. **Intercepción Pasiva de Datos (Sniffing):** Cualquier actor malicioso en la misma red Wi-Fi puede colocar su interfaz en modo promiscuo y utilizar herramientas como Wireshark para capturar tramas de red, leyendo credenciales, *cookies* de sesión y formularios ingresados.
2. **Ataques de Intermediario (Man-in-the-Middle - MitM) y ARP Spoofing:** El atacante envenena las tablas ARP de la red local para posicionarse entre el dispositivo de la víctima y el gateway/router, interceptando o alterando el tráfico en tiempo real.
3. **Robo de Sesión (Session Hijacking):** La falta de cifrado permite extraer *tokens* de autenticación y *cookies*, permitiendo al atacante suplantar la identidad de la víctima en la plataforma web.
4. **Riesgo de Movimiento Lateral:** Una vez comprometida la sesión o credenciales en la red pública, el atacante puede intentar el pívot o movimiento lateral hacia cuentas institucionales o bancarias vinculadas que compartan patrones de contraseña o correo.
5. **Redes Gemelas Malignas (Evil Twins):** Creación de un punto de acceso Wi-Fi falso con un SSID idéntico al del establecimiento comercial para redirigir todo el tráfico a servidores controlados por el atacante.

---

## 4. Mitigación y Protección mediante Red Privada Virtual (VPN)

Al implementar una VPN sobre la conexión en una red Wi-Fi pública, el perfil de riesgo cambia sustancialmente:

* **Túnel de Encapsulamiento Cifrado:** La VPN encapsula todo el tráfico IP del dispositivo dentro de un túnel cifrado (utilizando algoritmos robustos como AES-256 o ChaCha20) desde el cliente hacia el servidor VPN.
* **Inviolabilidad ante Sniffing Local:** Aunque un atacante capture los paquetes en el aire de la red Wi-Fi, solo obtendrá tramas de datos cifrados (*ruido ininteligible*), preservando la **Confidencialidad** y la **Integridad** de la información.
* **Ocultamiento de IP y Anonimización:** La dirección IP pública del usuario es reemplazada por la IP del servidor VPN, impidiendo el rastreo geográfico y bloqueando perfilamientos por parte de terceros o proveedores ISP locales.

---

## 5. Las 3 Reglas de Oro para Navegación en Redes Abiertas

1. **Usar siempre una VPN activa con Kill Switch:** Establecer el túnel cifrado antes de conectarse a cualquier red pública y verificar que la función *Kill Switch* esté habilitada para evitar fugas de datos si la VPN se desconecta.
2. **Exigir conexiones HTTPS y HSTS:** Verificar la presencia del candado digital y asegurarse de que los sitios web operen con protocolo seguro. Si aparece una advertencia de certificado o sitio no seguro, interrumpir la navegación.
3. **Cero Operaciones Sensibles y Deshabilitar Conexión Automática:** Evitar el acceso a plataformas de *Home Banking*, billeteras virtuales o correo institucional en Wi-Fi públicas. Asimismo, desactivar en el dispositivo la opción de "conectarse automáticamente a redes abiertas".

```
