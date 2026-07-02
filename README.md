# **⚽ Prode Mundial: Automatización E2E con IA**

Bienvenido al repositorio central de **Prode Mundial**. Este proyecto es una plataforma de automatización de extremo a extremo (E2E) que gestiona una competencia de predicciones deportivas, eliminando la carga operativa manual y personalizando la experiencia del usuario mediante Inteligencia Artificial.

Este proyecto fue desarrollado como caso de estudio para demostrar el potencial de las herramientas de **AI Automation**.

## **📁 Estructura del Repositorio**

* ./workflow.json : Contiene el archivo .json de exportación de n8n para importar el flujo de trabajo completo.  
* ./docu_tecnica: Contiene la Documentación Técnica del Sistema (prode\_mundial\_doc.md), detallando la arquitectura y lógica de negocio.  
* ./presentation: Espacio reservado para la presentación (PPT/Keynote) de la Masterclass.

## **🛠️ Tecnologías Aplicadas**

El sistema orquesta diversas herramientas modernas para lograr una solución escalable y robusta:

* **n8n (Cloud):** Servidor central de ejecución y lógica de orquestación.  
* **Airtable:** Base de datos relacional para la gestión de fixtures, jugadores y puntajes.  
* **API-Sports (Football):** Fuente de datos en tiempo real para resultados y partidos.  
* **Telegram Bot API:** Interfaz de usuario para notificaciones personalizadas.  
* **LangChain / AI Agents:** Motor de IA (modelos LLM) que analiza el ranking del usuario y genera mensajes personalizados con modismos locales.

## **🚀 Características Principales**

1. **Gestión Autónoma:** Sincronización automática de fixtures diarios mediante cron jobs.  
2. **UX Personalizada:** Formularios dinámicos vía webhooks identificando al usuario por Telegram.  
3. **Cálculo Inteligente:** Motor de lógica para puntuación (acierto ganador vs. resultado exacto).  
4. **Comunicación Generativa:** Notificaciones inteligentes que adaptan su "voz" según la región del usuario (Argentina, Colombia, Neutro).

## **🎓 Propósito Educativo**

Este repositorio sirve como material de apoyo para la **Masterclass de Marketing de Henry**. Su objetivo es ilustrar:

* Cómo integrar APIs REST con bases de datos no-code.  
* El uso de **AI Agents** para enriquecer la experiencia de usuario (UX).  
* El valor estratégico de la automatización en procesos de gamificación.

## **🤝 Contribuciones y Contacto**

Este proyecto es parte del ecosistema de formación de **Henry**. Si eres estudiante y deseas replicar este sistema, asegúrate de importar el .json en una instancia de n8n con acceso a las credenciales de la API-Sports y Airtable.

**Desarrollado por: Mariano Gobea Alcoba**