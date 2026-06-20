```mermaid
flowchart TD
    %% Estilos de Nodos
    classDef inicio C_Fin fill:#2b2d42,stroke:#2b2d42,stroke-width:2px,color:#fff;
    classDef fase fill:#f4a261,stroke:#e76f51,stroke-width:2px,color:#fff;
    classDef proceso fill:#edf2f4,stroke:#8d99ae,stroke-width:1px,color:#2b2d42;
    classDef decision fill:#e9c46a,stroke:#f4a261,stroke-width:2px,color:#2b2d42;
    classDef soporte fill:#ef233c,stroke:#d90429,stroke-width:1px,color:#fff;

    %% --- FASE 1 ---
    subgraph F1 [FASE 1: Planificación y Apertura de Turno]
        A[Inicio de Turno] --> B[Revisar Programa de Cargas enviado por Transportación]
        B --> C[Revisar Resumen del turno anterior e identificar Consecutivo de Facturas]
        C --> D[Asignar números de facturas a los Transfers del día]
        D --> E[Compartir Programa de Cargas con Vigilancia]
        E --> F[Revisar en Blue Yonder ítems de cada Transfer]
        F --> G{¿Coincide destino con el programa?}
        G -- No --> H[Solicitar información a Supply Chain y actualizar a Transportación]
        G -- Sí --> I[Revisar estimaciones de Harvest Plan en Oracle ERP Dashboard]
        I --> J[Convertir cantidades a tarimas y validar contra destinos]
        J --> K{¿Cumple con cantidades e ítems?}
        K -- No --> L[Solicitar dirección a Supply Chain / Evaluar rellenar carga con otro ítem]
        K -- Sí --> M[Compartir estimaciones del día y plan con el equipo]
        M --> N[Monitorear Receive to Cool cada 1:45 hrs para alertar tiempo de preenfrio]
    end
    class A inicio; class G,K decision; class H,L,N proceso;

    %% --- FASE 2 ---
    subgraph F2 [FASE 2: Coordinación del Día Siguiente]
        O[Recibir estimaciones de Harvest Plan 12:30 - 13:30 pm] --> P[Revisar en Realtime PAB Dashboard de Oracle]
        P --> Q[Enviar tabla de estimaciones a Supply Chain solicitando dirección]
        Q --> R{¿Supply Chain envía Transfers listos?}
        R -- No --> S[Generar Transfers manualmente en Oracle ERP Cloud]
        R -- Sí --> T[Abrir Solicitud de Camión y agregar nomenclatura de destino y transfers]
        S --> T
        T --> U[Enviar correo a Transportación antes de las 15:00 pm]
    end
    class R decision; class O,P,Q,S,T,U proceso;

    %% --- FASE 3 ---
    subgraph F3 [FASE 3: Preparación y Execution de Embarques]
        V[Revisar en Blue Yonder Warehouse disponibilidad de fruta] --> W{¿Suficiente fruta para iniciar?}
        W -- No --> X[Detener / Ajustar plan con Supply Chain]
        W -- Sí --> Y[Generar Plan de Embarque en tarimas y entregar a Maniobristas]
        Y --> Z[Solicitar a Vigilancia que operador de Transfer posicione unidad y abra puertas]
        Z --> AA[Actualizar Transfer con cantidades exactas en Oracle Fusion ERP Cloud]
        
        AA --> AB[Validar actualización en Blue Yonder]
        AB --> AC{¿Actualizado en BY?}
        AC -- No --> AD[Esperar 15-30 min o levantar ticket en Dist Support]
        AC -- Sí --> AE[Ir a Oracle OTM PROD > Order Management]
        
        AD --> AB
        AE --> AF[Validar cantidades y línea transportista]
        AF --> AG{¿Datos Correctos en OTM?}
        AG -- No --> AH[Esperar 15-30 min o levantar ticket en Dist Support]
        AG -- Sí --> AI[Ir a OTM Navigator > Dock Center y generar Cita de Embarque]
        
        AH --> AE
        AI --> AJ[Ir a BY Extension Dashboard > Outbound Planner y buscar Transfer]
        AJ --> AK{¿Shipment y Load generados?}
        AK -- No --> AL[Esperar 15-30 min o levantar ticket en Dist Support]
        AK -- Sí --> AM[Ir a Mobile Link > Seleccionar Bodega > Outbound Loading]
        
        AL --> AJ
        AM --> AN[Hacer Check-In: Operador, Línea, Factura consecutiva y Puerta]
    end
    class W,AC,AG,AK decision; class AD,AH,AL soporte; class V,Y,Z,AA,AB,AE,AF,AI,AJ,AM,AN proceso;

    %% --- FASE 4 ---
    subgraph F4 [FASE 4: Documentación Local y Proceso Fiscal]
        AO[Ingresar a la nube de INTRADE] --> AP[Seleccionar Factura asignada y agregar ítems y cantidades]
        AP --> AQ[Verificar totales de peso, precio y cantidad]
        AQ --> AR[Ir a Excel > Complemento Carta Porte y capturar datos]
        AR --> AS{¿Coinciden los totales de Excel vs Factura?}
        AS -- No --> AT[Regresar a INTRADE, verificar precios por caja y actualizar Excel]
        AS -- Sí --> AU[Enviar correo automatizado con el archivo adjunto a la línea transportista]
    end
    class AS decision; class AO,AP,AQ,AR,AT,AU proceso;

    %% --- FASE 5 ---
    subgraph F5 [FASE 5: Cierre Físico y Embarque en Sistema]
        AV[Maniobristas terminan de cargar el tráiler] --> AW[Recibir hoja física con licencias de tarimas]
        AW --> AX[Ingresar a Mobile Link y alimentar las licencias mandándolas a la puerta]
        AX --> AY[Verificar totales de la hoja vs Plan del Sistema]
        AY --> AZ{¿Coinciden los totales?}
        AZ -- No --> BA[Buscar puerta en BY, verificar consolidación y cuadrar inventario físico vs sistema]
        AZ -- Sí --> BB[Tomar un Recorder GPS físico]
        
        BA --> AX
        BB --> BC[Agregar GPS en Mobile Link con la numeración LOCUS + Código Invisible]
        BC --> BD[Seleccionar EMBARCAR en el sistema]
        BD --> BE[Verificar que el Bill of Lading BOL muestre la información correcta]
        BE --> BF{¿BOL Correcto?}
        BF -- No --> BG[Crear ticket en Dist Support y esperar indicación]
        BF -- Sí --> BH[Solicitar firma del operador y firmar encargado en turno]
    end
    class AZ,BF decision; class BG soporte; class AV,AW,AX,AY,BA,BB,BC,BD,BE,BH proceso;

    %% --- FASE 6 Y 7 ---
    subgraph F6_F7 [FASE 6 y 7: Reportes, Aduanas y Cierre de Turno]
        BI[Descargar reportes en Blue Yonder: BOL PDF, Customs Excel, Shipping Detail Excel] --> BJ[Ingresar a Intelligistic y guardar Loading Variance Report como TEMP]
        BJ --> BK[Recibir Complemento Carta Porte CCP de la línea transportista]
        BK --> BL{¿Información de CCP correcta?}
        BL -- No --> BM[Solicitar corrección / Escalar a Transportación si tarda mas de 30 min]
        BL -- Sí --> BN[Guardar CCP e imprimir 2 veces]
        
        BM --> BK
        BN --> BO[Realizar transmisión de datos a Agencias Aduanales MX y US]
        BO --> BP[Recibir y validar 3 archivos de las agencias: Factura, .txt, DS]
        BP --> BQ[Generar CFDI del embarque en la plataforma de Timbrame en XML y PDF]
        BQ --> BR{¿Información del CFDI coincide?}
        BR -- No --> BS[Buscar CFDI por número de factura y cancelar la transmisión]
        BR -- Sí --> BT[Guardar archivos en carpeta local de documentos de embarque]
        
        BS --> BQ
        BT --> BU[Enviar correo masivo con los 10 documentos esenciales]
        BU --> BV{¿Hay más Transfers pendientes?}
        BV -- Sí --> Y
        BV -- No --> BW[Llenar archivo Excel Resumen de Cargas]
        BW --> BX[Enviar correo de Resumen de Cargas a Transportación, Cooler y Packaging]
        BX --> BY_Fin[Fin del Turno]
    end
    class BL,BR,BV decision; class BS,BM proceso; class BI,BJ,BK,BN,BO,BP,BQ,BT,BU,BW,BX proceso; class BY_Fin inicio;

    %% Uniones entre Fases principales
    N --> O
    U --> V
    AN --> AO
    AU --> AV
    BH --> BI
