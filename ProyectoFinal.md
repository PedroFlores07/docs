```mermaid
flowchart TD
    Start([INICIO]) --> Init[Inicializar Sistema]
    Init --> LoadCalib[Cargar Calibración y Umbrales]
    LoadCalib --> InitMediaPipe[Inicializar MediaPipe Hands]
    InitMediaPipe --> InitBuffers[Inicializar Buffers de Historia<br/>predictions, landmarks, finger_states]
    InitBuffers --> OpenCam[Abrir Cámara]
    OpenCam --> ShowGuide[Mostrar Guía de Señas]
    ShowGuide --> MainLoop{Loop Principal<br/>Activo?}
    
    MainLoop -->|No| Cleanup[Liberar Recursos]
    Cleanup --> End([FIN])
    
    MainLoop -->|Sí| CheckPause{¿Pausado?}
    CheckPause -->|Sí| Display
    CheckPause -->|No| CaptureFrame[Capturar Frame]
    
    CaptureFrame --> FlipFrame[Voltear Frame<br/>Espejo]
    FlipFrame --> ConvertRGB[Convertir BGR a RGB]
    ConvertRGB --> MediaPipeProcess[MediaPipe:<br/>Detectar Mano]
    
    MediaPipeProcess --> HandDetected{¿Mano<br/>Detectada?}
    
    HandDetected -->|No| SetDefault[prediction = '?'<br/>confidence = 0.0]
    SetDefault --> Display
    
    HandDetected -->|Sí| DrawLandmarks[Dibujar Landmarks<br/>en Frame]
    DrawLandmarks --> ExtractLandmarks[Extraer 21 Puntos<br/>de la Mano]
    
    ExtractLandmarks --> SmoothLandmarks[Suavizar Landmarks<br/>Promedio Temporal<br/>últimos 3-5 frames]
    
    SmoothLandmarks --> CalcPalmSize[Calcular Tamaño<br/>de Palma<br/>dist muñeca-medio MCP]
    
    CalcPalmSize --> DetectFingers[Detectar Estado<br/>de Dedos]
    
    DetectFingers --> ThumbCheck{¿Es<br/>Pulgar?}
    
    ThumbCheck -->|Sí| ThumbAnalysis[Análisis Especial Pulgar:<br/>- Distancia horizontal<br/>- Distancia a palma<br/>- Progresión articulaciones]
    ThumbAnalysis --> ThumbState[Estado: Extendido=1<br/>Flexionado=0]
    
    ThumbCheck -->|No| NormalFinger[Análisis Normal:<br/>Comparar tip_y vs pip_y]
    NormalFinger --> FingerState[Estado: Extendido=1<br/>Flexionado=0]
    
    ThumbState --> SmoothStates
    FingerState --> SmoothStates[Suavizar Estados<br/>Votación Mayoritaria<br/>últimos 3 frames]
    
    SmoothStates --> CalcFeatures[Calcular Features:]
    CalcFeatures --> Distances[Distancias:<br/>thumb-index, index-middle<br/>middle-ring, ring-pinky]
    Distances --> Angles[Ángulos:<br/>ángulo del pulgar]
    Angles --> Curvature[Curvatura de Dedos]
    Curvature --> Curl[Curl Flexión]
    Curl --> Openness[Apertura de Mano]
    Openness --> Orientation[Orientación:<br/>Vertical/Horizontal<br/>usando PCA]
    
    Orientation --> AnalyzeGesture[ANÁLISIS DE GESTO]
    
    AnalyzeGesture --> CheckA{¿Es A?<br/>Puño cerrado<br/>pulgar extendido}
    CheckA -->|Sí| ReturnA[Retornar 'A'<br/>confianza 0.9]
    CheckA -->|No| CheckG{¿Es G?<br/>Índice horizontal<br/>pulgar extendido}
    
    CheckG -->|Sí| ReturnG[Retornar 'G'<br/>confianza 0.9]
    CheckG -->|No| CheckB{¿Es B?<br/>Mano abierta<br/>baja curvatura}
    
    CheckB -->|Sí| ReturnB[Retornar 'B'<br/>confianza 0.9]
    CheckB -->|No| CheckOthers[Verificar otras letras:<br/>C, D, E, F, H, I, L<br/>O, U, V, W, Y, S]
    
    CheckOthers --> LetterMatch{¿Coincide<br/>alguna?}
    LetterMatch -->|Sí| ReturnLetter[Retornar Letra<br/>y Confianza]
    LetterMatch -->|No| ReturnUnknown[Retornar '?'<br/>confianza 0.3]
    
    ReturnA --> AddToHistory
    ReturnG --> AddToHistory
    ReturnB --> AddToHistory
    ReturnLetter --> AddToHistory
    ReturnUnknown --> AddToHistory
    
    AddToHistory[Agregar a<br/>Historial de Predicciones]
    AddToHistory --> SmoothPrediction[Suavizar Predicción:<br/>Votación Ponderada<br/>últimos 15 frames]
    
    SmoothPrediction --> DrawFingerStates[Dibujar Estados<br/>de Dedos]
    DrawFingerStates --> DrawOrientation[Dibujar Info<br/>de Orientación]
    DrawOrientation --> CheckDebug{¿Modo<br/>Debug?}
    
    CheckDebug -->|Sí| DrawDebug[Mostrar Métricas:<br/>- Estados dedos<br/>- Distancias<br/>- Info pulgar<br/>- FPS]
    CheckDebug -->|No| Display
    DrawDebug --> Display
    
    Display[Dibujar Interfaz:<br/>- Letra detectada<br/>- Confianza<br/>- Barra progreso<br/>- Controles]
    
    Display --> CalcFPS[Calcular FPS]
    CalcFPS --> ShowWindow[Mostrar en Ventana]
    
    ShowWindow --> CheckKey{Tecla<br/>Presionada?}
    
    CheckKey -->|Q| Cleanup
    CheckKey -->|D| ToggleDebug[Toggle Debug Mode]
    CheckKey -->|S| SaveExample[Guardar Ejemplo<br/>para Entrenamiento]
    CheckKey -->|ESPACIO| TogglePause[Toggle Pausa]
    CheckKey -->|Ninguna| MainLoop
    
    ToggleDebug --> MainLoop
    SaveExample --> MainLoop
    TogglePause --> MainLoop
    
    style Start fill:#90EE90
    style End fill:#FFB6C1
    style AnalyzeGesture fill:#87CEEB
    style MediaPipeProcess fill:#DDA0DD
    style SmoothPrediction fill:#F0E68C
    style Display fill:#FFD700
