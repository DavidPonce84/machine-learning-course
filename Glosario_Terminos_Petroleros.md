# Glosario Didáctico de Términos Petroleros (Upstream)
### Preparado para: David Ponce (Manual de Repaso de Conceptos Físicos)

Este documento explica de manera sumamente sencilla y con analogías cotidianas las variables físicas utilizadas en los datasets del curso de Machine Learning para Schlumberger (SLB). Te servirá para explicar la física detrás de los datos en tus clases de forma intuitiva.

---

## 1. Variables de Producción y Presión (Yacimientos)

### 🔹 WHP (`WHP_psi` - Wellhead Pressure / Presión en Cabeza)
* **¿Qué es de forma simple?**: Es la presión del fluido (petróleo, agua y gas mezclados) medida justo en la parte superior del pozo, en la superficie (el "árbol de navidad" o cabezal).
* **Analogía cotidiana**: Imagina una manguera de jardín abierta. Si pones el dedo a medio tapar la boquilla, sientes una fuerza empujando tu dedo. Esa fuerza es la "presión en cabeza". Si la WHP cae a cero, significa que el pozo no tiene fuerza para empujar el fluido hasta la superficie.
* **Unidad**: **psi** (libras por pulgada cuadrada). Una llanta de auto inflada tiene unos 32 psi; un pozo petrolero puede tener entre 200 y 3000 psi en cabeza.

### 🔹 BHP (`BHP_psi` - Bottomhole Pressure / Presión de Fondo)
* **¿Qué es de forma simple?**: Es la presión medida en el fondo del pozo, a miles de metros bajo tierra, justo donde la roca del yacimiento conecta con la tubería de producción.
* **Analogía cotidiana**: Imagina que tomas un vaso con un batido espeso usando un pitillo (popote). La fuerza con la que el líquido es empujado en la base del pitillo es la BHP.
* **Comportamiento clave**: 
  * Si el pozo está **fluyendo** (produciendo), la BHP es baja porque el fluido está subiendo libremente (BHP fluyente).
  * Si el pozo se **cierra** (se apaga la producción), el fluido deja de moverse y la presión en el fondo sube hasta equilibrarse con la presión de la roca del yacimiento (BHP estática).

### 🔹 Caudales de Producción (`Qo_bpd`, `Qw_bpd`, `Qg_mscfd`)
* **¿Qué son?**: La velocidad de salida de los tres fluidos que produce un pozo. Rara vez un pozo produce 100% petróleo; usualmente es una mezcla de petróleo, agua salada y gas.
  * **`Qo_bpd` (Caudal de Petróleo)**: Cantidad de barriles de crudo puro al día.
  * **`Qw_bpd` (Caudal de Agua)**: Cantidad de barriles de agua salada al día. (El agua viene atrapada en los poros de la roca).
  * **`Qg_mscfd` (Caudal de Gas)**: Cantidad de gas que sale disuelto en el petróleo en miles de pies cúbicos estándar al día.
* **Unidades**: 
  * **bpd** (barriles por día). Un barril de petróleo equivale a **159 litros**.
  * **mscfd** (miles de pies cúbicos por día).
* **KPIs Derivados**:
  * **Water-Cut (Corte de Agua)**: Qué porcentaje del líquido total es agua salada. Si un pozo produce 100 barriles de líquido y 80 son de agua, el Water-Cut es del 80%.
  * **GOR (Relación Gas-Petróleo)**: Cuántos pies cúbicos de gas salen por cada barril de petróleo. Es como abrir una botella de Coca-Cola: a cierta presión, el gas empieza a burbujear fuera del líquido.

---

## 2. Variables Mecánicas de Perforación

### 🔹 ROP (`ROP_m_h` - Rate of Penetration / Velocidad de Penetración)
* **¿Qué es de forma simple?**: Qué tan rápido avanza la broca perforando la roca hacia el fondo. Es la velocidad de avance del taladro.
* **Analogía cotidiana**: Si estás colgando un cuadro en tu casa y pones un taladro contra la pared, la ROP es la velocidad a la que la broca entra en el ladrillo (ej. 5 centímetros por segundo). En un pozo real, se mide en **metros por hora (m/h)**.
* **Importancia**: Es la variable de salida que siempre queremos maximizar para ahorrar costos de renta de taladro.

