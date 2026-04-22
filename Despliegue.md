
# Informe Técnico: Seguridad y Gestión de Errores

## 1. Mejora de la Seguridad

Para elevar la calificación de seguridad , se implementó un archivo de configuración de infraestructura (`staticwebapp.config.json`). Las mejoras clave fueron:

-   **Encabezados HTTP de Seguridad:** Se configuraron políticas para evitar ataques de _Clickjacking_ (X-Frame-Options) y para asegurar que el navegador solo ejecute scripts confiables
    
-   **Gestión de Permisos:** Se deshabilitó por política el acceso a hardware innecesario (cámara, micrófono) y se restringió el origen de las imágenes para evitar la carga de contenido malicioso
    
-   **Ajuste de Conectividad:** Seguí la recomendación del profesor sobre la implementación de estándares de seguridad en la nube y utilizar este archivo de configuración para aplicar un control estricto sobre las conexiones externas. Esto permitió asegurar que la aplicación solo interactúe con fuentes autorizadas, como la PokeAPI, cumpliendo así con los criterios de auditoría y protección de datos exigidos para este laboratorio.
 
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
