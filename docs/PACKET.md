# Packet de planeación — Problema de Doña Mari

> **Supuesto de trabajo, pendiente de confirmar:** no se proporcionó una descripción del caso. Para que este packet sea útil sin inventar hechos, se plantea un escenario de ejercicio: Doña Mari debe decidir si una compra o gasto de su negocio cabe dentro del presupuesto que tiene disponible. La naturaleza de su negocio, el producto, las cantidades, los importes y las restricciones reales siguen pendientes de validación.

## 1. Resumen

Diseñar una herramienta sencilla para que Doña Mari compare el presupuesto disponible con el costo estimado de una decisión de compra. La primera entrega, Dragon Stack, responde una pregunta acotada: **¿el costo estimado cabe en el presupuesto y cuánto sobra o falta?** La herramienta no determina si la compra es rentable ni sustituye asesoría contable o financiera.

## 2. Persona y necesidad

- **Persona:** Doña Mari, dueña o administradora de un pequeño negocio (supuesto de trabajo).
- **Necesidad por validar:** entender rápidamente si puede cubrir un gasto planeado sin exceder el dinero que decidió asignar.
- **Contexto que falta:** giro del negocio, cómo controla actualmente su dinero, frecuencia de compras, moneda, acceso/dispositivo, quién más participa en la decisión y qué significa exactamente “problema de Doña Mari”.

## 3. Enunciado del problema

Cuando Doña Mari considera una compra para su negocio, necesita comparar el presupuesto disponible con el costo total estimado de forma clara, porque una diferencia difícil de calcular puede llevarla a comprometer más dinero del previsto. Dragon Stack hace visible la diferencia y el estado de la comparación.

**Pregunta de diseño:** ¿Cómo podemos ayudar a Doña Mari a evaluar un gasto con información mínima, sin ocultar los supuestos ni presentar el resultado como una recomendación financiera?

## 4. Objetivos y fuera de alcance

### Objetivos de esta iteración

1. Capturar presupuesto disponible y costo estimado.
2. Calcular el saldo después de la compra o el faltante si se excede el presupuesto.
3. Mostrar un resultado comprensible y una representación JSON actualizada al cambiar los campos.
4. Tratar la descripción como texto no confiable y evitar que se interprete como código HTML o JavaScript.
5. Poder abrir y probar el prototipo sin configurar servicios.

### Fuera de alcance

- Inventario, ventas, flujo de caja, impuestos, crédito, proveedores o pronóstico de demanda.
- Recomendar qué comprar, afirmar que una compra será rentable o conectar con cuentas bancarias.
- Guardar datos entre sesiones, crear cuentas, sincronizar datos o enviar información a un servidor.
- Asumir importes, productos, fechas o circunstancias reales de Doña Mari.

## 5. Flujo propuesto

1. La persona abre el simulador y escribe su presupuesto disponible.
2. Introduce el costo estimado total de la decisión.
3. Opcionalmente describe el escenario con texto libre.
4. La interfaz valida los importes y comunica si alcanza el presupuesto; informa el saldo restante o el monto faltante.
5. La vista JSON refleja inmediatamente los valores, el estado y la descripción sanitizada.
6. La persona ajusta los datos para comparar escenarios. Al cerrar la página, los datos no se conservan.

## 6. Datos y reglas

| Campo | Requerido | Regla |
|---|---:|---|
| Presupuesto disponible | Sí | Número finito, mayor o igual a cero; MXN |
| Costo estimado | Sí | Número finito, mayor o igual a cero; MXN |
| Descripción | No | Texto plano, máximo 500 caracteres tras sanitizar |
| Saldo | Calculado | Presupuesto menos costo |
| Factibilidad | Calculado | Alcanza si el saldo es mayor o igual a cero |

Si falta un importe válido, no se muestra una conclusión financiera. El JSON usa estado `invalid_input`; los campos y cálculos que no puedan determinarse quedan en `null`. El caso en que costo y presupuesto sean iguales cuenta como factible, con saldo cero.

