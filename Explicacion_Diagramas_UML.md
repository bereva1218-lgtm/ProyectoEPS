# 📊 DIAGRAMAS UML - PROYECTO HEALTHCORP EPS

---

## 📋 TABLA DE CONTENIDOS
1. [Diagrama de Clases UML](#diagrama-de-clases-uml)
2. [Diagrama de Componentes](#diagrama-de-componentes)
3. [Explicación Detallada](#explicación-detallada)
4. [Cómo Usar Los Archivos PlantUML](#cómo-usar-los-archivos-plantuml)

---

## 🎯 DIAGRAMA DE CLASES UML

### Descripción General
El diagrama de clases UML de HealthCorp contiene **23 clases principales** organizadas en las siguientes capas:

#### 📦 **Capa de Usuarios (Herencia)**
```
Usuario (Clase Abstracta)
├── Paciente
├── Médico
├── Administrativo
└── OperadorFarmacéutico
```

Todas heredan de la clase abstracta `Usuario` que contiene:
- Atributos comunes: idUsuario, nombre, email, teléfono, contraseña (hash)
- Métodos básicos: login(), logout(), actualizarPerfil(), cambiarContraseña()

---

### 📝 **Clases Principales por Módulo**

#### 1️⃣ **MÓDULO DE USUARIOS**

**Clase: Paciente**
- Hereda de: Usuario
- Atributos específicos:
  - `numeroAfiliacion`: Identificador de afiliación a EPS
  - `tipoDocumento`, `numeroDocumento`: Documento de identidad
  - `alergias[]`, `condicionesPrevias[]`: Información médica crítica
  - `eps`, `aseguradora`: Entidades aseguradoras
- Métodos:
  - `agendarCita()`: Crear nueva cita
  - `cancelarCita()`, `reprogramarCita()`: Gestionar citas
  - `consultarHistorial()`: Acceder a su historial médico
  - `solicitarAutorizacion()`: Solicitar procedimientos/medicinas
  - `accederTelemedicina()`: Participar en video consulta
  - `activarEmergencia()`: Botón de pánico con GPS
  - `verRecetas()`: Consultar fórmulas médicas

**Clase: Médico**
- Hereda de: Usuario
- Atributos específicos:
  - `numeroMatricula`: Registro profesional
  - `especialidad`: Campo médico (cardiología, pediatría, etc.)
  - `horarioAtencion[]`: Disponibilidad
  - `calificacionPromedio`, `totalPacientes`: Métricas
- Métodos:
  - `consultarHistorialPaciente()`: Acceso a datos médicos
  - `registrarDiagnóstico()`: Documentar encuentro clínico
  - `generarFórmula()`: Crear receta digital
  - `emitirIncapacidad()`: Generar baja médica
  - `gestionarAgenda()`: Administrar disponibilidad

**Clase: Administrativo**
- Hereda de: Usuario
- Atributos específicos:
  - `departamento`: Área de trabajo (autorización, afiliación, etc.)
  - `permisos[]`: Acciones autorizadas
  - `nivel`: Jerárquico (gerencial, operativo)
- Métodos:
  - `aprobarSolicitud()`: Autorizar procedimientos
  - `gestionarTrámites()`: Administración de procesos
  - `validarAfiliación()`: Verificar cobertura
  - `registrarClínicaIPS()`: Onboarding de entidades
  - `generarReporte()`: Análisis y estadísticas

**Clase: OperadorFarmacéutico**
- Hereda de: Usuario
- Atributos específicos:
  - `farmaciaAfiliada`: Establecimiento farmacéutico
  - `inventario[]`: Medicamentos disponibles
- Métodos:
  - `consultarFórmula()`: Validar QR/código de barras
  - `verificarMedicinas()`: Verificar disponibilidad
  - `prepararMedicamento()`: Dispensación
  - `notificarDisponibilidad()`: Comunicación con paciente
  - `registrarEntrega()`: Confirmación de entrega

---

#### 2️⃣ **MÓDULO DE CITAS**

**Clase: Cita**
- Relación: Vincula Paciente con Médico
- Atributos:
  - `fechaHora`: Programación específica
  - `motivo`: Razón de la consulta
  - `lugar`: Consultorio físico o telemedicina
  - `estado`: "programada" → "completada" o "cancelada"
  - `duracionEstimada`: Tiempo asignado
- Métodos:
  - `confirmar()`: Validar asistencia
  - `cancelar()`: Con registro de motivo
  - `reprogramar()`: Cambiar fecha/hora
  - `registrarAsistencia()`: Marcar que se realizó
  - `obtenerDetalles()`: DTO para mostrar en UI

**Clase: Horario**
- Valor object para disponibilidad médica
- Atributos:
  - `diaSemana`, `horaInicio`, `horaFin`
  - `capacidadCitas`: Cuántas citas ese día

---

#### 3️⃣ **MÓDULO DE HISTORIAL CLÍNICO**

**Clase: HistorialClínico**
- Centro del sistema: Almacena todo el expediente médico
- Atributos:
  - `paciente`: Propietario del historial
  - `diagnósticos[]`, `fórmulas[]`, `incapacidades[]`: Registro histórico
  - `vacunas[]`, `alergias[]`: Información de salud preventiva
  - `fechaÚltimaActualización`: Control de cambios
- Métodos:
  - `agregarDiagnóstico()`: Registrar nuevo diagnóstico
  - `agregarFórmula()`: Asociar receta a historial
  - `descargarPDF()`: Exportación segura
  - `obtenerResumen()`: Vista rápida de estado de salud

**Clase: Diagnóstico**
- Relación con: Médico, HistorialClínico
- Atributos:
  - `codigoCIE10`: Clasificación internacional de enfermedades
  - `descripción`: Detalles del diagnóstico
  - `severidad`: "leve", "moderada", "severa"
  - `fecha`, `médico`: Trazabilidad
- Métodos:
  - `validarCódigo()`: Verificar código CIE-10 válido

**Clase: Vacuna**
- Registro de inmunizaciones
- Atributos: nombre, fecha, lote, profesional, centro de vacunación

**Clase: Alergia**
- Registro de reacciones adversas
- Atributos: sustancia, reacción, severidad, fechaDetección

---

#### 4️⃣ **MÓDULO DE FÓRMULAS MÉDICAS**

**Clase: FórmulaMédica**
- Relación con: Médico, Paciente, Medicamento
- Atributos:
  - `medicamentos[]`, `cantidades[]`: Prescripción detallada
  - `instrucciones`: "2 veces al día con comida"
  - `frecuencia`: Duración del tratamiento
  - `vigencia`: Hasta cuándo es válida
  - `codigoQR`, `codigoBarras`: Para farmacia
  - `firmaDigital`: Garantiza autenticidad
  - `estadoDispensado`: Boolean de entrega
- Métodos:
  - `generarQR()`: Crear código para farmacia
  - `validarVigencia()`: Verificar que no haya caducado
  - `marcarComoDispensada()`: Registro de entrega
  - `obtenerDetalles()`: DTO para consultas

**Clase: Medicamento**
- Catálogo de fármacos disponibles
- Atributos:
  - `nombre`, `principioActivo`, `concentración`
  - `contraindicaciones[]`, `efectosSecundarios[]`: Información crítica
- Métodos:
  - `validarDisponibilidad()`: Verificar stock

---

#### 5️⃣ **MÓDULO DE AUTORIZACIONES**

**Clase: Autorización**
- Relación con: Paciente, Administrativo
- Atributos:
  - `tipo`: "procedimiento", "medicamento", "internación"
  - `descripción`: Qué se autoriza
  - `estado`: Flujo: "pendiente" → "aprobada" o "rechazada"
  - `motivo`: En caso de rechazo
  - `timestamps`: Auditoría de tiempos
- Métodos:
  - `solicitar()`: Inicia proceso
  - `aprobar()`: Administrativo aprueba
  - `rechazar()`: Con justificación
  - `obtenerEstado()`: Consulta actual

---

#### 6️⃣ **MÓDULO DE INCAPACIDADES**

**Clase: Incapacidad**
- Relación con: Paciente, Médico
- Atributos:
  - `diasAutorizados`: Duración de la baja
  - `fechaInicio`, `fechaFin`: Vigencia
  - `motivo`: Causa de la incapacidad
  - `documentoPDF`: Generado automáticamente con firma digital
- Métodos:
  - `generarDocumento()`: Crear PDF legal
  - `enviarAlPaciente()`: Notificación automática

---

#### 7️⃣ **MÓDULO DE TELEMEDICINA**

**Clase: Telemedicina**
- Relación con: Cita, Paciente, Médico
- Atributos:
  - `enlaceVideoconferencia`: URL de sesión
  - `estado`: "conectada" o "finalizada"
  - `fechaInicio`, `fechaFin`: Duración de sesión
- Métodos:
  - `iniciarSesión()`: Conectar video
  - `finalizarSesión()`: Cerrar y guardar registros
  - `enviarMensaje()`: Chat en tiempo real
  - `compartirArchivo()`: Enviar exámenes, imágenes
  - `registrarNotas()`: Diagnóstico post-consulta

**Clase: Chat**
- Comunicación asincrónica
- Atributos: participantes[], mensajes[], historial
- Métodos: enviarMensaje(), compartirImagen(), obtenerHistorial()

**Clase: Mensaje**
- Atributos: remitente, contenido, tipo (texto/imagen/archivo), timestamp, leído

---

#### 8️⃣ **MÓDULO DE EMERGENCIAS**

**Clase: SOS**
- Botón de pánico del paciente
- Atributos:
  - `ubicacion`: Coordenada GPS en tiempo real
  - `estado`: "activo", "atendido", "cancelado"
  - `ambulancia`: Asignada automáticamente
  - `familiaresNotificados[]`: Notificación inmediata
  - `historialMédicoAccedido`: Acceso rápido a información crítica
- Métodos:
  - `activarEmergencia()`: Dispara alerta
  - `notificarAmbulancia()`: Envío geolocalizado
  - `notificarFamiliares()`: Comunicación de emergencia
  - `deactivarEmergencia()`: Cancelar si se resolvió

**Clase: Ambulancia**
- Gestión de recursos de emergencia
- Atributos:
  - `número`: Identificación del vehículo
  - `ubicacion`: GPS en tiempo real
  - `estado`: "disponible", "en camino", "ocupada"
  - `paramédicos[]`, `equipamiento[]`: Recursos

**Clase: Coordenada**
- Valor object para geolocalización
- Atributos: latitud, longitud, precisión

---

#### 9️⃣ **MÓDULO DE NOTIFICACIONES**

**Clase: Notificación**
- Sistema de alertas multicanal
- Atributos:
  - `tipo`: "cita", "fórmula", "autorización", "recordatorio"
  - `canal`: "email", "SMS", "push mobile"
  - `estado`: "enviada", "entregada", "leída"
  - `enlace`: URL para acciones rápidas
- Métodos:
  - `enviar()`: Despachar notificación
  - `marcarComoLeída()`: Registro de interacción

---

#### 🔟 **MÓDULO DE REPORTES Y AUDITORÍA**

**Clase: Reporte**
- Análisis y estadísticas del sistema
- Atributos:
  - `tipo`: "estadístico", "auditoría", "financiero"
  - `datos[]`: Resultados del análisis
  - `formato`: "PDF", "Excel", "CSV"
  - `periodo`: Rango de fechas
- Métodos:
  - `generar()`: Ejecutar análisis
  - `descargar()`: Exportar resultados

**Clase: AuditoríaLog**
- Trazabilidad de cambios en el sistema
- Atributos:
  - `usuario`, `acción`, `entidad`: Quién, qué, dónde
  - `cambiosRealizados`: Detalle de modificaciones
  - `timestamp`, `ipAddress`: Cuándo y desde dónde
  - `estado`: Exitoso o fallido
- Usado para cumplimiento normativo HIPAA y leyes de datos

---

### 🔗 **Relaciones Principales**

| Relación | Tipo | Cardinalidad | Significado |
|----------|------|--------------|------------|
| Paciente → Cita | Composición | 1 : 0..* | Un paciente tiene múltiples citas |
| Cita → HistorialClínico | Agregación | * : 1 | Todas las citas se registran en el historial |
| Médico → Diagnóstico | Composición | 1 : 0..* | Un médico registra diagnósticos |
| FórmulaMédica → Medicamento | Asociación | * : * | Una fórmula contiene múltiples medicinas |
| Paciente → Autorización | Agregación | 1 : 0..* | Un paciente solicita autorizaciones |
| SOS → Ambulancia | Asociación | 1 : 0..1 | Un SOS asigna una ambulancia |

---

## 🏗️ DIAGRAMA DE COMPONENTES

### Descripción General
El diagrama de componentes muestra la **arquitectura técnica** de HealthCorp en 10 capas:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. Capa de Cliente (Web & Mobile)                           │
├─────────────────────────────────────────────────────────────┤
│ 2. Capa Frontend (UI & Presentación)                        │
├─────────────────────────────────────────────────────────────┤
│ 3. API Gateway & Load Balancer                              │
├─────────────────────────────────────────────────────────────┤
│ 4. Microservicios Backend                                   │
├─────────────────────────────────────────────────────────────┤
│ 5. Servicios Transversales                                  │
├─────────────────────────────────────────────────────────────┤
│ 6. Integraciones Externas                                   │
├─────────────────────────────────────────────────────────────┤
│ 7. Base de Datos                                            │
├─────────────────────────────────────────────────────────────┤
│ 8. Caché & Sesiones                                         │
├─────────────────────────────────────────────────────────────┤
│ 9. Almacenamiento en Nube                                   │
├─────────────────────────────────────────────────────────────┤
│ 10. Seguridad, Monitoreo e Infraestructura                  │
└─────────────────────────────────────────────────────────────┘
```

---

### 📱 **CAPA 1: CLIENTE**

**Componentes:**
- `Aplicación Web`: Acceso desde navegador (React/Angular/Vue)
- `Aplicación Mobile`: iOS/Android (React Native/Flutter)

**Responsabilidades:**
- Interfaz de usuario responsiva
- Captura de entrada del usuario
- Validación de formularios (lado cliente)

---

### 🎨 **CAPA 2: FRONTEND (PRESENTACIÓN)**

**Componentes:**
- `Web UI`: Componentes React/Angular para escritorio
- `Mobile UI`: Componentes optimizados para celular
- `Notificaciones UI`: Módulo de alertas visuales

**Responsabilidades:**
- Renderizado de interfaces
- Gestión de estado (Redux/Context/MobX)
- Interacciones de usuario

---

### 🚪 **CAPA 3: API GATEWAY & LOAD BALANCER**

**Componentes:**
- `API Gateway` (Kong/AWS API Gateway/Azure API Management)
  - Enrutamiento inteligente de requests
  - Rate limiting (protección contra ataques)
  - Autenticación inicial
  - Transformación de datos
  
- `Load Balancer` (NGINX/HAProxy)
  - Distribución de carga entre servidores
  - Failover automático
  - Balanceo por peso o salud del servicio

**Responsabilidades:**
- Punto de entrada único para toda la API
- Validación de tokens JWT
- Enrutamiento a microservicios apropiadosactical

---

### 🔧 **CAPA 4: MICROSERVICIOS BACKEND**

**9 Microservicios Independientes:**

#### 1. **Servicio de Autenticación** (AuthService)
```
Responsabilidades:
- Validar credenciales de usuario
- Generar tokens JWT
- Manejo de refresh tokens
- Integración con 2FA
- Logout y revocación de sesiones
- Soporte OAuth2 (Google, Microsoft)
```

#### 2. **Servicio de Usuarios** (UserService)
```
Responsabilidades:
- Gestión de perfiles de usuario
- Actualización de datos personales
- Cambio de contraseña
- Gestión de roles y permisos
- Onboarding de nuevos usuarios
```

#### 3. **Servicio de Citas** (CitaService)
```
Responsabilidades:
- Crear, modificar, cancelar citas
- Verificar disponibilidad de médicos
- Enviar notificaciones de citas
- Generar reportes de utilización
- Validar no overlapping de citas
- Integración con calendario médico
```

#### 4. **Servicio Médico** (MedicoService)
```
Responsabilidades:
- Gestionar información de médicos
- Horarios y disponibilidad
- Calificaciones y reseñas
- Especialidades
- Credenciales y certificaciones
- Estadísticas de atención
```

#### 5. **Servicio de Historial Clínico** (HistorialService)
```
Responsabilidades:
- Almacenamiento seguro de expedientes
- Acceso controlado por roles
- Registro de cambios (auditoría)
- Exportación a PDF
- Búsqueda full-text en diagnósticos
- Cumplimiento HIPAA
```

#### 6. **Servicio de Fórmulas** (FormulaService)
```
Responsabilidades:
- Generación de recetas digitales
- Firma digital de fórmulas
- Generación de QR/códigos de barras
- Validación de vigencia
- Comunicación con farmacias
- Alertas de vencimiento
- Seguimiento de dispensación
```

#### 7. **Servicio de Autorizaciones** (AutorizacionService)
```
Responsabilidades:
- Validar cobertura de paciente
- Solicitud y aprobación de procedimientos
- Verificación de vigencia de pólizas
- Comunicación con aseguradoras
- Generación de autorizaciones formales
- Reportes de negaciones
```

#### 8. **Servicio de Emergencias** (EmergenciaService)
```
Responsabilidades:
- Activación de botón SOS
- Obtención de ubicación GPS
- Dispatch de ambulancias
- Notificación a familiaresNotificationService
- Acceso rápido a información médica crítica
- Alertas a hospitales cercanos
- Registro de llamadas de emergencia
```

#### 9. **Servicio de Telemedicina** (TelemedicinService)
```
Responsabilidades:
- Iniciar sesiones de video
- Gestión de conexiones WebRTC
- Chat en tiempo real
- Compartición de archivos/imágenes
- Grabación de sesiones (con consentimiento)
- Terminación segura de sesiones
- Integración con proveedores de videoconferencia
```

---

### 🔄 **CAPA 5: SERVICIOS TRANSVERSALES**

Servicios compartidos por múltiples microservicios:

#### 1. **Servicio de Notificaciones** (NotificationService)
```
Canales soportados:
- Email (SMTP/SendGrid)
- SMS (Twilio/Nexmo)
- Push notifications (Firebase Cloud Messaging)
- In-app notifications

Tipos de notificaciones:
- Recordatorios de citas
- Alertas de fórmulas listas
- Confirmación de autorizaciones
- Emergencias y alertas críticas
- Cambios de estado

Características:
- Plantillas personalizables
- Scheduling de envíos
- Reintento automático
- Tracking de entrega
- Preferencias del usuario
```

#### 2. **Servicio de Reportes** (ReportService)
```
Tipos de reportes:
- Estadísticos: Citas completadas, pacientes activos
- Financieros: Ingresos, utilización
- Auditoría: Accesos, cambios, incidencias
- Clínicos: Diagnósticos más frecuentes, medicinas

Formatos de exportación:
- PDF
- Excel
- CSV
- PowerBI

Características:
- Reportes programados
- Exportación masiva
- Gráficos interactivos
```

#### 3. **Servicio de Auditoría** (AuditService)
```
Responsabilidades:
- Registrar TODOS los cambios en la BD
- Quién, qué, cuándo, dónde, por qué
- Cumplimiento regulatorio
- Análisis de seguridad
- Detección de anomalías
```

#### 4. **Servicio de Autenticación 2FA** (TwoFAService)
```
Métodos soportados:
- TOTP (Time-based One-Time Password) - Google Authenticator
- SMS OTP
- Email OTP
- Claves de respaldo

Responsabilidades:
- Generar códigos
- Validar intentos
- Registrar dispositivos
- Recuperación de cuenta
```

---

### 🌐 **CAPA 6: INTEGRACIONES EXTERNAS**

#### 1. **Servicio de Videoconferencia** (VideoService)
```
Proveedores:
- Zoom API
- Jitsi Meet (open source)
- AWS Chime
- Google Meet API

Funcionalidades:
- Crear salas de reunión
- Gestionar permisos
- Grabación de sesiones
- Soporte para multiple participantes
```

#### 2. **Servicio de Geolocalización** (GeoService)
```
Proveedores:
- Google Maps API
- Mapbox
- OpenStreetMap

Funcionalidades:
- Obtener coordenadas GPS
- Buscar ambulancias cercanas
- Calcular ruta más rápida
- Mapeo de clínicas e IPS
```

#### 3. **Servicio de Comunicación** (CommunicationService)
```
Proveedores SMS/Email:
- Twilio (SMS)
- AWS SES (Email)
- SendGrid (Email)
- Nexmo (SMS)

Responsabilidades:
- Envío de SMS
- Envío de Email
- Templates dinámicos
- Logs de entrega
```

#### 4. **Proveedor de Farmacia** (PharmacyProvider)
```
Integraciones:
- APIs de farmacias locales
- Verificación de stock
- Validación de precios
- Confirmación de entrega

Responsabilidades:
- Sincronizar fórmulas
- Consultar disponibilidad
- Generar órdenes
```

---

### 🗄️ **CAPA 7: BASE DE DATOS**

Arquitectura **políglotica** (múltiples bases de datos especializadas):

#### 1. **BD Usuarios** (PostgreSQL/MySQL)
```sql
Tablas principales:
- usuarios (id, email, nombreCompleto, contraseña_hash, rol)
- permisos (id_usuario, id_permiso)
- sesiones (id_usuario, token_jwt, fecha_expiracion)
```

#### 2. **BD Citas** (PostgreSQL)
```sql
Tablas principales:
- citas (id, id_paciente, id_medico, fechaHora, estado)
- horarios_medicos (id_medico, dia_semana, hora_inicio, hora_fin)
- recordatorios (id_cita, tipo_notificacion, fecha_envio)
```

#### 3. **BD Historial Clínico** (MongoDB - DocumentoDB)
```json
Colecciones:
- expedientes: {
    id_paciente,
    diagnosticos: [
      { codigoCIE10, descripcion, fecha, medico }
    ],
    formulas: [ ... ],
    vacunas: [ ... ],
    alergias: [ ... ]
  }
```

#### 4. **BD Fórmulas** (PostgreSQL)
```sql
Tablas:
- formulas (id, id_paciente, id_medico, fecha_creacion, vigencia)
- formula_medicamentos (id_formula, id_medicamento, cantidad, instrucciones)
- codigosQR (id_formula, qr_data, codigo_barras)
```

#### 5. **BD Autorizaciones** (PostgreSQL)
```sql
Tablas:
- autorizaciones (id, id_paciente, tipo, estado, fecha_solicitud)
- aprobaciones (id_autorizacion, id_administrativo, fecha, motivo)
```

#### 6. **BD Auditoría & Logs** (Elasticsearch/Splunk)
```json
Documentos:
{
  timestamp: ISO8601,
  usuario: { id, nombre, rol },
  accion: "CREATE | UPDATE | DELETE | READ",
  entidad: "Cita | Paciente | Medicamento",
  id_entidad: "...",
  cambios: { antes: {}, despues: {} },
  ipAddress: "192.168.x.x",
  estado: "exitoso | error"
}
```

---

### ⚡ **CAPA 8: CACHÉ & SESIONES**

#### **Redis Cache**
```
Usos:
- Cache de médicos disponibles (TTL 30min)
- Caché de medicamentos frecuentes (TTL 1h)
- Sesiones de usuario (TTL 24h)
- Contadores de rate limiting
- Caché de autorizaciones aprobadas

Estrategia:
- Cache-aside (lazy loading)
- Invalidación por patrones
```

#### **Session Manager**
```
Responsabilidades:
- Almacenar datos de sesión activa
- Sincronización entre instancias
- Expiración automática
- Seguridad: CSRF tokens
```

---

### ☁️ **CAPA 9: ALMACENAMIENTO EN NUBE**

#### **Cloud Storage (AWS S3 / Azure Blob Storage)**
```
Archivos almacenados:
- PDFs de historial clínico
- Reportes generados
- Imágenes médicas (rayos X, etc.)
- Documentos legales (incapacidades)

Características:
- Replicación geográfica
- Versionado
- Lifecycle policies (borrar después de X días)
- Encriptación en reposo
```

#### **PDFStorage**
```
Documentos específicos:
- Fórmulas con firma digital
- Reportes de auditoría
- Incapacidades
- Autorizaciones formales
```

#### **ImageStorage**
```
Imágenes médicas:
- Radiografías
- Resonancias
- Fotos de consulta
- Scans de documentos
```

---

### 🔐 **CAPA 10: SEGURIDAD, MONITOREO E INFRAESTRUCTURA**

#### **Seguridad**

**SSL/TLS**
```
- Certificados letsencrypt/DigiCert
- TLS 1.3 mínimo
- Perfect forward secrecy (PFS)
- HSTS headers
```

**Encriptación de Datos**
```
- En tránsito: TLS 1.3
- En reposo: AES-256
- Claves gestionadas por AWS KMS / Azure Key Vault
- Encrypting sensitive fields: contraseñas, números de documento
```

**JWT Manager**
```
- Emisión de tokens
- Rotación de keys
- Validación de firma
- Blacklist de tokens revocados
```

**CORS & Rate Limiting**
```
- Whitelist de orígenes
- Rate limiting por IP/usuario
- DDoS protection
```

---

#### **Monitoreo & Logging**

**Kibana/ELK Stack** (Elasticsearch, Logstash, Kibana)
```
Logs capturados:
- Accesos a API
- Errores de aplicación
- Cambios en BD
- Intentos de autenticación fallidos

Análisis:
- Búsqueda full-text
- Dashboards en tiempo real
- Alertas automáticas
```

**Prometheus/Grafana**
```
Métricas:
- CPU, memoria, disco
- Latencia de APIs
- Tasa de errores
- Throughput

Visualización:
- Dashboards
- Alertas de umbral
- Histórico de 2 años
```

**Alert Manager**
```
Alertas:
- Servicio caído → Notification a equipo DevOps
- Error rate > 5% → Escalar a SRE
- Latencia p99 > 2s → Investigar
- Espacio disco < 10% → Limpiar logs
```

---

#### **Infraestructura Cloud**

**Orquestación (Kubernetes)**
```
- Despliegue de microservicios en contenedores
- Auto-healing: reiniciar pods fallidos
- Rolling updates: cero downtime
- Network policies: aislamiento entre servicios
```

**Auto-scaling**
```
- Horizontal Pod Autoscaler (HPA)
- Vertical Pod Autoscaler (VPA)
- Cluster autoscaler: agregar/quitar nodos
- Scale triggers: CPU > 70%, memoria > 80%
```

**Backup Automático**
```
- Snapshots diarios de BD
- Replicación a región secundaria
- RTO (Recovery Time Objective): < 1 hora
- RPO (Recovery Point Objective): < 15 minutos
- Pruebas mensuales de restauración
```

---

### 🔄 **FLUJO DE DATOS CRÍTICOS**

#### **1. Agendar una Cita**
```
Paciente (App Mobile) 
  ↓ POST /citas
API Gateway ← validar JWT
  ↓
CitaService
  ↓ verifyAvailability()
MedicoService ← consultar horarios
  ↓ query to Cache/BD
BD Citas ← registrar cita
  ↓
NotificationService ← enviar recordatorio
CommunicationService ← enviar SMS/Email
  ↓
Paciente recibe confirmación
```

#### **2. Generar Fórmula Médica**
```
Médico (Web App)
  ↓ POST /formulas
API Gateway ← validar token
  ↓
FormulaService
  ↓ generateQR()
FormulaService ← crear código QR
  ↓ save to BD + CloudStorage
HistorialService ← agregar a expediente
  ↓
NotificationService ← alertar farmacia
  ↓
Operador Farmacéutico ve en su app
```

#### **3. Activar Emergencia**
```
Paciente presiona botón SOS (GPS activo)
  ↓
EmergenciaService ← recibe ubicación
  ↓ query nearby ambulances
GeoService + BD Ambulancias
  ↓
Dispatch a ambulancia más cercana
  ↓ notify family + hospital
CommunicationService + NotificationService
  ↓
HistorialService ← acceso rápido a alergias/medicinas
```

---

## 📚 **EXPLICACIÓN DETALLADA**

### 🎓 **Conceptos Clave del Diagrama de Clases**

**1. Herencia**
```
La relación Usuario (padre) → Paciente/Médico/etc. (hijos)
permite código reutilizable y polimorfismo.

Beneficio: Cambios a Usuario se heredan automáticamente
```

**2. Agregación vs Composición**
```
Agregación (◇): Relación "tiene un" débil
  HistorialClínico contiene Diagnósticos
  Incluso si se borra el historial, los diagnósticos existen en DB

Composición (◆): Relación "tiene un" fuerte
  Cita compuesta por Paciente + Médico + DateTime
  Si se borra la cita, la composición desaparece
```

**3. Asociación Muchos-a-Muchos**
```
FórmulaMédica → Medicamento es * : *
Una fórmula tiene varios medicamentos
Un medicamento aparece en varias fórmulas

Implementación: tabla intermedia formula_medicamentos
```

---

### 🛡️ **Seguridad por Diseño**

**1. Acceso Controlado por Roles (RBAC)**
```
Paciente NO puede ver historial de otro paciente
Médico solo ve pacientes que le fueron asignados
Administrativo solo ve lo de su departamento

Implementación: 
- Administrativo tiene attribute permisos[]
- Middleware en API Gateway valida permisos
- HistorialService hace double-check por usuario
```

**2. Encriptación de Datos Sensibles**
```
- Contraseña: bcrypt o Argon2
- Número de documento: AES-256
- Número de afiliación: AES-256
- Información médica: TLS en tránsito, AES en reposo
```

**3. Auditoría Completa (AuditoríaLog)**
```
Cada cambio es registrado inmutablemente:
- Quién cambió (Usuario)
- Qué cambió (Entidad + ID + campos)
- Cuándo (timestamp)
- Dónde (IP)
- Por qué (motivo, si aplica)

Permite:
- Detectar acceso no autorizado
- Cumplir regulaciones (HIPAA, GDPR)
- Investigar incidentes
```

---

### 📈 **Escalabilidad del Sistema**

**1. Microservicios Desacoplados**
```
Cada servicio:
- Tiene su propia BD
- Escala independientemente
- Falla sin afectar otros

Ejemplo: Si CitaService recibe pico de carga,
escalamos solo esa instancia sin afectar FormulaService
```

**2. Caché Distribuido (Redis)**
```
Reduce carga en BD principal:
- Resultados de consultas frecuentes en memoria
- TTL automático para actualizar datos
- Cache invalidation patterns
```

**3. Almacenamiento Separado por Tipo**
```
- SQL (PostgreSQL): datos transaccionales, ACID
- NoSQL (MongoDB): documentos médicos, flexibilidad
- ElasticSearch: logs, búsquedas full-text
- S3: archivos grandes, backups

Cada uno optimizado para su caso de uso
```

---

## 💻 **CÓMO USAR LOS ARCHIVOS PLANTUML**

### 🔧 **Opción 1: Usar PlantUML Online**

1. Ir a: **https://www.plantuml.com/plantuml/uml/**
2. Copiar todo el contenido del archivo `.puml`
3. Pegar en el editor
4. Export → PNG/SVG/PDF

### 🔧 **Opción 2: Instalar PlantUML Localmente**

```bash
# Opción A: Con Graphviz (mejor calidad)
# Descargar de: http://www.graphviz.org/download/

# Opción B: Con Java (más simple)
java -version  # Verificar que Java está instalado
```

**Usar desde línea de comandos:**
```bash
java -jar plantuml.jar Diagrama_Clases_UML.puml
# Genera: Diagrama_Clases_UML.png
```

### 🔧 **Opción 3: Integración VS Code**

1. Instalar extensión: **PlantUML** (jebbs.plantuml)
2. Abrir archivo `.puml`
3. Preview: `Alt + D`
4. Export: Click derecho → Export Diagram

---

## 📊 **ESTADÍSTICAS DE LOS DIAGRAMAS**

### **Diagrama de Clases:**
- 23 clases principales
- 4 roles de usuario
- 6 módulos funcionales
- 50+ atributos
- 70+ métodos
- 40+ relaciones

### **Diagrama de Componentes:**
- 10 capas de arquitectura
- 30+ componentes
- 2 aplicaciones cliente
- 9 microservicios
- 4 servicios transversales
- 4 integraciones externas
- 6 BD especializadas

---

## 🎯 **PRÓXIMOS PASOS**

1. **Implementación**: Usar estos diagramas como base para:
   - Crear repositorios Git por servicio
   - Definir OpenAPI/Swagger specs
   - Crear modelos de datos

2. **Documentación**: Generar guías de:
   - APIs REST
   - Flujos de autenticación
   - Procedimientos de deployment

3. **Testing**: Diseñar pruebas basadas en:
   - Interacciones entre servicios
   - Casos de uso críticos (emergencia, autorización)
   - Seguridad y acceso controlado

---

**Diagramas generados automáticamente con PlantUML**
**Proyecto: HealthCorp EPS**
**Fecha: Mayo 2026**