### 🔹 WOB (`WOB_klb` - Weight on Bit / Peso sobre la Broca)
* **¿Qué es de forma simple?**: La fuerza de empuje vertical hacia abajo aplicada sobre la broca contra la roca del fondo.
* **Analogía cotidiana**: Cuando usas el taladro en casa y la pared está muy dura, te apoyas con el peso de tu cuerpo sobre el taladro para obligarlo a entrar. Ese peso que aplicas es el WOB.
* **Unidad**: **klb** (kilo-libras o miles de libras de peso).

### 🔹 RPM (Revoluciones por Minuto)
* **¿Qué es de forma simple?**: La velocidad de giro de la broca. Cuántas vueltas completas da la broca en un minuto.
* **Analogía cotidiana**: Los niveles de velocidad (1, 2, 3) de una batidora o taladro de mano.

### 🔹 Torque (`Torque_ft_lb`)
* **¿Qué es de forma simple?**: La fuerza de torsión necesaria para hacer girar la tubería y vencer la resistencia de la roca.
* **Analogía cotidiana**: La fuerza que tienes que hacer con los dedos y la muñeca para abrir un frasco de mermelada muy apretado. Si la tapa está atascada, requieres un alto "torque" para romper la resistencia.
* **Importancia en datos**: Si el torque empieza a vibrar o subir bruscamente de forma errática, significa que la broca está sufriendo fricción excesiva contra la roca y podría atascarse en el pozo.

---

## 3. Variables de Registros Geofísicos (Well Logs)

*Nota: Los registros geofísicos son herramientas con sensores que se bajan con un cable al pozo para medir la roca a diferentes profundidades.*

### 🔹 GR (`GR_API` - Gamma Ray / Rayos Gamma)
* **¿Qué es de forma simple?**: Mide la radiactividad natural de las capas de la tierra.
* **Física simple**: Las arcillas o lutitas (rocas sello, compactas e impermeables) contienen elementos radiactivos naturales (potasio, torio). Las arenas limpias (que pueden almacenar petróleo) no son radiactivas.
* **Regla didáctica**: 
  * **GR Alto (>90 API)** = Arcilla/Lutita (Malo, no hay petróleo).
  * **GR Bajo (<40 API)** = Arena limpia (Bueno, potencial roca almacén).
* **Analogía cotidiana**: Un contador Geiger de radiación. Mapea la arcilla como si fuera radiación.

### 🔹 ILD (`ILD_ohm_m` - Resistividad Inductiva Profunda)
* **¿Qué es de forma simple?**: Mide qué tan difícil es para la electricidad pasar a través de la roca y los fluidos dentro de sus poros.
* **Física simple**:
  * El **agua salada** conduce la electricidad perfectamente. Por ende, la roca con agua tiene **muy baja resistividad** (< 2 ohm-m).
  * El **petróleo y el gas** son aislantes eléctricos (no conducen corriente). Por ende, la roca saturada con petróleo tiene **alta resistividad** (> 15 ohm-m).
* **Regla didáctica**:
  * Arena con GR bajo + Resistividad baja = Arena con agua.
  * Arena con GR bajo + Resistividad alta = Arena con petróleo (¡Bingo! Reservorio comercial).

### 🔹 RHOB (`RHOB_g_cc` - Densidad Bulk) & NPHI (`NPHI_v_v` - Porosidad Neutrón)
* **¿Qué son de forma simple?**:
  * **RHOB**: Mide el peso (densidad) físico de la roca en gramos por centímetro cúbico. Rocas porosas y livianas tienen baja densidad; rocas compactas tienen alta densidad.
  * **NPHI**: Mide la cantidad de hidrógeno en los poros (el hidrógeno frena neutrones). Dado que el agua y el petróleo tienen mucho hidrógeno, se usa como estimador directo de la porosidad.
* **El Crossover (Efecto Cruce)**:
  * Al graficar ambas curvas juntas con escalas invertidas estándar: en zonas de arcilla (shale), las curvas corren en paralelo o separadas.
  * Cuando entramos a una **arena limpia con petróleo o gas**, la densidad RHOB baja (se desplaza a la izquierda en el gráfico) y el neutrón NPHI también marca lecturas más bajas (se desplaza a la derecha en la escala invertida). Esto causa que las líneas se crucen visualmente, creando un espacio cerrado entre ellas.
  * **Firma visual**: Este cruce de curvas es la evidencia directa de que encontramos una zona de reservorio poroso con hidrocarburos.
