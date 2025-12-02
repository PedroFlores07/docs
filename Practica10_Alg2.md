```mermaid
flowchart TD
    A[Inicio] --> B[Inicializar MediaPipe Pose]
    B --> C[Contador = 0, Estado = ABAJO]
    C --> D[Capturar frame]
    D --> E[Convertir BGR a RGB]
    E --> F[Procesar con MediaPipe]
    F --> G{¿Pose detectada?}
    G -->|No| N[Mostrar solo contador]
    G -->|Sí| H[Obtener hombro, cadera, rodilla]
    H --> I[Calcular ángulo torso]
    I --> J{¿Ángulo < 90°?}
    J -->|Sí y Estado=ABAJO| K[Estado = ARRIBA]
    J -->|No| L{¿Ángulo > 130°?}
    K --> L
    L -->|Sí y Estado=ARRIBA| M[Estado = ABAJO<br/>Contador++]
    L -->|No| N
    M --> N
    N --> O[Dibujar landmarks y contador]
    O --> P{¿Tecla?}
    P -->|'q'| Q[Fin]
    P -->|'r'| C
    P -->|Ninguna| D
