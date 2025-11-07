```mermaid
flowchart TD
    Start([Inicio]) --> Init[Cargar base de datos JSON<br/>con momentos de Hu]
    Init --> Camera[Inicializar cámara<br/>VideoCapture]
    Camera --> Capture{Capturar<br/>fotograma}
    
    Capture -->|Error| End([Fin])
    Capture -->|Éxito| Gray[Convertir a escala de grises<br/>cvtColor]
    
    Gray --> Blur[Aplicar desenfoque gaussiano<br/>GaussianBlur 5x5]
    Blur --> Thresh[Umbralización binaria<br/>threshold umbral=200]
    Thresh --> Close[Operación morfológica CLOSE<br/>kernel 5x5]
    Close --> Open[Operación morfológica OPEN<br/>kernel 5x5]
    
    Open --> Contours[Detectar contornos<br/>findContours EXTERNAL]
    Contours --> InitCount[Contador objetos = 0]
    InitCount --> LoopStart{¿Hay más<br/>contornos?}
    
    LoopStart -->|No| Display[Mostrar contador en pantalla<br/>putText]
    LoopStart -->|Sí| NextContour[Tomar siguiente contorno]
    
    NextContour --> AreaCheck{Área entre<br/>2000 y 100000?}
    AreaCheck -->|No| LoopStart
    AreaCheck -->|Sí| Moments[Calcular momentos espaciales<br/>moments]
    
    Moments --> ZeroCheck{m00 ≠ 0?}
    ZeroCheck -->|No| LoopStart
    ZeroCheck -->|Sí| HuCalc[Calcular momentos de Hu<br/>HuMoments]
    
    HuCalc --> LogTrans[Aplicar transformación logarítmica<br/>-sign × log10]
    LogTrans --> InitDist[Mejor clase = None<br/>Menor distancia = ∞]
    InitDist --> ClassLoop{¿Hay más<br/>clases en DB?}
    
    ClassLoop -->|No| DrawRect[Dibujar rectángulo delimitador<br/>rectangle]
    ClassLoop -->|Sí| CalcDist[Calcular distancia euclidiana<br/>con clase actual]
    
    CalcDist --> CompDist{Distancia <<br/>Menor distancia?}
    CompDist -->|No| ClassLoop
    CompDist -->|Sí| UpdateBest[Actualizar mejor clase<br/>y menor distancia]
    UpdateBest --> ClassLoop
    
    DrawRect --> DrawLabel[Dibujar etiqueta de clase<br/>putText]
    DrawLabel --> IncCount[Incrementar contador<br/>objetos]
    IncCount --> LoopStart
    
    Display --> Show[Mostrar ventanas:<br/>Frame original y Binaria<br/>imshow]
    Show --> KeyCheck{Tecla 'q'<br/>presionada?}
    
    KeyCheck -->|No| Capture
    KeyCheck -->|Sí| Release[Liberar cámara<br/>y cerrar ventanas]
    Release --> End
