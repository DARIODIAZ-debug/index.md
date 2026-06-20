```mermaid
flowchart LR
    %% Estilos de Nodos
    classDef header fill:#2b2d42,stroke:#2b2d42,stroke-width:2px,color:#fff;
    classDef content fill:#edf2f4,stroke:#8d99ae,stroke-width:1px,color:#2b2d42;
    classDef process fill:#f4a261,stroke:#e76f51,stroke-width:2px,color:#fff;

    %% --- COLUMNAS PRINCIPALES (SIPOC) ---

    subgraph S [S - PROVEEDORES / SUPPLIERS]
        direction TB
        S1[Transportación]
        S2[Supply Chain]
        S3[Harvest Plan]
        S4[Maniobristas / Piso]
        S5[Líneas Transportistas]
    end
    class S1,S2,S3,S4,S5 content;

    subgraph I [I - ENTRADAS / INPUTS]
        direction TB
        I1[Programa de Cargas]
        I2[Estimaciones de Harvest Plan]
        I3[Transfers y Destinos]
        I4[Fruta disponible en BY Warehouse]
        I5[Hoja física con Licencias cargadas]
        I6[Consecutivo de Facturas anterior]
    end
    class I1,I2,I3,I4,I5,I6 content;

    subgraph P [P - PROCESO / PROCESS]
        direction TB
        P1[1. Planificación Matutina y Asignación de Facturas]
        P2[2. Coordinación y Solicitud de Camiones]
        P3[3. Ejecución Digital y Check-In en Mobile Link]
        P4[4. Cierre de Carga Física y Acoplamiento de GPS LOCUS]
        P5[5. Trámites Fiscales, Aduanales y Cierre de Turno]
    end
    class P1,P2,P3,P4,P5 process;

    subgraph O_Col [O - SALIDAS / OUTPUTS]
        direction TB
        O1[Cita de Embarque en Dock Center]
        O2[Excel de Complemento Carta Porte]
        O3[Bill of Lading BOL firmado]
        O4[Paquete de Transmisión Aduanal MX/US]
        O5[CFDI timbrado XML y PDF]
        O6[Paquete de 10 Documentos Esenciales]
        O7[Excel Resumen de Cargas del turno]
    end
    class O1,O2,O3,O4,O5,O6,O7 content;

    subgraph C [C - CLIENTES / CUSTOMERS]
        direction TB
        C1[Área de Vigilancia / Seguridad]
        C2[Agencias Aduanales MX y US]
        C3[Cooler de Destino]
        C4[Comercio Exterior y Supply Chain]
        C5[Línea Transportista]
        C6[Coordinador de Packaging / Turno Siguiente]
    end
    class C1,C2,C3,C4,C5,C6 content;

    %% --- CONEXIONES DEL MAPEO ---
    S --> I
    I --> P
    P --> O_Col
    O_Col --> C

    %% Encabezados de Estilo
    class S,I,O_Col,C header;
