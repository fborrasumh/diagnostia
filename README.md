# DiagnostIA

Un panel virtual de cinco médicos que razona por pasos hasta el diagnóstico. Aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/diagnostia/

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23597307.svg)](https://doi.org/10.5281/zenodo.23597307)

**Idiomas:** español (por defecto), inglés, portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

> **No es un producto sanitario.** Es una herramienta docente y de investigación para entender el razonamiento diagnóstico secuencial. No debe usarse para decidir sobre pacientes reales.

## Qué hace

- Reproduce con fines docentes el **orquestador de diagnóstico secuencial** descrito por Nori et al. (*Science*, 2026): un único modelo de IA interpreta a cinco médicos (Hipótesis, Pruebas, Contradictor, Austeridad y Lista de control) que, en cada ronda, actualizan un diferencial de tres diagnósticos con probabilidades, debaten y acuerdan una sola acción: preguntar (hasta 5 preguntas), pedir pruebas (hasta 3) o diagnosticar.
- Trae los **cinco casos sintéticos de SDBench** (repositorio de Microsoft, licencia MIT). Quien diagnostica solo ve la presentación inicial; un **gatekeeper** con el caso completo revela únicamente lo que se pide (y genera resultados sintéticos coherentes si una prueba no está en el caso), y un **juez** puntúa el diagnóstico de 1 a 5 frente al real (4 o 5 cuenta como acierto, como en el artículo).
- Permite comparar en el mismo caso y con el mismo modelo: el **panel** (variantes sin límite, con presupuesto, solo preguntas y respuesta inmediata), el **modelo solo** con el prompt baseline del artículo, y **tú** haciendo de médico ante el mismo gatekeeper.
- Admite **tu propio caso** (presentación, expediente y diagnóstico de referencia) y una **consulta guiada** en la que el panel pregunta y la persona responde.
- Muestra la transcripción ronda a ronda, la evolución del diferencial, el coste orientativo y las notas del juez. Exporta Word, CSV y JSON (reimportable). Incluye un ejemplo precalculado que funciona sin clave.

## Qué hace el código y qué hace la IA

La IA propone (el panel razona, el gatekeeper responde, el juez puntúa). El código decide y comprueba, siempre igual: límites por ronda, eliminación de preguntas y pruebas repetidas, rechazo de órdenes imprecisas («analítica completa», «todas las pruebas»), presupuesto (cancela las pruebas que no caben), umbral de certeza para aceptar un diagnóstico, pregunta de confirmación antes de cerrar (como en el artículo), número máximo de rondas, normalización de probabilidades, coste, nota del juez (entero 1-5) y avisos sobre el gatekeeper: respuestas declaradas como reales con cifras que no están en el caso, y respuestas que mencionan términos del diagnóstico final que no estaban en la petición (posibles pistas).

## Cómo se usa la IA

Con la propia clave de OpenAI, Google Gemini o Anthropic Claude. La clave se guarda solo en el navegador. No hace falta servidor. Cada ronda hace una llamada del panel y otra del gatekeeper (que lee el caso completo, unos 35.000-48.000 caracteres en los casos de práctica); una ejecución completa suele costar de 10 a 20 llamadas.

## Privacidad

Los casos, las ejecuciones y los resultados se guardan en el navegador (IndexedDB). Al ejecutar salen hacia el proveedor de IA elegido la presentación y el expediente que va reuniendo el agente y, para el gatekeeper y el juez, el texto del caso. En «tu caso escrito» y en la «consulta guiada» se enmascaran correos, teléfonos y documentos de identidad antes de enviar; antes del primer envío de la sesión se muestra una muestra de lo que sale y se pide confirmación. Aun así, no deben introducirse datos que identifiquen a una persona.

## Límites

- No es un producto sanitario ni sustituye el juicio clínico.
- El panel es una **reconstrucción** a partir de la descripción publicada del MAI-DxO; el código del orquestador de Microsoft no es público. Sus resultados no reproducen necesariamente los del artículo, y con modelos económicos serán peores.
- Los cinco casos de práctica son sintéticos y sirven para aprender y comparar, no para medir rendimiento (lo advierte el propio repositorio). Con un solo caso no se puede generalizar.
- Los costes son **orientativos**: 300 $ por visita y una tabla propia por tipo de prueba en USD. No son los del artículo (que asignaba códigos CPT y los cruzaba con la lista de precios de un hospital de EE. UU.).
- El gatekeeper y el juez son modelos de lenguaje: pueden equivocarse, filtrar pistas o puntuar con demasiada severidad. El código señala algunos de esos fallos, no todos.
- Las cifras del artículo que muestra la portada (80,3 % frente a 46,8 % en MIMIC-IV-ED; 83,2 % frente a 36,1 % y 22,3 % en NEJM-CPC) proceden de médicos **generalistas** en simulación; los autores advierten de que la diferencia se reduciría frente a especialistas y de que no es práctica clínica real.
- No se ha probado con claves reales: las pruebas usan IA simulada.

## Origen y componentes de terceros

- Nori H, Daswani M, Kelly C, et al. *Sequential diagnosis with language models*. **Science** (2026). doi:[10.1126/science.aeh7930](https://doi.org/10.1126/science.aeh7930). Versión ampliada: arXiv:2506.22405.
- [SDBench](https://github.com/microsoft/sdbench) (Microsoft, licencia MIT, Copyright (c) Microsoft Corporation): casos sintéticos (`data/processed_case_data/synthetic`), prompt baseline (`sdbench/agents/baseline.py`) y reglas del gatekeeper (`gatekeeper_synthetic_tests/v6_fixed`) y del juez (`check_diagnosis/v3_reasoning`), adaptadas a salida JSON. El aviso de licencia de Microsoft se conserva dentro de `index.html`.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche).

A partir del artículo «Sequential diagnosis with language models» (Nori et al., Science, 2026; doi:10.1126/science.aeh7930) y del repositorio SDBench de Microsoft (licencia MIT), del que proceden los casos sintéticos y las reglas del gatekeeper y del juez. El panel es una reconstrucción docente a partir de la descripción publicada, no el código de MAI-DxO.

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

## Cómo citar

Borrás Rocher, F. (2026). *DiagnostIA* (v1.0.0) [Software]. DOI: [10.5281/zenodo.23597307](https://doi.org/10.5281/zenodo.23597307)

## Licencia

MIT. Véase [LICENSE](LICENSE). Los componentes de SDBench mantienen su licencia MIT de Microsoft.
