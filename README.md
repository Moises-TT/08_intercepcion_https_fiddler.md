# 🕵️ Intercepción de Tráfico HTTPS con Fiddler

> Captura y análisis de credenciales en tráfico HTTPS mediante proxy SSL con Fiddler en Windows 10 virtualizado.

---

## 📋 Descripción

Laboratorio de seguridad donde se configuró **Fiddler** como proxy HTTPS en una VM Windows 10, permitiendo descifrar y analizar el tráfico cifrado para interceptar credenciales transmitidas mediante el método **HTTP POST**.  
Se realizaron pruebas con Gmail y Outlook.com en entorno controlado.

---

## 🛠️ Stack y Herramientas

| Tecnología | Uso |
|-----------|-----|
| Windows 10 (VM KVM) | Sistema de análisis |
| Fiddler | Proxy y analizador de tráfico HTTPS |
| SSL Certificate (Fiddler CA) | Descifrado de tráfico TLS |
| Gmail / Outlook | Servicios de prueba |

---

## ⚙️ Configuración del Entorno

### 1. Instalar Windows 10 en KVMa
```
VM: win10_seguridad_informacion
Hipervisor: QEMU/KVM
Configuración: Estándar x86_64
```

### 2. Instalar y configurar Fiddler
1. Instalar **Fiddler Classic** (versión para Windows)
2. Ir a: `Tools → Settings → HTTPS`
3. Activar: ✅ **Capture HTTPS traffic**
4. Instalar certificado CA de Fiddler en el almacén del sistema:
   - "Trust CA Certificate in the User Store"
   - "Trust CA Certificate in the Machine Store"

---

## 🔬 Proceso del Laboratorio

### Captura de credenciales en Gmail

1. Activar Live Traffic en Fiddler
2. Iniciar sesión en Gmail desde el navegador
3. Filtrar por método POST:
   ```
   Filtro URL: batchexecute
   ```
4. Analizar cuerpo de la petición en la pestaña **Form-Data** / **JSON**

**Resultado obtenido:**
```
Campo: correo   → moisesmagallanes07@gmail.com
Campo: password → [credencial interceptada]
```

### Captura de credenciales en Outlook

1. Filtrar peticiones POST a: `https://login.live.com/ppsecure/post.srf`
2. Analizar campo `Form-Data` de la petición seleccionada

**Resultado obtenido:**
```
Campo: login   → moidavidmagallanes@outlook.com
Campo: passwd  → [credencial interceptada]
```

---

## 📊 Análisis Técnico

| Aspecto | Detalle |
|---------|---------|
| Protocolo atacado | HTTPS / TLS 1.2 |
| Técnica | SSL Stripping / Man-in-the-Middle proxy |
| Método HTTP | POST |
| Por qué funciona | El certificado de Fiddler es confiado por el sistema operativo |
| Defensa | Certificate pinning, verificación de certificados, HSTS |

---

## 🧠 Aprendizajes

- HTTPS no protege contra ataques en el dispositivo del usuario (endpoint)
- Un proxy SSL puede descifrar tráfico "seguro" si controla el certificado raíz
- Diferencia entre cifrado de transporte y seguridad de endpoint
- Técnicas de filtrado en Fiddler (por URL, método HTTP, cookies)
- Por qué el "candadito verde" no garantiza privacidad total

---

## ⚠️ Aviso Legal

Laboratorio realizado exclusivamente con **cuentas propias** en entorno **aislado**.  
Interceptar comunicaciones ajenas es un **delito** penalizado por la ley.
