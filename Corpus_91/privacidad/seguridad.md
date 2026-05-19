---
titulo: Medidas de seguridad de 91
tema: privacidad
tipo: legal
fuente: Politica de privacidad.txt
tags: [seguridad, https, hash, rls, tokens, rate-limiting]
---

# Medidas de seguridad de 91

91 implementa medidas razonables para proteger la información de los usuarios.

## Medidas de seguridad aplicadas

- **HTTPS** en todas las conexiones.
- **Contraseñas hasheadas** mediante Supabase Auth.
- **Row-Level Security (RLS)** en la base de datos.
- **Control de acceso mediante tokens**.
- **Rate-limiting** contra abuso.

## Limitación

Ningún sistema es 100% impenetrable. La plataforma aplica medidas razonables, pero no garantiza seguridad absoluta.

## Recomendación al usuario

Se recomienda usar una contraseña **fuerte y única** para la cuenta de 91 (no reutilizar contraseñas de otros servicios).

## Preguntas frecuentes

**¿Cómo protege 91 mis datos?**
91 implementa medidas razonables como HTTPS, contraseñas hasheadas, Row-Level Security, tokens de acceso y rate-limiting contra abuso.

**¿91 garantiza seguridad total?**
No. Ningún sistema es 100% impenetrable. Por eso se recomienda usar una contraseña fuerte y única.

**¿91 guarda mi contraseña en texto plano?**
No. La contraseña se gestiona de forma hasheada por el proveedor de autenticación (Supabase Auth).
