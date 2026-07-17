# Diccionario de Datos y Nomenclatura - Datasets Sintéticos (Petróleo & Gas)

Este directorio contiene tres conjuntos de datos sintéticos diseñados para simular escenarios reales de la industria del petróleo (Upstream). Han sido preparados específicamente para prácticas de Análisis Exploratorio de Datos (EDA) y Limpieza de Datos.

---

## 1. Dataset de Producción de Pozos (`produccion_pozos.csv`)
Simula el historial de producción diaria de 5 pozos activos durante un periodo de 3 años (2023 - 2025).

### Diccionario de Variables
| Nombre de Variable | Tipo de Dato | Unidad | Descripción |
| :--- | :--- | :--- | :--- |
| `Fecha` | DateTime (YYYY-MM-DD) | - | Fecha de registro diario de la producción. |
| `Pozo` | String / Categórico | - | Identificador único del pozo (`Pozo-01` a `Pozo-05`). |
| `WHP_psi` | Float | psi (libras por pulgada cuadrada) | Presión en Cabeza del Pozo (Wellhead Pressure). Medida antes del estrangulador (choke). |
| `BHP_psi` | Float | psi | Presión de Fondo Fluyente (Bottomhole Pressure). Medida a nivel del reservorio. |
| `Qo_bpd` | Float | bpd (barriles de petróleo por día) | Caudal diario de crudo producido (Oil rate). |
| `Qw_bpd` | Float | bpd | Caudal diario de agua producida (Water rate). |
| `Qg_mscfd` | Float | mscfd (miles de pies cúbicos estándar por día) | Caudal diario de gas asociado producido (Gas rate). |

### Anomalías Operativas inyectadas intencionalmente para limpieza
- **Fallas de Telemetría (Valores Nulos)**: Ciertas filas contienen valores vacíos (`NaN`) en `WHP_psi` y `Qo_bpd` simulando caídas de señal de sensores RTU.
- **Error de Transductor de Presión**: En el `Pozo-01` entre el 10 y el 20 de Julio de 2023, la variable `BHP_psi` lee exactamente `-999.0`, valor comúnmente configurado en sistemas SCADA ante fallas del transductor.
- **Falla del Medidor de Flujo de Agua**: El `Pozo-03` reporta exactamente `0.0` bpd de agua durante todo el año 2023 debido a un medidor atascado mecánicamente, a pesar de que el pozo está fluyendo con crudo.
- **Ruido Eléctrico / Spike Absurdo**: El `Pozo-02` contiene un registro de `99,999.0` bpd de crudo el día `2024-11-12`, causado por una falla óptica temporal del medidor multifásico.
- **Cierre de Pozo (Shut-in)**: Periódicamente, los pozos entran en mantenimiento (ej. `Pozo-04` del 10 al 25 de Marzo de 2024). Durante estos periodos, los caudales (`Qo`, `Qw`, `Qg`) son `0.0`, la presión de cabeza (`WHP`) cae a niveles residuales de línea (~50 psi), y la presión de fondo (`BHP`) sube progresivamente hasta alcanzar la presión estática del yacimiento (~3300 psi).

---

## 2. Dataset de Perforación de Pozos (`perforacion_pozos.csv`)
Simula datos operacionales de alta frecuencia (registrados en función de la profundidad) recolectados por sensores en la cabina del perforador mientras se perfora una sección de pozo desde los 2000m hasta los 3000m de profundidad.

### Diccionario de Variables
| Nombre de Variable | Tipo de Dato | Unidad | Descripción |
| :--- | :--- | :--- | :--- |
| `Profundidad_m` | Float (intervalo 0.5m) | metros | Profundidad medida de la broca de perforación. |
| `WOB_klb` | Float | klb (kilo-libras) | Peso sobre la Broca (Weight on Bit). Parámetro controlado por el perforador. |
| `RPM` | Float | rpm (revoluciones por minuto) | Velocidad de rotación de la broca o de la mesa rotaria/Top Drive. |
| `Torque_ft_lb` | Float | ft-lb (pies-libra) | Torque o torsión experimentada en la sarta de perforación al rotar. |
| `ROP_m_h` | Float | m/h (metros por hora) | Velocidad de Penetración (Rate of Penetration). Velocidad de avance de la broca. |

### Eventos Físicos y Anomalías inyectadas intencionalmente para limpieza
- **Falla del Tacómetro de RPM**: De los 2250m a los 2270m, el sensor de RPM lee exactamente `0.0` rpm, lo cual contradice físicamente el hecho de que la ROP siga activa en un promedio de 30 m/h (perforación rotatoria clásica).
- **Cambio de Litología (Formación Dura)**: De los 2400m a los 2460m, el pozo atraviesa una caliza muy dura. Esto provoca una caída abrupta de ROP a ~6 m/h (a pesar de que el perforador aumenta el peso `WOB` a ~25 klb para compensar) y genera una alta vibración de torque con oscilaciones severas.

---

## 3. Dataset de Registros Geofísicos (`registros_pozo.csv`)
Simula curvas de perfiles de pozo (Well Logs) medidas por herramientas LWD/Wireline desde los 1500m hasta los 2200m de profundidad. Estas curvas se usan en petrofísica para caracterizar el yacimiento y estimar litología.

### Diccionario de Variables
| Nombre de Variable | Tipo de Dato | Unidad | Descripción |
| :--- | :--- | :--- | :--- |
| `Profundidad_m` | Float (intervalo 0.1m) | metros | Profundidad vertical o medida del registro. |
| `GR_API` | Float | API (unidades del American Petroleum Institute) | Registro de Rayos Gamma. Mide la radiactividad natural de las formaciones (determina si es arcilla o arena). |
| `ILD_ohm_m` | Float | ohm-m (ohmio-metro) | Resistividad de la Formación. Indica el tipo de fluido en los poros (baja = agua salada, alta = hidrocarburos). |
| `RHOB_g_cc` | Float | g/cc (gramos por centímetro cúbico) | Registro de Densidad de Formación. Mide la densidad bulk de la roca. |
| `NPHI_v_v` | Float | v/v (fracción volumétrica) | Registro de Porosidad Neutrón. Mide la concentración de iones de hidrógeno (asociada a la porosidad). |

### Respuestas Físicas y Anomalías inyectadas intencionalmente para limpieza
- **Zonas de Reservorio (Arena con Hidrocarburos)**: En el intervalo de 1820m a 1950m se simula una arena limpia de yacimiento cargada de petróleo. Presenta bajo Gamma Ray (GR < 40 API), alta resistividad (ILD > 20 ohm-m) y un cruce visual de las curvas de densidad y neutrón (crossover).
- **Derrape del Sensor de Densidad (Falla de Calibración)**: De los 2050m a los 2070m, el sensor de densidad `RHOB_g_cc` reporta exactamente `1.0` g/cc (densidad del agua pura, físicamente imposible para una roca consolidada a esa profundidad), debido a un desprendimiento de la zapata de la herramienta contra la pared del pozo (lavado de pozo o washout).
