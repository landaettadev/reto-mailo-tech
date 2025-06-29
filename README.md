# 🤖 Sistema de Calificación Automática de Leads - Mailo Technologies

## 📋 Descripción del Proyecto

Sistema automatizado de calificación de leads de WhatsApp que integra **n8n**, **API REST**, **RAG (Recuperación Aumentada)** y **Google Sheets** para el seguimiento comercial.

### 🎯 Objetivos Cumplidos

✅ **Conexión con canal conversacional** (WhatsApp simulado)  
✅ **API REST de inventario** (400 productos con múltiples características)  
✅ **Motor de decisión para calificación** (Alto/Medio/Bajo)  
✅ **Captura de datos del lead** (Nombre, email, teléfono)  
✅ **Registro en Google Sheets** (Con segmentación y conversación)  
✅ **RAG/Embeddings** (Búsqueda semántica implementada)  
✅ **Agentes conversacionales** (Saludo, captura, interacción)  

## 🏗️ Arquitectura del Sistema

### 📁 Estructura del Proyecto

```
whatsapp-leads-n8n/
├── workflow_complete.json          # Workflow principal de n8n (9 nodos)
├── catalogo.json                   # Base de datos de 400 productos
├── generar_catalogo.js             # Generador de productos aleatorios
├── test_webhook_enhanced.js        # Script de pruebas automatizadas
├── routes.json                     # Configuración de rutas API
├── package.json                    # Dependencias y scripts
└── README.md                       # Documentación completa
```

### 🔄 Flujo de Trabajo (9 Nodos)

1. **01_Webhook_Entrada** - Recibe mensajes de WhatsApp
2. **02_Normalizar_Lead** - Procesa y estructura datos iniciales
3. **03_Agente_Saludo_Captura** - Saluda y captura nombre/email
4. **04_RAG_Embeddings** - Implementa búsqueda semántica
5. **05_Buscar_Producto_API** - Consulta inventario en tiempo real
6. **06_Agente_Interactivo_RAG** - Responde con productos relevantes
7. **07_Agente_Calificacion_Avanzada** - Califica lead con score 0-100
8. **08_Respuesta_Final_Calificada** - Envía respuesta personalizada
9. **09_Registro_Sheets_Enhanced** - Guarda en Google Sheets

## 🚀 Instalación y Configuración

### 1. Instalar Dependencias

```bash
npm install
npm install -g json-server
```

### 2. Generar Catálogo de Productos

```bash
npm run generate
```

Esto creará `catalogo.json` con 400 productos aleatorios.

### 3. Iniciar API REST

```bash
npm run api
```

- **URL**: http://localhost:3000/products
- **Búsqueda**: http://localhost:3000/products?nombre_like=camiseta

### 4. Configurar n8n

1. Importar `workflow_complete.json` en n8n
2. Configurar credenciales de Google Sheets
3. Activar el webhook

### 5. Ejecutar Pruebas

```bash
# Pruebas completas
npm run test:webhook

# Prueba individual
npm run test:single

# Ejecutar todo junto
npm run start:complete
```

## 🧠 Características Técnicas

### 🔍 RAG (Recuperación Aumentada)

- **Embeddings semánticos** para búsqueda inteligente
- **Categorización automática** por tipo de producto
- **Búsqueda por palabras clave** (ropa, colores, tallas, precios)
- **Recomendaciones contextuales** basadas en intereses

### 📊 Algoritmo de Calificación

**Score 0-100 basado en:**
- **Precio máximo** (30 puntos): ≥$200k=A, ≥$100k=B, <$100k=C
- **Precio promedio** (25 puntos): ≥$150k=25, ≥$80k=15, <$80k=5
- **Cantidad de productos** (20 puntos): ≥5=20, ≥3=15, <3=10
- **Engagement conversacional** (15 puntos): ≥6=15, ≥4=10, <4=5
- **Datos completos** (10 puntos): Nombre+Email=10, Solo nombre=5

### 🏷️ Segmentación de Leads

| Tipo | Precio | Score | Acción |
|------|--------|-------|--------|
| **A (VIP)** | ≥$200,000 | 70-100 | Contacto inmediato (30 min) |
| **B (Medio)** | $100,000-$200,000 | 40-69 | Contacto estándar (2 horas) |
| **C (Básico)** | <$100,000 | 1-39 | Seguimiento por email |

