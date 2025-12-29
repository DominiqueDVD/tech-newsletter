# 📬 Tech Newsletter Automatizado con IA (Serverless)

Sistema **serverless** que genera y envía diariamente un **informe de tendencias tecnológicas** utilizando **IA**, sin servidores y con costos mínimos.

El proyecto permite:
- Suscribirse desde un frontend estático.
- Generar contenido diario con OpenAI.
- Enviar newsletters en formato HTML por correo.
- Gestionar desuscripción automática.
- Ejecutarse de forma programada sin intervención manual.

---

## 🧠 Descripción general

Este proyecto implementa un **newsletter tecnológico diario**, completamente automatizado, usando servicios **AWS Serverless** y **OpenAI**.

El flujo completo incluye:
- Frontend estático (GitHub Pages)
- Backend serverless (AWS Lambda)
- Base de datos NoSQL (DynamoDB)
- Envío de correos (Amazon SES)
- Programación diaria (EventBridge Scheduler)
- Generación de contenido con IA (OpenAI)

Todo el sistema está diseñado para:
- Escalar automáticamente
- Tener costos extremadamente bajos
- No requerir servidores ni mantenimiento

---

## 🏗 Arquitectura del sistema
[Usuario]
↓
[Frontend - GitHub Pages]
↓
[Lambda: subscribeTechNewsletter]
↓
[DynamoDB: TechNewsletterSubscribers]

(EventBridge Scheduler - Diario)
↓
[Lambda: dailyTechReport]
↓
[OpenAI API] → Genera contenido
↓
[Amazon SES] → Envía correos HTML

[Link en correo]
↓
[Lambda: unsubscribeTechNewsletter]
↓
[DynamoDB: active = false]

---

## 🌐 Frontend

- **Tecnología:** HTML + JavaScript
- **Hosting:** GitHub Pages
- **Función principal:**  
  Permitir que un usuario ingrese su correo y se suscriba al newsletter.

El frontend llama a una **Lambda pública** mediante `fetch` para registrar el correo.

---

## 📥 Suscripción

### Lambda: `subscribeTechNewsletter`

- Recibe un email vía HTTP (POST).
- Valida el correo.
- Guarda el registro en DynamoDB con:
  ```json
  {
    "email": "usuario@email.com",
    "active": true,
    "createdAt": "timestamp"
  }

Devuelve respuesta con CORS habilitado para el frontend.

---

## 🗄 Base de datos
### DynamoDB: TechNewsletterSubscribers

✔ Clave primaria: email (Partition Key)

✔ Atributos:

  email (string)

  active (boolean)

  createdAt (string / timestamp)

Se utiliza:

  Scan para obtener suscriptores activos.

  PutItem para suscripción.

  UpdateItem para desuscripción.

--- 

## 🧠 Generación de contenido con IA
### Lambda: dailyTechReport

✔ Se ejecuta automáticamente una vez al día.

✔ Genera un informe usando OpenAI (Responses API).

✔ El prompt está diseñado para producir:

    - Noticias específicas

    - Casos reales

    - Impacto en TI, cloud, ciberseguridad y minería

✔ Devuelve HTML limpio, listo para email.

--- 

## ✉ Envío de correos
### Amazon SES

-Envío de correos HTML.

-Cada correo incluye:

  Diseño responsive

  Secciones claras

  Link personalizado de desuscripción

Ejemplo de envío:
Body: {
  Html: { Data: htmlContent, Charset: "UTF-8" }
}

---

## 🧾 Desuscripción automática
### Lambda: unsubscribeTechNewsletter

  Se accede desde un link en el correo:
    https://<lambda-url>/?email=usuario@email.com

  Marca el registro como:
    active = false

Devuelve una página HTML de confirmación.

No requiere login ni autenticación adicional.

---

## ⏱ Automatización
### EventBridge Scheduler

-Ejecuta dailyTechReport diariamente mediante CRON.

-Zona horaria configurada (America/Santiago).

-Permite reintentos automáticos en caso de fallo.

---

## 🔐 Seguridad y permisos

Cada Lambda tiene un rol IAM mínimo necesario, por ejemplo:

  - subscribeTechNewsletter: dynamodb:PutItem

  - dailyTechReport: dynamodb:Scan, ses:SendEmail

  - unsubscribeTechNewsletter: dynamodb:UpdateItem

No se usan credenciales hardcodeadas.

---

## 💰 Costos

 - AWS: prácticamente $0 USD (entra en free tier)

 - OpenAI: ~ $0.20–$0.40 USD al mes

 - Sin servidores, sin instancias, sin escalado manual

Se recomienda:

 - Configurar AWS Budgets con alerta de $1 USD.

 - Limitar gasto en OpenAI desde su panel.

---

## 🚀 Estado del proyecto

✔ Suscripción funcional
✔ Newsletter diario automatizado
✔ Envío HTML profesional
✔ Desuscripción con un clic
✔ Costos controlados
✔ 100% serverless

---

## 🛠 Posibles mejoras futuras

✔ Token de desuscripción (en vez de email visible)

✔ Panel de administración

✔ Segmentación por intereses

✔ Historial de newsletters en S3

✔ Versión pública del archivo diario

✔ Dashboard de métricas
