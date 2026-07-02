# **⚽ Prode Mundial: Automatización E2E con IA**

Bienvenido al repositorio central de **Prode Mundial**. Este proyecto es una plataforma de automatización de extremo a extremo (E2E) que gestiona una competencia de predicciones deportivas, eliminando la carga operativa manual y personalizando la experiencia del usuario mediante Inteligencia Artificial.

Este proyecto fue desarrollado como caso de estudio para demostrar el potencial de las herramientas de **AI Automation**.

## **📁 Estructura del Repositorio**

* `workflow.json`: Exportación del workflow de n8n para importar el flujo completo.  
* `docu_tecnica.md`: Documentación técnica del sistema, con arquitectura, lógica de negocio y detalle de nodos principales.  
* `presentation.pdf`: Presentación de apoyo para la masterclass.

## **🛠️ Tecnologías Aplicadas**

El sistema orquesta diversas herramientas modernas para lograr una solución escalable y robusta:

* **n8n (Cloud):** Servidor central de ejecución y lógica de orquestación.  
* **Airtable:** Base de datos relacional para la gestión de fixtures, jugadores, predicciones y puntajes.  
* **API-Sports (Football):** Fuente de datos en tiempo real para resultados y partidos.  
* **Telegram Bot API:** Interfaz de usuario para notificaciones personalizadas.  
* **LangChain / AI Agents:** Motor de IA que analiza el ranking del usuario y genera mensajes personalizados con modismos locales.  
* **Modelos LLM (Gemini / OpenAI):** Modelos conectados al agente para redactar las notificaciones finales.

## **🚀 Características Principales**

1. **Gestión Autónoma:** A las 19:00 hs, el workflow consulta los partidos de mañana, normaliza la respuesta de API-Sports y actualiza Airtable sin duplicar fixtures.  
2. **UX Personalizada:** El usuario recibe por Telegram un link con `?player=ID`; el webhook GET renderiza un formulario dinámico con los partidos activos y el webhook POST registra sus predicciones.  
3. **Cálculo Inteligente:** A las 23:00 hs, el sistema consulta primero resultados de ayer y de hoy, los unifica, actualiza fixtures terminados y calcula puntajes por jugador.  
4. **Comunicación Generativa:** El AI Agent genera notificaciones que adaptan su "voz" según la región del usuario (Argentina, Colombia o tono neutro).

## **🎓 Propósito Educativo**

Este repositorio sirve como material de apoyo para la **Masterclass de Marketing de Henry**. Su objetivo es ilustrar:

* Cómo integrar APIs REST con bases de datos no-code.  
* El uso de **AI Agents** para enriquecer la experiencia de usuario (UX).  
* El valor estratégico de la automatización en procesos de gamificación.

## **🤝 Contribuciones y Contacto**

Este proyecto es parte del ecosistema de formación de **Henry**. Si eres estudiante y deseas replicar este sistema, importá `workflow.json` en una instancia de n8n y configurá las credenciales necesarias para API-Sports, Airtable, Telegram y los modelos de IA.

**Desarrollado por: Mariano Gobea Alcoba**
