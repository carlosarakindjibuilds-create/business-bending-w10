# Registro de Decisiones Técnicas - Semana 10 (DECISIONS.md)

## 1. Decisiones Técnicas Tomadas
- **Arquitectura de Interfaz Interactiva:** Se implementó una interfaz visual moderna basada en Tailwind CSS que simula de extremo a extremo las condiciones de compra de un negocio en un entorno co-laboral.
- **Validación Orientada a Restricciones:** Se integró un motor lógico en JavaScript para evaluar escenarios financieros en tiempo real (Suficiente, Excedido e Importes Iguales). Esto rompe con el uso de métricas fijas de semanas pasadas que afectaron las notas.
- **Sanitización Dinámica de Entradas:** Se desplegó un filtro estricto en el lado del cliente (Frontend) para inspeccionar caracteres en el cuadro de texto. Al detectar anomalías (como etiquetas `<script>`), se bloquea el envío y se activa un escudo rojo de protección.

## 2. Checklist del Piso de Seguridad (Security Floor)
- **[PISO 1] No secretos en código:** El diseño de la aplicación opera sin exponer llaves privadas, variables de entorno (.env) o tokens de acceso en el código público de GitHub.
- **[PISO 2] Sanitización de Inputs:** Todas las entradas de texto del formulario eliminan caracteres de inyección de código antes de calcular cualquier valor en el Dragon Stack.
- **[PISO 3] Persistencia Segura:** Las estructuras se prepararon con esquemas limpios listos para operar bajo políticas RLS (Row Level Security) de Supabase.

## 3. Próximo Paso Técnico Inmediato
- Mantener el monitoreo de telemetría estructurada JSON a la derecha para auditar el comportamiento del sistema.
