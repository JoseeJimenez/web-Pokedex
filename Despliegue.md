# Informe Técnico: Seguridad y Gestión de Errores

## 1. Mejora de la Seguridad

En mi aplicación, implementé una arquitectura de seguridad basada en `Global Headers` mediante el archivo de configuración de Azure Static Web Apps, con el fin de endurecer la postura de seguridad del lado del cliente

El pilar central es el `Content-Security-Policy`, donde establecí una política de 'Lista Blanca' para restringir la ejecución de scripts y la carga de recursos únicamente a orígenes confiables, como `PokeAPI` y mi propia infraestructura en Azure, mitigando así riesgos de inyección y ataques `XSS`.

Para garantizar la integridad de los datos, configuré `Strict-Transport-Security` con una vigencia de un año y soporte para precarga, forzando una conexión cifrada de extremo a extremo. Además, utilicé cabeceras como `X-Frame-Options` y `Permissions-Policy` para prevenir ataques de `Clickjacking` y restringir el acceso a periféricos innecesarios

Finalmente, resolví los desafíos de navegación de las `Single Page Applications` mediante un `Navigation Fallback`, asegurando que el servidor redirija correctamente las rutas virtuales hacia el núcleo de Angular. En conjunto, estas configuraciones no solo cumplen con los requisitos del laboratorio, sino que elevan la aplicación a estándares profesionales de seguridad
 
## 2. Gestión de Errores y Soluciones

Durante el proceso de despliegue y post-configuración, se presentaron y resolvieron los siguientes incidentes técnicos:

### A. Error de Sincronización (Git Rejected)

-   El Problema: Al intentar subir los cambios de seguridad, el sistema rechazó la subida porque la nube (Azure) había generado archivos automáticos que no estaban en la computadora local
    
-   **La Solución:** Apliqué  un `git pull origin main --rebase` para integrar los cambios remotos con los locales de forma limpia, permitiendo un flujo de trabajo sin pérdida de información
    

### B. Error 500 y Bloqueo de Datos

-   **El Problema:** Tras aplicar las políticas de seguridad iniciales, la página mostraba un error 500 o no cargaba los Pokémon
    
-   **La Solución:** Tras identificar que la política de seguridad era demasiado estricta. Se ajustó el archivo de configuración para permitir conexiones salientes hacia `https://pokeapi.co`, restaurando el servicio inmediatamente
    

### C. Fallo al Recargar (Error de Rutas SPA)

-   **Problema:** Al presionar F5 o refrescar la página, el servidor de Azure no encontraba la ruta y mostraba un error
    
-   **Solución:** Se configuró una regla de `navigationFallback`. Esta regla redirige cualquier petición desconocida al `index.html`, permitiendo que Angular maneje la navegación de forma interna sin que el servidor se rompa

### Optimización del Flujo de CI/CD y Limpieza de Repositorio

 - **Problema:** Se identificaron tres archivos de configuración (`.yml`) en la carpeta `.github/workflows/` compitiendo por el mismo evento de `push`
 
 - **Solución** 	Se eliminaron los flujos erróneos para centralizar el despliegue en el único canal verificado
