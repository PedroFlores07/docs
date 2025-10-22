```mermaid
graph TD
    A[Inicio] --> B[Cargar Imagen]
    
    subgraph "ALGORITMO RGB"
    B --> C1[Separar canales B, G, R]
    C1 --> D1[Crear máscara R: R_min ≤ R ≤ R_max]
    D1 --> E1[Crear máscara G: G_min ≤ G ≤ G_max]
    E1 --> F1[Crear máscara B: B_min ≤ B ≤ B_max]
    F1 --> G1[Combinar máscaras con AND]
    G1 --> H1[Convertir a uint8 * 255]
    H1 --> I1[Aplicar máscara con bitwise_and]
    I1 --> J1[Resultado RGB]
    end
    
    subgraph "ALGORITMO HSL"
    B --> C2[Normalizar imagen a 0-1]
    C2 --> D2[Separar canales B, G, R]
    D2 --> E2[Calcular C_max, C_min, delta]
    E2 --> F2[Calcular L = C_max + C_min / 2]
    F2 --> G2[Calcular S según delta y L]
    G2 --> H2[Calcular H según canal dominante]
    H2 --> I2[Crear máscara H: H_min ≤ H ≤ H_max]
    I2 --> J2[Crear máscara S: S_min ≤ S ≤ S_max]
    J2 --> K2[Crear máscara L: L_min ≤ L ≤ L_max]
    K2 --> L2[Combinar máscaras con AND]
    L2 --> M2[Convertir a uint8 * 255]
    M2 --> N2[Aplicar máscara con bitwise_and]
    N2 --> O2[Resultado HSL]
    end
    
    J1 --> Z[Fin]
    O2 --> Z
    
    style A fill:#90EE90
    style Z fill:#FFB6C1
    style J1 fill:#87CEEB
    style O2 fill:#87CEEB