# Decisiones de arquitectura — Semana 10

## Propósito

La entrega es un prototipo ejecutable sin instalación: una página permite comparar un presupuesto con un costo estimado y ver el resultado como JSON. La implementación prioriza la facilidad de prueba y la seguridad básica del lado del navegador. No hay servidor, almacenamiento persistente ni envío de datos.

## Decisiones

1. **Una sola página, sin framework ni proceso de compilación.** `index.html` contiene estructura, estilos utilitarios y lógica del simulador para que pueda abrirse directamente en un navegador. Tailwind se carga desde su CDN para mantener la interfaz ligera durante esta etapa. Para producción se debe valorar compilar y alojar Tailwind localmente.
2. **Cálculo determinista y transparente.** El escenario compara dos valores no negativos. `remaining` es `budget - estimatedCost`; el presupuesto alcanza cuando el resultado es mayor o igual a cero. Los importes son MXN y se formatean solo para presentación; el JSON conserva números.
3. **JSON como salida observable, no como entrada ejecutable.** El objeto se serializa con `JSON.stringify` y se escribe en un elemento `<code>` mediante `textContent`. La descripción nunca se interpreta como HTML, no se concatena en markup ni se pasa a `eval`.
4. **Sanitización de la descripción.** Antes de mostrarla o incluirla en el JSON, se eliminan bloques/etiquetas `script`, otras etiquetas HTML y caracteres de control; el resultado se limita a 500 caracteres. Se usa texto plano como defensa adicional: la sanitización no sustituye el escape contextual si el dato llegara a utilizarse en otro contexto o a un servidor.
5. **Validación explícita.** Los dos importes deben ser números finitos mayores o iguales a cero. Si no lo son, la página informa el error y el JSON expresa `null` para el valor inválido y `invalid_input` para el estado, en vez de simular un resultado válido.
6. **Sin persistencia ni secretos.** Los valores solo viven en la página mientras está abierta. `.gitignore` excluye `.env`, sus variantes y `node_modules/`, y conserva la posibilidad de versionar `.env.example` sin valores secretos.
7. **Accesibilidad básica.** Los controles tienen etiquetas, las unidades no son el único nombre del campo y el resultado se anuncia mediante una región `aria-live`. El diseño responde a pantallas estrechas y amplias.

## Piso de Seguridad — checklist

- [x] No se evalúa ni ejecuta texto introducido por la persona usuaria.
- [x] Descripción limpiada antes de aparecer en vista previa o en el JSON.
- [x] Texto dinámico insertado con `textContent`, no con `innerHTML`.
- [x] Sin peticiones de red, almacenamiento local, cookies ni recolección de datos desde el simulador.
- [x] Validación de importes finitos y no negativos; errores visibles para la persona usuaria.
- [x] `.env` y variantes ignorados por Git; dependencias instaladas excluidas.
- [ ] Confirmar y documentar una política de seguridad de contenido (CSP) antes de desplegar.
- [ ] Para producción, servir Tailwind compilado localmente en vez de depender del CDN.
- [ ] Si se agrega un backend, repetir validación en servidor, usar consultas parametrizadas, limitar entradas y proteger secretos fuera del repositorio.
- [ ] Revisar teclado, contraste y lector de pantalla en los navegadores objetivo antes de liberar.

## Contrato de salida JSON

```json
{
  "simulator": "Dragon Stack",
  "version": 1,
  "currency": "MXN",
  "budget": 15000,
  "estimatedCost": 12000,
  "remaining": 3000,
  "feasible": true,
  "status": "within_budget",
  "description": ""
}
```

`budget` y `estimatedCost` pueden ser `null` mientras el importe respectivo sea inválido; `remaining` y `feasible` también serán `null` mientras ambos importes no sean válidos. `status` será `invalid_input`, `within_budget` o `over_budget`.
