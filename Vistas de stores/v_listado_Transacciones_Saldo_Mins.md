

```sql
CREATE or ALTER VIEW v_listado_Transacciones_Saldo_Mins
AS
SELECT 
tsm.nId_Colaborador,
    p.sPersona_Nombre AS SNombreColaborador,
    COUNT(tsm.nId_Transaccion) AS Total_Solicitudes, -- Total de solicitudes
    SUM(CASE WHEN tsm.nTipo_Transaccion = 1 THEN tsm.dCantidad_Minutos ELSE 0 END) AS CantidadTotalSuma, -- Sumar permisos tipo 1
    SUM(CASE WHEN tsm.nTipo_Transaccion = 2 THEN tsm.dCantidad_Minutos ELSE 0 END) AS CantidadTotalResta, -- Sumar permisos tipo 2
    SUM(tsm.dCantidad_Minutos) AS CantidadTotalDescontado, -- Total descontado
    CAST(MAX(tsm.dDatetime_Creacion) AS DATE) AS FechaUltimaTransaccion,
    c.sUrl_Foto
    
FROM 
    Transacciones_Saldo_Mins tsm
    JOIN Colaboradores c ON c.nId_Colaborador = tsm.nId_Colaborador 
    JOIN Personas p ON p.nId_Persona = c.nId_Persona 
where tsm.nId_Cierre IS NULL
GROUP BY 
    tsm.nId_Colaborador,
    p.sPersona_Nombre,
    c.sUrl_Foto;
```