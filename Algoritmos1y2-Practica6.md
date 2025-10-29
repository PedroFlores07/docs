```mermaid
graph TD
    Start([Inicio]) --> Capture[Capturar Frame de Cámara]
    Capture --> Gray[Convertir a Escala de Grises]
    Gray --> Choice{Seleccionar Algoritmo}
    
    Choice -->|Algoritmo 1| Blur1[Suavizado Gaussiano<br/>kernel=9x9]
    Blur1 --> Hough[Transformada de Hough<br/>Detectar Círculos]
    Hough --> CheckCircles{¿Círculos<br/>Detectados?}
    CheckCircles -->|No| Display
    CheckCircles -->|Sí| ClassifyR[Clasificar por<br/>Rangos de Radio]
    ClassifyR --> CountR[Contar Monedas<br/>y Calcular Total]
    CountR --> DrawR[Dibujar Círculos<br/>y Etiquetas]
    DrawR --> Display
    
    Choice -->|Algoritmo 2| Blur2[Suavizado Gaussiano<br/>kernel adaptativo]
    Blur2 --> Thresh[Umbralización Binaria<br/>Inversa]
    Thresh --> Morph[Operaciones Morfológicas<br/>Cierre + Apertura]
    Morph --> Contours[Detectar Contornos<br/>Externos]
    Contours --> CheckCont{¿Contornos<br/>Encontrados?}
    CheckCont -->|No| Display
    CheckCont -->|Sí| Filter[Filtrar por Área<br/>y Circularidad]
    Filter --> ClassifyA[Clasificar por<br/>Rangos de Área]
    ClassifyA --> CountA[Contar Monedas<br/>y Calcular Total]
    CountA --> DrawA[Dibujar Contornos<br/>y Etiquetas]
    DrawA --> Display
    
    Display[Mostrar Resultados<br/>Frame + Panel Info]
    Display --> Exit{Presionar<br/>ESC?}
    Exit -->|No| Capture
    Exit -->|Sí| End([Fin])
    
    style Start fill:#90EE90
    style End fill:#FFB6C1
    style Hough fill:#87CEEB
    style Thresh fill:#87CEEB
    style Choice fill:#FFD700
    style Display fill:#DDA0DD
