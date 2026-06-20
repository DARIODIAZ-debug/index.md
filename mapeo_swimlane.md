```mermaid
flowchart LR
    %% Estilos de Nodos
    classDef inicio C_Fin fill:#2b2d42,stroke:#2b2d42,stroke-width:2px,color:#fff;
    classDef proceso fill:#edf2f4,stroke:#8d99ae,stroke-width:1px,color:#2b2d42;
    classDef decision fill:#e9c46a,stroke:#f4a261,stroke-width:2px,color:#2b2d42;
    classDef soporte fill:#ef233c,stroke:#d90429,stroke-width:1px,color:#fff;

    %% --- CARRILES (SWIMLANES) POR DEPARTAMENTO ---

    subgraph CARRIEL_TRANSPORTACION [CARRIEL: Transportación y Supply Chain]
        B[Revisar Programa de Cargas del día]
        H[Solicitar info de modificaciones]
        L[Solicitar dirección de ítems/relleno]
        Q[Enviar tabla de estimaciones para mañana]
        S[Generar Transfers en Oracle ERP]
        U[Enviar Solicitud de Camión antes 15:00]
        BM[Escalar retraso de Carta Porte]
    end

    subgraph CARRIEL_EMBARQUES [CARRIEL: Operación de Embarques / Usuario]
        A[Inicio de Turno] 
        C[Identificar Consecutivo de Facturas]
        D[Asignar facturas a Transfers]
        F[Revisar ítems en Blue Yonder]
        I[Revisar estimaciones en Oracle Dashboard]
        J[Convertir a tarimas y validar cantidad]
        M[Compartir plan con el equipo]
        N[Monitorear Receive to Cool cada 1:45 hrs]
        V[Auditar disponibilidad de fruta en BY]
        Y[Generar Plan en tarimas]
        AA[Actualizar Transfer en Oracle Fusion]
        
        %% Validaciones de Sistemas
        AB[Validar en Blue Yonder]
        AE[Ir a Oracle OTM PROD]
        AF[Validar cantidades y transportista]
        AI[Generar Cita en Dock Center]
        AJ[Buscar Transfer en Outbound Planner]
        AM[Entrar a Mobile Link > Outbound Loading]
        AN[Hacer Check-In con Factura Asignada]
        
        %% Documentación e Intrade
        AO[Ingresar a INTRADE]
        AP[Seleccionar Factura y cargar ítems]
        AQ[Tomar totales de peso y precio]
        AR[Capturar datos en Excel Carta Porte]
        AT[Corregir precios por caja en INTRADE]
        AU[Enviar correo automático a Transportista]
        
        %% Cierre Físico y GPS
        AX[Capturar licencias físicas en Mobile Link]
        AY[Verificar totales Hoja vs Plan]
        BA[Cuadrar inventario físico vs sistema en BY]
        BB[Tomar Recorder GPS físico]
        BC[Registrar LOCUS + Código Invisible]
        BD[Seleccionar EMBARCAR en sistema]
        BE[Verificar Bill of Lading BOL]
        BH[Firmar documentación de salida]
        
        %% Reportes y Cierre
        BI[Descargar BOL, Customs y Shipping de BY]
        BJ[Guardar Loading Variance de Intelligistic]
        BK[Recibir y verificar CCP de Transportista]
        BN[Guardar CCP e imprimir 2 veces]
        BO[Transmitir datos a Aduanas MX/US]
        BP[Validar Factura, .txt y DS]
        BQ[Generar CFDI en Timbrame]
        BS[Cancelar transmisión en Timbrame si hay error]
        BT[Guardar paquete de 10 documentos]
        BU[Enviar correo masivo de cierre]
        BW[Llenar archivo Excel Resumen de Cargas]
        BX[Enviar correo final a Cooler/Packaging]
    end

    subgraph CARRIEL_SISTEMAS [CARRIEL: Soporte IT - Dist Support]
        AD[Levantar Ticket Soporte BY]
        AH[Levantar Ticket Soporte OTM]
        AL[Levantar Ticket Soporte Planner]
        BG[Levantar Ticket Soporte BOL]
    end

    subgraph CARRIEL_PISO [CARRIEL: Vigilancia y Maniobristas]
        E[Recibir programa de cargas]
        Z[Operador abre puertas y se posiciona]
        AV[Maniobristas cargan tráiler]
        AW[Entregar hoja física de licencias]
    end

    %% --- FLUJO INTERFUNCIONAL (CONEXIONES ENTRE CARRILES) ---
    A --> B
    B --> C --> D
    D --> E
    E --> F
    F --> G{¿Coincide destino?}
    G -- No --> H --> F
    G -- Sí --> I --> J
    J --> K{¿Cumple cantidad?}
    K -- No --> L --> J
    K -- Sí --> M --> N
    
    %% Flujo tarde (Mañana)
    N --> O[Recibir estimaciones mañana] --> P[Revisar Realtime Dashboard] --> Q
    Q --> R{¿Transfers listos?}
    R -- No --> S --> T[Preparar Solicitud de Camión]
    R -- Sí --> T
    T --> U --> V
    
    %% Ejecución
    V --> W{¿Suficiente fruta?}
    W -- No --> X[Ajustar Plan]
    W -- Sí --> Y --> Z --> AA
    
    %% Círculo de sistemas
    AA --> AB --> AC{¿Actualizado?}
    AC -- No --> AD --> AB
    AC -- Sí --> AE --> AF --> AG{¿Datos OK?}
    AG -- No --> AH --> AE
    AG -- Sí --> AI --> AJ --> AK{¿Load listo?}
    AK -- No --> AL --> AJ
    AK -- Sí --> AM --> AN
    
    %% Fiscal e Intrade
    AN --> AO --> AP --> AQ --> AR --> AS{¿Coinciden totales?}
    AS -- No --> AT --> AR
    AS -- Sí --> AU --> AV --> AW --> AX
    
    %% Cierre Físico y GPS
    AX --> AY --> AZ{¿Coinciden?}
    AZ -- No --> BA --> AX
    AZ -- Sí --> BB --> BC --> BD --> BE --> BF{¿BOL OK?}
    BF -- No --> BG --> BE
    BF -- Sí --> BH --> BI
    
    %% Aduanas y Cierre
    BI --> BJ --> BK --> BL{¿CCP Correcta?}
    BL -- No --> BM --> BK
    BL -- Sí --> BN --> BO --> BP --> BQ --> BR{¿CFDI OK?}
    BR -- No --> BS --> BQ
    BR -- Sí --> BT --> BU --> BV{¿Más Transfers?}
    BV -- Sí --> Y
    BV -- No --> BW --> BX --> BY_Fin[Fin del Turno]

    %% Asignación de clases iniciales
    class A inicio; class G,K,R,W,AC,AG,AK,AS,AZ,BF,BL,BR,BV decision;
    class AD,AH,AL,BG soporte;
