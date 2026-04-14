---
applyTo: "**"
---

# Contexto de Negocio — Remesas 365 Backoffice

## ¿Qué es?

**Remesas 365** es una super-app fintech especializada en remesas y transferencias de dinero en Latinoamérica.
El **backoffice** es el panel de administración web donde el equipo interno gestiona usuarios, transacciones,
compliance, proveedores de pago y configuración de la plataforma.

---

## Dominios de Negocio

### Gestión de Usuarios

| Módulo | Ruta | Descripción |
|---|---|---|
| Usuarios | `/users` | CRUD de usuarios, búsqueda, reset de contraseña, toggle de estado |
| Niveles | `/levels` | Sistema de tiers (BASIC → REGULAR → VERIFIED) basado en KYC |
| KYC | `/kyc` | Monitoreo de verificación de identidad |
| Países | `/countries` | Configuración de países destino, monedas, disponibilidad |

### Operaciones Financieras

| Módulo | Ruta | Descripción |
|---|---|---|
| Wallets | `/wallets` | Cuentas wallet multi-moneda de usuarios |
| Transacciones | `/transactions` | Vista de todas las transacciones de la plataforma |
| Transferencias | `/transfers` | Transferencias P2P entre contactos |
| Remesas | `/remittances` | Envíos internacionales con conversión de moneda |
| Tasas de Cambio | `/exchange-rates` | Configuración de tipos de cambio (con spread) |
| FX Rate Configs | `/fx-rate-configs` | Configuración de fuentes y comportamiento de tasas FX |

### Pagos y Proveedores

| Módulo | Ruta | Descripción |
|---|---|---|
| Proveedores | `/providers` | Integraciones con procesadores externos |
| Métodos por País | `/provider-methods` | Métodos de depósito/retiro por país y proveedor |
| Métodos de Pago | `/payment-methods` | Configuración de métodos (banco, crypto, tarjeta, cash, e-wallet) |
| Comisiones | `/commissions` | Reglas de comisión por tipo de transacción |
| Commission Ledger | `/commission-ledger` | Auditoría de cálculos de comisiones |

### Compliance y Riesgo

| Módulo | Ruta | Descripción |
|---|---|---|
| Compliance | `/compliance` | Monitoreo AML/KYC, alertas, congelamiento de cuentas, reportes SOS |
| Blacklist | `/blacklist` | Identificadores bloqueados (documentos, emails, teléfonos, IPs, crypto) |
| Audit Log | `/audit-log` | Log inmutable de operaciones para investigación |

### Engagement

| Módulo | Ruta | Descripción |
|---|---|---|
| Push Notifications | `/push-notifications` | Campañas push con segmentación de audiencia |
| Dashboard | `/dashboard` | Métricas generales de la plataforma |
| Settings | `/settings` | Configuración del sistema |

---

## Roles de Usuario

Gestionados via **Keycloak**:

| Rol | Descripción |
|---|---|
| `admin` | Administrador estándar |
| `super-admin` | Administrador con permisos elevados |
| `realm-admin` | Administrador a nivel realm |

---

## Glosario

| Término | Definición |
|---|---|
| **User Level** | Tier del usuario: BASIC (0), REGULAR (1), VERIFIED (2) |
| **Wallet** | Cuenta del usuario con balances multi-moneda |
| **P2P Transfer** | Transferencia entre dos usuarios que son contactos activos |
| **Remittance** | Transferencia internacional con conversión de moneda |
| **Exchange** | Conversión de moneda dentro del wallet del usuario |
| **Pay In (Depósito)** | Dinero entrando al wallet desde proveedor externo |
| **Pay Out (Retiro)** | Dinero saliendo del wallet hacia proveedor externo |
| **Provider** | Procesador de pago externo (ZPay24, PayCash, Stripe, Binance) |
| **Payment Method** | Opción específica de depósito/retiro (ej: "ZPay24 COP Bank Transfer") |
| **Commission** | Fee por tipo de transacción |
| **Rate Limit Event** | Usuario excediendo límites operacionales |
| **AML/KYC** | Anti-Money Laundering / Know Your Customer |
| **Blacklist Entry** | Identificador bloqueado (OFAC, sanciones UN, manual) |
| **Idempotency Key** | Identificador único para prevenir transacciones duplicadas |
| **FX Rate** | Tipo de cambio (buy rate, sell rate) con spread |
| **SOS** | Reporte de Operación Sospechosa |
| **Push Campaign** | Notificación push masiva con segmentación |

---

## Proveedores de Pago Integrados

| Proveedor | Países | Monedas | Operaciones |
|---|---|---|---|
| ZPay24 PK | Chile, Perú | CLP, PEN | Depósito, Retiro |
| ZPay24 TDP | Colombia, Chile | COP, CLP | Depósito, Retiro |
| ZPay24 BNC | Global | USDT | Depósito, Retiro |
| PayCash | CO, CL, PE, MX | COP, CLP, PEN, MXN | Depósito (todos), Retiro (solo CO) |
| Stripe | Global | USD | Solo depósito |

---

## Flujos Clave

### Depósito (Pay In)
Usuario selecciona método → Valida métodos disponibles por país → Info de comisión →
Envía pago a proveedor externo → Recibe URL de pago → Completa pago → Webhook confirma →
Wallet acreditado

### Transferencia P2P
Sender selecciona destinatario (debe ser contacto ACCEPTED) → Valida balance →
Request idempotente → Débito sender → Crédito recipient (transacción atómica con locks) →
Log en audit trail

### Promoción de Nivel
Usuario completa requisitos KYC → Sistema detecta elegibilidad →
Promoción automática a REGULAR/VERIFIED → Log en audit → Notificación al usuario

### Conversión de Moneda
Usuario convierte balance → Preview (tasa + comisión, expira en 30s) →
Ejecuta conversión → Débito moneda origen → Crédito moneda destino → Log en audit

### Monitoreo de Compliance
Actividad del usuario → Check de rate limit → Log del evento →
Compara contra umbrales → Si excede: alerta o congelamiento →
Admin revisa en dashboard → Puede agregar a Blacklist → Genera reporte SOS

---

## Documentación Detallada

Cada dominio tiene documentación en `docs/`:

| Carpeta | Contenido |
|---|---|
| `docs/user/` | Gestión de usuarios |
| `docs/levels/` | Sistema de niveles/tiers |
| `docs/transfer/` | Transferencias P2P |
| `docs/exchange/` | Conversión de moneda |
| `docs/payment/` | Pagos y depósitos |
| `docs/payment-method/` | Métodos de pago |
| `docs/recipient/` | Beneficiarios |
| `docs/compliance/` | Compliance y AML |
| `docs/blacklist/` | Lista negra |
| `docs/audit/` | Auditoría |
| `docs/countries/` | Países y monedas |
| `docs/contacts/` | Contactos de usuario |
| `docs/push/` | Push notifications |
| `docs/wallet-pin/` | PIN de wallet |
| `docs/External Providers/` | Documentación de proveedores externos |
