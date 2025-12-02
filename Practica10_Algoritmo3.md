```mermaid
graph TD
    A[Inicio] --> B[Inicializar MediaPipe Face Mesh]
    B --> C[Abrir cámara]
    C --> D[Capturar frame]
    D --> E[Detectar rostro]
    E --> F{¿Rostro detectado?}
    F -->|No| D
    F -->|Sí| G[Calcular apertura de ojos EAR]
    G --> H{¿EAR < umbral?}
    H -->|No| I[Ojos abiertos<br/>Resetear contador]
    H -->|Sí| J[Iniciar/continuar temporizador]
    J --> K{¿Tiempo > 3s?}
    K -->|No| L[Mostrar tiempo transcurrido]
    K -->|Sí| M[Activar alarma sonora]
    M --> L
    I --> L
    L --> N{¿Presionar 'q'?}
    N -->|No| D
    N -->|Sí| O[Liberar recursos]
    O --> P[Fin]
