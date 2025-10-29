```mermaid
graph TD
    Start([Inicio]) --> LoadRef{¿Usar imagen<br/>de referencia?}
    
    LoadRef -->|Sí| LoadImg[Cargar imagen externa]
    LoadRef -->|No| CaptureROI[Capturar ROI central<br/>de la cámara]
    
    LoadImg --> ConvertHSV[Convertir a HSV]
    CaptureROI --> ConvertHSV
    
    ConvertHSV --> ApplyMask[Aplicar máscara de filtrado<br/>S_min, V_min]
    ApplyMask --> CalcHist[Calcular histograma<br/>del canal Hue]
    CalcHist --> NormHist[Normalizar histograma<br/>0-255]
    
    NormHist --> InitWindow[Inicializar ventana<br/>de búsqueda]
    InitWindow --> LoopStart([Bucle principal])
    
    LoopStart --> CaptureFrame[Capturar frame<br/>de cámara]
    CaptureFrame --> FrameHSV[Convertir frame a HSV]
    FrameHSV --> BackProj[Calcular backprojection<br/>cv2.calcBackProject]
    
    BackProj --> Algorithm{Algoritmo}
    
    Algorithm -->|MeanShift| MeanShift[Ejecutar MeanShift<br/>Ventana fija]
    Algorithm -->|CAMShift| CAMShift[Ejecutar CAMShift<br/>Ventana adaptativa]
    
    MeanShift --> DrawRect[Dibujar rectángulo<br/>de seguimiento]
    CAMShift --> CalcMoments[Calcular momentos<br/>y orientación]
    CalcMoments --> DrawEllipse[Dibujar elipse rotada<br/>y rectángulo]
    
    DrawRect --> Display[Mostrar resultados:<br/>Video, backprojection,<br/>info, referencia]
    DrawEllipse --> Display
    
    Display --> CheckKey{Tecla presionada}
    
    CheckKey -->|Q| End([Fin])
    CheckKey -->|R| InitWindow
    CheckKey -->|Ninguna| LoopStart
    
    style Start fill:#90EE90
    style End fill:#FFB6C6
    style Algorithm fill:#FFD700
    style MeanShift fill:#87CEEB
    style CAMShift fill:#FFA500
    style Display fill:#DDA0DD
