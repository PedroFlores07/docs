```mermaid
graph TD
    A[Inicio] --> B[Cargar Imagen]
    B --> C{Seleccionar Método}
    
    C -->|RGB| D[Método RGB]
    C -->|HLS| E[Método HLS]
    
    D --> D1[Separar canales R, G, B]
    D1 --> D2[Ecualizar canal R]
    D2 --> D3[Ecualizar canal G]
    D3 --> D4[Ecualizar canal B]
    D4 --> D5[Reconstruir imagen RGB]
    D5 --> F[Calcular Histogramas]
    
    E --> E1[Convertir BGR a HLS]
    E1 --> E2[Separar canales H, L, S]
    E2 --> E3[Ecualizar solo canal L]
    E3 --> E4[Reconstruir con H y S originales]
    E4 --> E5[Convertir HLS a RGB]
    E5 --> F
    
    F --> G[Visualizar Resultados]
    G --> H[Fin]
    
    style A fill:#90EE90
    style H fill:#FFB6C1
    style D fill:#FFE4B5
    style E fill:#ADD8E6
    style C fill:#FFD700
    style G fill:#DDA0DD
