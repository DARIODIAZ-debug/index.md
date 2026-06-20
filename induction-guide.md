# 🔰 Guía de Inducción Digital: Operación de Embarques (2026-2027)

¡Bienvenido al equipo! Este diagrama es tu mapa de supervivencia diario. Sigue el flujo de arriba hacia abajo conforme avanza tu jornada laboral. 

Las cajas **Rojas** 🟥 indican problemas de sistema donde debes esperar o levantar ticket; las cajas **Amarillas** 🟨 son decisiones que debes tomar según el escenario.

```mermaid
flowchart TD
    %% Estilos de Nodos para Aprendizaje Rápido
    classDef inicio fill:#2b2d42,stroke:#2b2d42,stroke-width:2px,color:#fff;
    classDef fase fill:#f4a261,stroke:#e76f51,stroke-width:2px,color:#fff;
    classDef proceso fill:#edf2f4,stroke:#8d99ae,stroke-width:1px,color:#2b2d42;
    classDef decision fill:#e9c46a,stroke:#f4a261,stroke-width:2px,color:#2b2d42;
    classDef soporte fill:#ef233c,stroke:#d90429,stroke-width:2px,color:#fff;

    %% --- FASE 1: MAÑANA ---
    subgraph F1 [🌅 PASO A PASO 1: Tu Rutina de la Mañana]
        A[Apertura de Turno] --> B[1. Revisar Programa de Cargas de Transportación]
        B --> C[2. Identificar el último consecutivo de facturas del turno anterior]
        C --> D[3. Asignar los números de facturas correspondientes a los Transfers del día]
        D --> E[4. Compartir el Programa de Cargas con Vigilancia]
        E --> F[5. Validar ítems y destinos en Blue Yonder]
        F --> G{¿El destino coincide\ncon el programa?}
        G -- NO ❌ --> H[Avisar a Supply Chain, resolver y actualizar a Transportación]
        G -- SÍ  --> I[6. Revisar estimaciones de Harvest Plan en Oracle ERP Dashboard]
        I --> J[7. Convertir cantidades a tarimas y validar contra el destino]
        J --> K{¿Cumplimos con las\ncantidades e ítems?}
        K -- NO ❌ --> L[Solicitar dirección a Supply Chain / Evaluar rellenar con otro ítem]
        K -- SÍ  --> M[8. Compartir plan final con el equipo de trabajo]
        M --> N[9. Monitorear 'Receive to Cool' cada 1:45 hrs para alertar al preenfrio]
    end
    class A inicio; class G,K decision; class H,L proceso;

    %% --- FASE 2: TARDE ---
    subgraph F2 [☀️ PASO A PASO 2: Coordinación para el Día Siguiente]
        N --> O[10. Recibir estimaciones de Harvest Plan entre 12:30 y 13:30 pm]
        O --> P[11. Validar datos en Realtime PAB Dashboard de Oracle]
        P --> Q[12. Enviar tabla a Supply Chain para solicitar dirección]
        Q --> R{¿Supply Chain ya\nenvió los Transfers?}
        R -- NO ❌ --> S[Generar Transfers manualmente en Oracle ERP Cloud]
        R -- SÍ  --> T[13. Llenar 'Solicitud de Camión' con nomenclatura destino/transfers]
        S --> T
        T --> U[14. Enviar correo a Transportación ANTES de las 15:00 pm]
    end
    class R decision; class S proceso;

    %% --- FASE 3: EJECUCIÓN ---
    subgraph F3 [📦 PASO A PASO 3: Preparación y Ventana de Sistemas]
        U --> V[15. Revisar disponibilidad de fruta en BY Warehouse]
        V --> W{¿Hay fruta suficiente\npara iniciar?}
        W -- NO ❌ --> X[Detener proceso y ajustar con Supply Chain]
        W -- SÍ  --> Y[16. Entregar Plan de Embarque en tarimas a Maniobristas]
        Y --> Z[17. Pedir a Vigilancia que posicione el camión y abra puertas]
        Z --> AA[18. Actualizar Transfer con cantidades exactas en Oracle Fusion ERP]
        
        %% Validaciones Críticas (Semaforo de errores)
        AA --> AB[19. Validar actualización en Blue Yonder]
        AB --> AC{¿Se actualizó en BY?}
        AC -- NO ❌ --> AD[⚠️ Esperar 15-30 min.\nSi persiste, levantar ticket en Dist Support]
        AC -- SÍ  --> AE[20. Ir a Oracle OTM PROD > Order Management]
        
        AD --> AB
        AE --> AF[21. Validar cantidades y línea transportista]
        AF --> AG{¿Datos correctos en OTM?}
        AG -- NO ❌ --> AH[⚠️ Esperar 15-30 min.\nSi persiste, levantar ticket en Dist Support]
        AG -- SÍ  --> AI[22. Ir a Dock Center y generar Cita de Embarque]
        
        AH --> AE
        AI --> AJ[23. Buscar Transfer en BY Outbound Planner]
        AJ --> AK{¿Se generó\nShipment y Load?}
        AK -- NO ❌ --> AL[⚠️ Esperar 15-30 min.\nSi persiste, levantar ticket en Dist Support]
        AK -- SÍ  --> AM[24. Abrir Mobile Link > Fruit > Outbound Loading]
        
        AL --> AJ
        AM --> AN[25. Hacer Check-In capturando: Chofer, Línea, Puerta y FACTURA ASIGNADA]
    end
    class W,AC,AG,AK decision; class AD,AH,AL soporte; class X proceso;

    %% --- FASE 4 Y 5: DOCUMENTOS Y ANDEN ---
    subgraph F4_F5 [🚛 PASO A PASO 4: Papeles, Carga Física y Candado GPS]
        AN --> AO[26. Entrar a plataforma INTRADE]
        AO --> AP[27. Seleccionar Factura asignada y cargar ítems/cantidades]
        AP --> AQ[28. Validar y anotar totales de peso, precio y cantidad]
        AQ --> AR[29. Capturar datos en Excel de Complemento Carta Porte]
        AR --> AS{¿Totales de Excel\ncoinciden con Factura?}
        AS -- NO ❌ --> AT[Regresar a INTRADE, revisar precios por caja y corregir Excel]
        AS -- SÍ  --> AU[30. Enviar correo automático con el archivo a la línea transportista]
        
        %% Carga física
        AU --> AV[31. El equipo de maniobristas termina la carga física en andén]
        AV --> AW[32. Recibir hoja física con las licencias de las tarimas cargadas]
        AW --> AX[33. Capturar licencias en Mobile Link hacia la puerta seleccionada]
        AX --> AY[34. Validar totales de la hoja física vs Plan del Sistema]
        AY --> AZ{¿Los totales coinciden?}
        AZ -- NO ❌ --> BA[Buscar puerta en BY, revisar consolidación y cuadrar físico vs sistema]
        AZ -- SÍ  --> BB[35. Tomar un Recorder GPS físico]
        
        BA --> AX
        BB --> BC[36. Registrar en Mobile Link: No. LOCUS + CÓDIGO INVISIBLE]
        BC --> BD[37. Seleccionar EMBARCAR en el sistema]
        BD --> BE[38. Verificar que el Bill of Lading BOL sea correcto]
        BE --> BF{¿BOL Correcto?}
        BF -- NO ❌ --> BG[⚠️ Levantar ticket en Dist Support y esperar indicación]
        BF -- SÍ  --> BH[39. Recabar firma del operador y firmar tú en turno]
    end
    class AS,AZ,BF decision; class AT,BA proceso; class BG soporte;

    %% --- FASE 6 Y 7: ADUANAS Y CIERRE ---
    subgraph F6_F7 [🏁 PASO A PASO 5: Trámites Aduanales y Fin de Turno]
        BH --> BI[40. Descargar de BY: BOL PDF, Customs Excel y Shipping Excel]
        BI --> BJ[41. Descargar Loading Variance de Intelligistic y guardar como TEMP]
        BJ --> BK[42. Recibir la Carta Porte CCP revisada del transportista]
        BK --> BL{¿Información de\nla CCP correcta?}
        BL -- NO ❌ --> BM[Solicitar corrección. Si tarda +30 min, escalar a Transportación]
        BL -- SÍ  --> BN[43. Guardar CCP e imprimir 2 veces]
        
        BM --> BK
        BN --> BO[44. Hacer transmisión de datos a Agencias Aduanales MX y US]
        BO --> BP[45. Validar los 3 archivos devuelvos por agencias: Factura, .txt y DS]
        BP --> BQ[46. Generar CFDI en Timbrame agregando info y número de factura]
        BQ --> BR{¿Datos del CFDI\nson correctos?}
        BR -- NO ❌ --> BS[Buscar CFDI por número de factura y cancelar transmisión para repetir]
        BR -- SÍ  --> BT[47. Guardar todos los archivos en tu carpeta de embarques]
        
        BS --> BQ
        BT --> BU[48. Enviar correo masivo con los 10 DOCUMENTOS ESENCIALES]
        BU --> BV{¿Te quedan más\nTransfers programados?}
        BV -- SÍ  --> Y
        BV -- NO ❌ --> BW[49. Llenar el archivo Excel 'Resumen de Cargas']
        BW --> BX[50. Enviar Resumen de Cargas a Transportación, Cooler y Packaging]
        BX --> BY_Fin[¡Turno Concluido exitosamente!]
    end
    class BL,BR,BV decision; class BS,BM proceso; class BY_Fin inicio;

    %% Conexiones directas de retorno si hay bucles de transfers
    classDef default fill:#edf2f4,stroke:#8d99ae,stroke-width:1px,color:#2b2d42;
