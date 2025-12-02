```mermaid
graph TD
    A[Inicio] --> B[Inicializar Selfie Segmentation]
    B --> C[Abrir cámara y video de fondo]
    C --> D[Capturar frame de cámara]
    D --> E[Leer frame de video de fondo]
    E --> F{¿Video terminó?}
    F -->|Sí| G[Reiniciar video]
    F -->|No| H[Redimensionar fondo]
    G --> H
    H --> I[Procesar segmentación]
    I --> J[Generar máscara binaria]
    J --> K[Combinar persona + fondo de video]
    K --> L[Mostrar resultado]
    L --> M{¿Presionar 'q'?}
    M -->|No| D
    M -->|Sí| N[Liberar recursos]
    N --> O[Fin]
