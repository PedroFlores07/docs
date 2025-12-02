```mermaid
flowchart TD
    A[Inicio] --> B[Inicializar MediaPipe Hands]
    B --> C[Capturar frame de cámara]
    C --> D[Voltear imagen horizontalmente]
    D --> E[Convertir BGR a RGB]
    E --> F[Procesar con MediaPipe]
    F --> G{¿Mano detectada?}
    G -->|No| M[No mostrar gesto]
    G -->|Sí| H[Contar dedos extendidos]
    H --> I{¿Cuántos dedos?}
    I -->|0| J[Gesto = PIEDRA]
    I -->|5| K[Gesto = PAPEL]
    I -->|2| L[Gesto = TIJERA]
    I -->|Otro| M
    J --> N[Mostrar gesto en pantalla]
    K --> N
    L --> N
    M --> O{¿Presionar 'q'?}
    N --> O
    O -->|No| C
    O -->|Sí| P[Fin]
    
