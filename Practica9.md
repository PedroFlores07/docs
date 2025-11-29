```mermaid
flowchart TD
    A["INICIO<br/>Inicializar cámara 720p<br/>Cargar modelos OCR"] --> B["Capturar frame de video"]
    B --> C["Convertir a escala de grises"]
    C --> D["Aplicar GaussianBlur<br/>y CLAHE"]
    D --> E["Detección de bordes<br/>Canny"]
    E --> F["Dilatación morfológica"]
    F --> G["Encontrar contornos"]
    G --> H{"¿Pasó 0.5s<br/>desde última<br/>detección?"}
    H -->|No| B
    H -->|Sí| I["Filtrar contornos<br/>por área ≥ 3000 px"]
    I --> J{"¿Existe<br/>cuadrilátero<br/>válido?"}
    J -->|No| B
    J -->|Sí| K["Validar relación<br/>de aspecto<br/>0.4 - 2.5"]
    K --> L{"¿Relación<br/>válida?"}
    L -->|No| B
    L -->|Sí| M["Ordenar esquinas<br/>orden_puntos()"]
    M --> N["Calcular matriz de<br/>perspectiva"]
    N --> O["Aplicar transformación<br/>warpPerspective()"]
    O --> P["Obtener tarjeta<br/>frontal rectificada"]
    P --> Q["Preprocesar imagen<br/>preprocess()"]
    Q --> R["Thresholding binario<br/>+ Upscaling 2x"]
    R --> S["Ejecutar OCR<br/>Tesseract"]
    S --> T["Medir tiempo<br/>Tesseract"]
    T --> U["Ejecutar OCR<br/>EasyOCR"]
    U --> V["Medir tiempo<br/>EasyOCR"]
    V --> W["Limpiar expresión<br/>clean_expr()"]
    W --> X["Evaluar expresión<br/>eval_expr()"]
    X --> Y{"¿Expresión<br/>válida?"}
    Y -->|No| Z["Mostrar error"]
    Y -->|Sí| AA["Calcular resultado"]
    Z --> AB["Dibujar recuadro verde<br/>en tarjeta detectada"]
    AA --> AB
    AB --> AC["Mostrar en pantalla:<br/>- Operación Tesseract<br/>- Resultado Tesseract<br/>- Operación EasyOCR<br/>- Resultado EasyOCR<br/>- Tiempos promedio"]
    AC --> AD{"¿Presionó<br/>tecla 'q'?"}
    AD -->|No| B
    AD -->|Sí| AE["Mostrar estadísticas<br/>finales"]
    AE --> AF["Liberar recursos<br/>FIN"]
    
    style A fill:#90EE90
    style AF fill:#FFB6C6
    style S fill:#87CEEB
    style U fill:#87CEEB
    style H fill:#FFE4B5
    style J fill:#FFE4B5
    style L fill:#FFE4B5
    style Y fill:#FFE4B5
    style AD fill:#FFE4B5
