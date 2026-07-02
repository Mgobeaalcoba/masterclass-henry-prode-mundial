# **Prode Mundial: Documentación Técnica**

## **1\. Resumen Ejecutivo**

El sistema "Prode Mundial" es una plataforma de automatización de extremo a extremo (E2E) que gestiona una competencia de predicciones deportivas. El sistema elimina la carga operativa manual mediante el uso de n8n como orquestador, Airtable como base de datos relacional y modelos de lenguaje (LLMs) para la personalización de la experiencia del usuario.

## **2\. Herramientas Integradas**

* **n8n (Cloud):** Servidor de ejecución, lógica de negocio y orquestación de flujos.  
* **Airtable:** Base de datos centralizada (Fixtures, Jugadores, Predicciones, Puntajes).  
* **API-Sports (Football):** Fuente de datos en tiempo real (Resultados, Partidos).  
* **Telegram Bot API:** Interfaz de comunicación con el usuario final.  
* **OpenAI / Gemini / Qwen:** Motores de IA para la personalización de mensajes.

## **3\. Arquitectura del Workflow**

### **A. Gestión de Fixture (Cron 19:00 hs)**

1. **Activación:** Disparador programado a las 19:00 hs.  
2. **Ingesta de Datos:** HTTP Request a la API de Football (parámetro date \= tomorrow).  
3. **Normalización:** Nodo Parsear Mañana (JavaScript) que estandariza el JSON de la API, generando un partido\_id único y normalizado.  
4. **Upsert:** Nodo de Airtable (Upsert Fixture Mañana) que garantiza que los partidos estén creados o actualizados sin duplicar registros mediante el fixture\_id.

### **B. Interacción Web (Webhook)**

1. **Frontend:** Se entrega un link dinámico vía Telegram con parámetros ?player=ID.  
2. **GET Request:** El nodo HTML Builder & Parser genera un formulario dinámico renderizado en tiempo real, inyectando el nombre del jugador (vía búsqueda en jugadores\_prode).  
3. **POST Request:** Al enviar el formulario, el nodo Parsear Formulario POST transforma datos planos en un mapa de objetos, insertando las predicciones en predicciones\_jugadores.

### **C. Cálculo de Puntaje y Notificación (Cron 23:00 hs)**

Este es el motor de lógica de negocio principal.

1. **Ingesta de Resultados:** Primero se consultan dos ventanas de resultados en API-Sports: **Buscar Resultados Ayer** (`date = yesterday`) y **Buscar Resultados Hoy** (`date = today`). Esto cubre partidos que pudieron finalizar en cualquiera de los dos días operativos.  
2. **Unificación y Normalización:** El nodo **Merge** combina ambas respuestas y **Parsear Resultados** filtra los partidos del Mundial, normaliza el JSON y estandariza campos como fixture\_id, equipos, goles, ganador y estado terminado.  
3. **Persistencia de Resultados:** El nodo **Upsert Fixture Ayer** actualiza la tabla de fixtures en Airtable usando fixture\_id como clave de deduplicación. Aunque el nombre operativo del nodo menciona "Ayer", en esta etapa persiste resultados provenientes tanto de ayer como de hoy.  
4. **Cálculo Lógico:** El nodo **Calcular Puntajes1** realiza el cruce entre las predicciones almacenadas y los resultados reales ya persistidos.

#### **Lógica de Puntuación**

Para cada partido, el sistema compara el ganador predicho contra el ganador real y, si coincide, valida si también acertó el marcador exacto:

* **3 puntos:** acierta ganador o empate y también los goles exactos de ambos equipos.  
* **1 punto:** acierta solo el ganador o empate.  
* **0 puntos:** cualquier otro caso.

5. **Persistencia:** El nodo Guardar Historial Diario realiza un upsert en la tabla puntaje\_jugadores.  
6. **IA Generativa:** Se utiliza un **AI Agent (LangChain)**.  
   * **System Prompt:** Define la personalidad ("presentador futbolero") y las reglas de modismos locales (Argentina, Colombia, Neutro).  
   * **User Prompt:** Recibe el contexto consolidado (nombre, país, puntos hoy, puntos totales, puesto en ranking).  
7. **Distribución:** El nodo Notificar por Telegram1 entrega el mensaje redactado por IA al telegram\_chat\_id correspondiente.

## **4\. Problemas Resueltos**

* **Desacoplamiento:** El administrador no necesita tocar el código para actualizar partidos.  
* **Integridad de Datos:** Gracias a los upsert basados en fixture\_id y id\_jugador, se eliminan riesgos de duplicación.  
* **UX (Experiencia de Usuario):** El usuario recibe una comunicación que parece humana y personalizada, aumentando significativamente la tasa de retorno al Prode.  
* **Robustez de Tipos:** El uso de la opción typecast: true en los nodos de Airtable resuelve errores de compatibilidad entre cadenas de texto y fechas.

## **5\. Recomendaciones de Mantenimiento**

* **Cuentas de API:** Monitorear el consumo de la API-Sports para evitar exceder los límites diarios.  
* **Prompts de IA:** El System Prompt es modular. Si deseas cambiar el estilo (hacerlo más serio o formal), puedes actualizar únicamente ese nodo sin afectar el flujo de datos.  
* **Monitoreo de Logs:** Ante errores de "chat not found", recordar que es necesario que el usuario inicie el bot en Telegram (comando /start).
