# Reflexión Laboratorio 1

- [Enlace a repositorio](https://github.com/JosueSay/nlp-tokenization-guardrails)

## Preguntas

**¿Qué falsos positivos encontró o podrían ocurrir?**

**Respuesta:**

Durante las pruebas observé que algunos patrones pueden clasificarse de forma incorrecta. En el texto **"Mi api key de Claude es sdk-123456789abc."**, el programa detectó únicamente la secuencia numérica **123456789** como un teléfono y la reemplazó por **[PHONE_REDACTED]**, cuando en realidad formaba parte de una API key. Esto demuestra que Regex puede encontrar coincidencias parciales que pertenecen a otro tipo de dato. Además, podrían ocurrir falsos positivos cuando un número largo corresponde a un código de producto, un identificador interno o un número de referencia que no representa información personal, pero el sistema igualmente lo clasifica como un dato sensible.

**¿Qué falsos negativos encontró o podrían ocurrir?**

**Respuesta:**

Los falsos negativos aparecen cuando la información sensible no coincide con el formato definido en las expresiones regulares. Por ejemplo, un correo escrito como **"ana arroba correo punto com"** o una API key con un formato diferente podrían pasar desapercibidos. En mis pruebas ocurrió algo similar: el sistema no identificó la API key completa, sino únicamente la parte numérica, por lo que parte de la credencial quedó expuesta. Esto demuestra que Regex depende completamente de los patrones definidos y cualquier variación puede hacer que información sensible no sea detectada.

**¿Qué información se pierde al redactar datos sensibles?**

**Respuesta:**

Al redactar datos sensibles también se pierde parte del contexto que ayuda al modelo a interpretar mejor una solicitud. Por ejemplo, el dominio de un correo electrónico puede indicar si la conversación es académica, empresarial o personal, mientras que una URL puede dar información sobre la página o documentación a la que hace referencia el usuario. De igual forma. El objetivo del guardrail es proteger la privacidad, pero siempre existe un equilibrio entre ocultar la información sensible y conservar suficiente contexto para que el modelo pueda razonar correctamente.

**¿Regex sería suficiente para proteger información de una empresa real?**

**Respuesta:**

No. Regex es una buena primera capa porque permite detectar patrones conocidos de forma rápida antes de enviar información al modelo. Sin embargo, solo reconoce los formatos para los que fue programado y no comprende el contexto. Si un dato sensible se escribe de otra forma o utiliza un formato diferente, puede no ser detectado. En una empresa real, donde existen muchos tipos de credenciales, identificadores y documentos internos.

**¿Qué otra capa de seguridad agregaría?**

**Respuesta:**

Además de Regex, agregaría reglas específicas para los formatos utilizados por la empresa, por ejemplo patrones para API keys, tokens, identificadores internos o credenciales propias de sus sistemas. También incorporaría una revisión de la información antes de enviarla al modelo y otra sobre la respuesta generada, para evitar que se envíen o devuelvan datos sensibles por error. De esta forma, Regex funcionaría como una primera capa de protección, complementada con validaciones adicionales dentro del pipeline de guardrails.
