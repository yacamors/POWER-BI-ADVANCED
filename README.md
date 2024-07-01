# POWER-BI-ADVANCED DAX
Columnas     
 Funciones de Fecha y Tiempo

Año = YEAR(DimCalendar[DateKey])

Mes = MONTH(DimCalendar[DateKey])

Dia = DAY(DimCalendar[DateKey])

Trimestre = QUARTER(DimCalendar[DateKey])

SemanaAño = WEEKNUM(DimCalendar[DateKey],2)

DiaSemana = WEEKDAY(DimCalendar[DateKey],2)

DiasPromo = DATEDIFF(DimPromotion[StartDate], DimPromotion[EndDate], DAY)

Módulo: Funciones Condicionales y Lógicas

CantidadTotal = FactSales[SalesQuantity] - FactSales[ReturnQuantity]

Devolución = IF(FactSales[ReturnQuantity]>0,"Si","No")

TipoVenta = IF(FactSales[SalesQuantity]>10,"Mayoreo","Menudeo")

TipoDevolucion = IF(FactSales[ReturnQuantity]=0,BLANK(),IF(FactSales[ReturnQuantity]=1,"Unica","Multiple"))

FinSemana = IF( NOT DimCalendar[DiaSemana] in {1,2,3,4,5} ,"Fin de Semana", "Día Laboral")

MalaSuerte = IF((DimCalendar[CalendarDayOfWeekLabel]="Tuesday" || DimCalendar[CalendarDayOfWeekLabel]="Friday") && DimCalendar[Dia]=13,"Mala Suerte", "Día Normal")

DctoMayoreo = IF(FactSales[TipoVenta] = "Mayoreo" && FactSales[channelKey] = 1 && FactSales[PromotionKey] = 1,0.05,0)

 Funciones de Texto

Ubicación Completa = DimGeography[RegionCountryName] & DimGeography[ContinentName]

Ubicación Completa = DimGeography[RegionCountryName] & ", " & DimGeography[ContinentName]

Trimestre = "Trimestre " & QUARTER(DimCalendar[DateKey])

MesCorto = LEFT(DimCalendar[CalendarMonthLabel],1)

MesTxt = FORMAT(DimCalendar[DateKey], "MMMM")

DiaTxt = FORMAT(DimCalendar[DateKey], "DDDD")

DiaTxt = UPPER(FORMAT(DimCalendar[DateKey], "DDDD"))

Tienda = SUBSTITUTE(DimStores[StoreName], "Store", BLANK())

Módulo: RELATED

Ubicacion Fisica = RELATED(DimGeography[Ubicación Completa])

Ubicacion Fisica = DimStores[Ciudad] & ", " & RELATED(DimGeography[Ubicación Completa])

Ubicacion Fisica = IF(DimStores[StoreType]="Store", DimStores[Ciudad] & ", " & RELATED(DimGeography[Ubicación Completa]), BLANK())

Precio Unitario = RELATED(DimProduct[UnitPrice])

Ingresos = FactSales[Precio Unitario] * FactSales[CantidadTotal]

Medidas     	

Cantidad Ventas = SUM(FactSales[SalesQuantity])

Cantidad Devoluciones = SUM(FactSales[ReturnQuantity])

Cantidad Total = [Cantidad Ventas] - [Cantidad Devoluciones]

Ratio Devoluciones = DIVIDE([Cantidad Devoluciones],[Cantidad Ventas],0)

Ratio Devoluciones = “La cantidad de devoluciones respecto a ventas es: “ & DIVIDE([Cantidad Devoluciones],[Cantidad Ventas],0)

PU Promedio = AVERAGE(DimProduct[UnitPrice])

Cantidad Tiendas = COUNT(DimStores[StoreKey])

Cantidad Tiendas = COUNTA(DimStores[StoreKey])

Cantidad Tiendas = COUNTROWS(DimStores)

Cantidad Tiendas con Ventas = DISTINCTCOUNT(DimStores[StoreKey])

Cantidad Regiones = COUNTA(DimGeography[RegionCountryName])

Cantidad Regiones 2 = COUNTROWS(DimGeography)

Cantidad Regiones en Blanco = COUNTBLANK(DimGeography[RegionCountryName])

Cantidad Regiones Unicas = DISTINCTCOUNT(DimGeography[RegionCountryName])

Total Ordenes = COUNTROWS(FactSales)


 CALCULATE

Total Ordenes Devueltas = CALCULATE([Total Ordenes], FactSales[ReturnQuantity] > 0)

Total Ordenes Devueltas Unica = CALCULATE([Total Ordenes], FactSales[ReturnQuantity] > 0, FactSales[TipoDevolucion] = "Unica")

Módulo: CALCULATE ALL / FILTER

ALL Total Ordenes = CALCULATE([Total Ordenes],ALL(FactSales))

% Ordenes Devueltas = DIVIDE([Total Ordenes Devueltas],[ALL Total Ordenes])

PU Promedio General = CALCULATE([PU Promedio], ALL(DimProduct))

Total Ordenes Devueltas Unica FILTER = CALCULATE([Total Ordenes], FILTER(FactSales, FactSales[ReturnQuantity] > 0), FILTER(FactSales, FactSales[TipoDevolucion] = "Unica"))

Total Ordenes PU Alto = CALCULATE([Total Ordenes], FILTER(DimProduct, DimProduct[UnitPrice] > [PU Promedio General]))