## 7. Requisitos de experiencia y seguridad

- Etiquetas explícitas, instrucciones junto a los campos y mensajes de error comprensibles.
- Resultado visual distinto para importe cubierto, excedido e inválido; no depender solo del color.
- Actualización inmediata sin botón de envío ni recarga.
- La descripción no se procesa como HTML. El prototipo elimina bloques `script`, etiquetas y caracteres de control; el renderizado usa texto plano.
- No incluir datos personales ni secretos en ejemplos. No enviar o persistir los datos introducidos.
- Comunicar que el resultado es una comparación aritmética, no una recomendación financiera.

## 8. Criterios de aceptación

- [ ] Con presupuesto 15,000 y costo 12,000, indica que alcanza y muestra 3,000 MXN restantes.
- [ ] Con presupuesto 10,000 y costo 12,000, indica que excede y muestra un faltante de 2,000 MXN.
- [ ] Con cantidades iguales, indica que alcanza y muestra saldo cero.
- [ ] Valores vacíos, negativos o no numéricos generan un error visible y no un resultado factible.
- [ ] Cambiar cualquier importe actualiza resultado y JSON sin recargar.
- [ ] Una descripción que contenga etiquetas o un bloque `<script>` se presenta como texto inocuo; el JSON incluye solo la versión sanitizada.
- [ ] En una pantalla estrecha, los campos y el JSON siguen siendo legibles y utilizables.
- [ ] Ningún dato introducido persiste al cerrar o recargar la página.

## 9. Plan de trabajo

1. **Descubrimiento:** entrevistar a Doña Mari; confirmar el problema concreto, la decisión que necesita tomar y el vocabulario que utiliza.
2. **Definir el modelo:** acordar moneda, qué significa presupuesto, qué costos deben sumarse y cómo manejar costos inciertos o recurrentes.
3. **Prototipar:** probar el flujo de comparación con datos ficticios y validar que la diferencia entre saldo y faltante sea clara.
4. **Probar accesibilidad y seguridad:** teclado, lector de pantalla, contraste, entradas límite, campos vacíos y texto con etiquetas/script.
5. **Validar el resultado:** observar a una persona usuaria completar una decisión de principio a fin y ajustar el flujo según evidencia.
6. **Decidir siguiente alcance:** solo después de validar la necesidad, priorizar almacenamiento, historial o funciones de negocio; cada ampliación requiere análisis de privacidad y seguridad.

## 10. Riesgos, supuestos y preguntas abiertas

| Riesgo o pregunta | Mitigación / validación |
|---|---|
| El problema real de Doña Mari puede no ser una compra o gasto. | Confirmar el relato original con ella antes de usar el prototipo como solución final. |
| “Presupuesto” puede significar efectivo, presupuesto mensual o límite para una compra. | Acordar definición y periodo; mostrarlo explícitamente en la interfaz si aplica. |
| El costo podría omitir envío, impuestos u otros cargos. | Confirmar qué compone el costo total y mostrar desglose si es necesario. |
| Un resultado “alcanza” podría interpretarse como recomendación de compra. | Mantener el alcance descriptivo y explicar que solo compara dos importes. |
| El CDN de Tailwind requiere conexión y añade dependencia externa. | Aceptable para prototipo; compilar y alojar estilos localmente antes de producción. |
| Sanitizar texto no sustituye protección contextual en futuras superficies. | Seguir usando APIs de texto plano; validar en servidor si se incorpora backend. |

## 11. Métricas para validar el prototipo

- La persona puede completar la comparación sin ayuda.
- La persona identifica correctamente si el importe cabe y cuánto sobra o falta.
- Tiempo y errores al ingresar valores, observados en pruebas moderadas.
- Comprensión de que “factible” solo expresa una comparación presupuesto/costo.

No se fijan umbrales cuantitativos hasta tener una línea base de pruebas con personas usuarias y confirmar las necesidades reales.
