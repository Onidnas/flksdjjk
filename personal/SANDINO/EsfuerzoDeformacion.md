# Esfuerzo y Deformación (en la vida real)

Piensa en una **liga o goma de mascar** que estiras con las manos.

## Esfuerzo (σ) = Lo fuerte que jalas
Es **cuánto empujas o tiras** sobre el material.
- Es la fuerza aplicada **repartida** sobre el área.
- Fórmula: σ = F / A (fuerza entre área)
- Ejemplo: pisar una lata con el pie plano (área grande → poco esfuerzo) vs. pisarla con taco (área chica → mucho esfuerzo). La lata se hunde igual, pero el taco la atraviesa.

## Deformación (ε) = Cuánto se estira
Es **cuánto cambia de forma** el material.
- Fórmula: ε = ΔL / L₀ (lo que se estiró entre el largo original)
- Ejemplo: estiras la liga de 10cm a 15cm → se deformó 5cm.

## La conexión: Ley de Hooke (materiales elásticos)
> Lo que más jalas, más se estira... hasta que ya no vuelve.

- Si la liga **vuelve a su tamaño** al soltar → comportamiento **elástico** (se recupera).
- Si la estiras de más y **ya no vuelve** → sobrepasaste el **límite elástico**.
- Si la **rompes** → **fractura** (se acabó).

## Torque (relación con tu otra nota)
Recuerda el torque: es la capacidad de una fuerza de **girar** algo.
- Empujar una puerta por el borde (lejos de las bisagras) → mucho torque, se abre fácil.
- Empujar justo en las bisagras → casi nada de torque, no se abre.
- Lo mismo: a distancia mayor o fuerza mayor = más giro = más **esfuerzo** interno en el material.

## Para qué sirve en la vida
- No se rompe el puente porque se calcula el esfuerzo máximo que soporta cada material.
- Los tornillos, cables, vigas y tuberías se dimensionan con estas cuentas.
- Concreto resiste compresión (apriete), acero resiste tensión (jalón): por eso se usan juntos.

## Aterrizando en Ingeniería en Sistemas... con la carretera

No hay puentes ni vigas en tu computadora, pero el **mismo pensamiento** se usa para que un sistema no se "rompa". Cambiemos la liga por una **carretera con casetas de peaje**:

- **Esfuerzo (σ)** = cuántos coches (peticiones) llegan a la vez. En rebajas = miles, a las 3am = casi ninguno.
- **Deformación (ε)** = la **latencia** (tiempo de respuesta). Respuestas normal en 100ms → con mucha carga responden en 2 segundos. Ahí el "material" se está estirando.
- **Límite elástico** = hasta dónde aguanta sin dañarse. Sí responde lento pero aún responde. Si pasas de ahí → **fractura** = el servidor se cae / timeouts.
- **Fractura** = caída total del sistema (página no carga, error 500, app cerrada).

## ¿Y qué hace un ingeniero (dev/SRE) con esto?

**Autoescalado = abrir más casetas cuando llegan más coches.**
- Se acerca la avalancha → se levanta otro servidor → todo vuelve a fluir rápido → "la deformación se recupera" (es *elástico*).

**Pruebas de estrés = pisar el acelerador a propósito.**
- Enviar cargas enormes (millones de peticiones) en un entorno de prueba para **encontrar el punto de quiebre** ANTES de que pase en producción.
- Igual que estiras la liga hasta que se rompe, pero en un laboratorio, no con clientes reales.

**Monitoreo = la regla y el termómetro.**
- Se miran las métricas: CPU, memoria, latencia. Cuando la "deformación" sube (respuesta lenta), es señal de que estás llegando al límite elástico y hay que escalar o optimizar.

## Mini-ejercicio (dinámico) para estudiantes

> Hoy atendiste un sistema que responde en **200ms** con 100 usuarios. En un examen salen **1000 usuarios** y la respuesta sube a **2 segundos** (2000ms).
>
> 1. ¿Cuál fue el "esfuerzo"? (pistas: usuarios subieron x10).
> 2. ¿Cuál fue la "deformación"? (= ¿cuánto creció la latencia?).
> 3. Si el objetivo es responder en menos de **1.5s**, ¿ese sistema está en región elástica o ya se está acercando a la fractura?

**Respuesta (al final del problema, escribe primero la tuya):**
1. Esfuerzo: las peticiones (demanda) subieron de 100 a 1000, es decir **x10**.
2. Deformación: la latencia creció de 200ms a 2000ms → **se multiplicó x10**.
3. 2s (2000ms) > 1.5s (meta) → **ya pasó el límite**: el sistema "se deformó" más de lo permitido. Hay que sumar servidores (escalar) o optimizar antes de que se fracture del todo.

## La regla de oro
> La computadora no es metal, pero **se fractura igual** cuando la demanda (esfuerzo) supera su capacidad (lo que puede procesar por segundo). El truco de la ingeniería: **encontrar ese punto de quiebre ANTES del desastre** y mantener el sistema siempre en zona elástica.

---
[[Fisica]]
K[[FeymanEsfuerzoDeformacion]]