## 📱 Casos de Uso

### Ejemplo 1: Lead VIP
```
Usuario: "Hola"
Bot: "¡Hola! 👋 Soy tu asistente virtual de Mailo Technologies. ¿Podrías decirme tu nombre?"

Usuario: "María González"
Bot: "¡Muchas gracias María! 😊 Ahora necesito tu correo electrónico..."

Usuario: "maria.gonzalez@email.com"
Bot: "Perfecto María! 📧 Ahora cuéntame, ¿qué tipo de productos estás buscando?"

Usuario: "Busco chaquetas premium de alta calidad"
Bot: "¡Perfecto María! Encontré estos productos que pueden interesarte:
• Chaqueta Marrón - Talla XXL - $124,314
• Chaqueta Negra - Talla L - $189,567
• Chaqueta Azul - Talla M - $156,789

💡 Recomendación: Encontré 8 productos similares.

¿Te gustaría saber más detalles de alguno o buscas algo diferente?"

[Calificación: Tipo A, Score 85/100]
Bot: "¡Excelente María! 🎉 Basándome en tus intereses, eres un cliente VIP. Un asesor especializado te contactará en los próximos 30 minutos. Tu calificación: 85/100 (Alta prioridad)"
```

## 📊 Datos Almacenados en Google Sheets

| Campo | Descripción |
|-------|-------------|
| **Fecha_Registro** | Timestamp de la interacción |
| **Nombre** | Nombre del lead |
| **Teléfono** | Número de WhatsApp |
| **Email** | Correo electrónico |
| **Tipo_Lead** | A (VIP), B (Medio), C (Básico) |
| **Score_Calificacion** | Puntuación 0-100 |
| **Precio_Maximo** | Precio más alto de productos interesados |
| **Precio_Promedio** | Precio promedio de productos |
| **Productos_Interesados** | Cantidad de productos consultados |
| **Accion_Siguiente** | Próximo paso para el equipo comercial |
| **Conversacion_Completa** | JSON con toda la conversación |
| **Datos_Capturados** | Información adicional del lead |

## 🧪 Pruebas Automatizadas

El script `test_webhook_enhanced.js` incluye 4 casos de prueba:

1. **Lead VIP** - Productos premium (>$200k)
2. **Lead Medio** - Productos intermedios ($100k-$200k)
3. **Lead Básico** - Productos económicos (<$100k)
4. **Lead Múltiples Intereses** - Variedad de productos

### Ejecutar Pruebas

```bash
# Todas las pruebas
npm run test:webhook

# Prueba individual
node -e "
const { testSingleMessage } = require('./test_webhook_enhanced.js');
testSingleMessage('573001234567', 'Busco chaquetas premium');
"
```

## 🔧 Configuración Avanzada

### Personalizar Categorías RAG

Editar en `workflow_complete.json` (nodo 04_RAG_Embeddings):

```javascript
const palabrasClave = {
  'ropa': ['camiseta', 'pantalón', 'chaqueta', ...],
  'colores': ['rojo', 'azul', 'verde', ...],
  'tallas': ['xs', 's', 'm', 'l', 'xl', 'xxl'],
  'precios': ['barato', 'económico', 'caro', 'premium', ...]
};
```

### Ajustar Algoritmo de Calificación

Modificar en `workflow_complete.json` (nodo 07_Agente_Calificacion_Avanzada):

```javascript
// Calificación por precio máximo
if (precioMax >= 200000) {
  score += 30;  // Ajustar peso
  tipoLead = 'A';
}
```

## 📈 Métricas y KPIs

- **Tasa de conversión** por segmento
- **Tiempo de respuesta** del bot
- **Precisión de calificación** vs resultados comerciales
- **Engagement** por tipo de lead
- **Efectividad de RAG** en búsquedas

## 🚀 Próximas Mejoras

- [ ] Integración con Pinecone/Supabase Vector
- [ ] Análisis de sentimiento en conversaciones
- [ ] Machine Learning para calificación
- [ ] Integración con CRM real
- [ ] Dashboard de métricas en tiempo real

## 📞 Soporte

Para el reto técnico de **Mailo Technologies**:
- **Email**: Hola@mailotech.com
- **Fecha límite**: 28 de Junio 2025, 6:00 PM

---

**Desarrollado para el Reto Técnico de Automatización de Calificación de Leads en n8n** 🎯 