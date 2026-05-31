# paginapowerbi
Pagina creada para visualizar el dashboard


| Lenguaje de Programación | Paradigma Dominante | Mecanismo de Ejecución y Compilación | Tiempo de Ejecución Promedio (ms) | Consumo de Memoria Pico (MB) |
| :--- | :--- | :--- | :---: | :---: |
| **Zig** | Imperativo / Estructurado | Compilación Nativa (LLVM) | | |
| **Python** | Multiparadigma (OO, Imperativo) | Interpretado (CPython / VM) | | |
| **Rust** | Multiparadigma (Funcional, Imperativo) | Compilación Nativa (LLVM) | | |
| **JavaScript** | Multiparadigma (Prototípico, Funcional) | JIT (Just-In-Time) / V8 Engine | | |


#### 1. Matriz Comparativa de Paradigmas de Programación

La siguiente tabla sintetiza los cinco paradigmas fundamentales abordados en esta investigación, destacando sus principios, ventajas, limitaciones y manifestación en lenguajes modernos (Sebesta, 2016).

| Paradigma | Principios Fundamentales | Ventajas Clave | Limitaciones | Lenguajes Representativos |
|-----------|--------------------------|----------------|--------------|---------------------------|
| **Imperativo / Estructural** | Gestión explícita del estado, secuencialidad de instrucciones, mutabilidad de memoria, efectos secundarios, estructuras de control básicas (bucle, condicionales) | Control preciso del hardware, eficiencia en sistemas embebidos, modelo mental sencillo | Complejidad en sistemas grandes, difícil mantenimiento, errores por estado mutable | C, Pascal, Zig |
| **Orientado a Objetos (POO)** | Encapsulamiento, polimorfismo, herencia vs composición, abstracción basada en datos y comportamiento combinados, envío de mensajes | Reutilización, modelado del mundo real, mantenibilidad, escalabilidad | Complejidad añadida, sobreingeniería, problemas con herencia múltiple | Java, C++, Python |
| **Funcional** | Inmutabilidad de datos, funciones como ciudadanas de primer orden, evaluación de la referencia, transparencia referencial, ausencia de efectos colaterales | Código fiable, prueba y depuración, concurrencia natural por diseño | Curva de aprendizaje pronunciada, overhead por inmutabilidad, no intuitivo para problemas con estado intensivo | Haskell, Elixir, Rust, JavaScript |
| **Lógico Declarativo** | Programación basada en relaciones, unificación, cláusulas de Horn, abstracción total del flujo de control por parte del programador | Alto nivel de abstracción, demostración automática de teoremas, ideal para sistemas de reglas | Bajo rendimiento en problemas grandes, ecosistema reducido, poca aplicación comercial | Prolog, Datalog |
| **Concurrencia Actores** | Paso de mensajes entre entidades, aislamiento estricto de estado, mitigación de condiciones de carrera a nivel de diseño lingüístico, modelo "no share" | Concurrencia segura por diseño, ausencia de locks y deadlocks, escalabilidad horizontal | Overhead del paso de mensajes, depuración compleja, riesgo de desbordamiento de buzones | Erlang, Rust (vía librerías) |
