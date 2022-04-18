# FINANCE KPI


En este tablero estaremos hablando de gastos, ingresos, utilidad y saldo. Información de interés para distintos roles, como puede ser un jefe de administración y finanzas, un líder de área o a niveles gerenciales superiores como un CFO.

## ¿Qué me permite visualizar?

Este tablero tiene como finalidad evaluar el comportamiento de las ventas de los años 2019 y 2020. Observaremos las siguientes medidas:
- Ingresos + Detalle de ingresos
- Gastos + Detalle de gastos
- Utilidad + detalle de utilidad
- Saldo
- Medidas generales

## ¿Qué me permite medir?
- % de ingresos obtenidos vs esperados
- Desviaciones de gastos
- % de utilidad obtenida vs esperada
- Saldo final y sus variaciones


## Fuente de Datos

> Modelo de datos

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/DB.jpg)

El proyecto parte de información suministrada en un archivo Excel, la cual tuvo que sufrir varias transformaciones para poder ser estructurada de forma tal que se pueda explotar de la forma más eficiente.

Se crearon las siguientes tablas:

**Conjunto de tablas:**
- Finanzas
- Categoria
- Expectativa
- Calendarios

Se realizó limpieza de archivos Excel, eliminando columnas innecesarias y reubicando datos.   
Para que el tablero sea eficiente y pueda recibir actualización de la información de forma simple y fiable, inicialmente se parte de un acuerdo en el que se indica que no se va a cambiar el formato de los datos de ingreso, por lo que se crean 4 tablas, que no serán visibles al usuario final, sino que alimentarán las tablas finales.

**Unificación de tablas del origen:**
- Metas y Presupuestos: Ya que son las medidas presupuestarias a las que queremos llegar estarán en la tabla "Expectativa" 
- Gastos e Ingresos: Estarán en nuestra tabla Finanzas, en la que tendremos una columna que indica el origen de la información


**Medidas DAX:**

| Nombre de Campo  | Cálculo |
| :------------ | :------------|
|% Cuota Gastos|[Gastos] / [Presupuesto]|
|% Cuota Ingresos|[Gastos] / [Presupuesto]|
|% Cuota Saldo|[Saldo] / [Saldo Esperado]|
|% Cuota Util.|[Saldo] / [Saldo Esperado]|
|Gastos|CALCULATE ([Total Finanzas], Finanzas[Tipo] = "Gastos")|
|Ingresos|CALCULATE ([Total Finanzas], Finanzas[Tipo] = "Ingresos")|
|Meta|CALCULATE ([Total Expectativa], Expectativa[Tipo] = "Meta")|
|Meta vs Ing|[Ingresos] - [Meta]|
|Pres vs Gastos|[Gastos] - [Presupuesto]|
|Presupuesto|CALCULATE ([Total Expectativa] , Expectativa[Tipo] = "Presupuesto")|
|Saldo| CALCULATE ([Utilidad], FILTER(all(Finanzas), Finanzas[Fecha] <= Max(Finanzas[Fecha])))|
|Saldo Esperado|CALCULATE ([Utilidad esperada], FILTER(all(Finanzas), Finanzas[Fecha] <= Max(Finanzas[Fecha])))|
|Saldo vs Saldo esp.|[Saldo] - [Saldo Esperado]|
|Total Expectativa|[Saldo] - [Saldo Esperado]|
|Total Finanzas|SUM( Finanzas[Cantidad])|
|Util. vs Util. esp.|[Utilidad esperada]- [Utilidad]|
|Utilidad|[Ingresos] - [Gastos]|
|Utilidad esperada|[Meta]- [Presupuesto]|

### Tablero: Página principal

En esta página podremos apreciar los gráficos sobre: Cuota de ingresos, Cuota de gastos, Cuota de utilidad y Cuota de saldo.
Si se selecciona alguno de ellos se podrá acceder a un nivel más detallado.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/F1.jpg)

Detallemos un poco más cada una de las mediciones:

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/F2.jpg)

### Tablero: Medidas

En este panel podremos ver las diferentes medidas financieras mensuales. Cada columna ha mantenido el color del panel principal para ayudar al usuario a ubicarse.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/MEDIDAS.jpg)

## Tablero: Cuota de Ingresos

En este tablero podremos tener el detalle sobre el movimiento de los ingresos en el año y mes seleccionado.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/CUOTAINGRESO.jpg)

Además, haciendo clic sobre el botón "¿Por qué no cumplí  la cuota de ingresos?" podremos acceder a un nivel más detallado:

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/DETALLEINGRESOS.jpg)

## Tablero: Cuota de Gastos

En este tablero podremos tener el detalle sobre los gastos en el año y mes seleccionado. Nos permitirá rápidamente saber, si hemos tenido una desviación y que tan grave ha sido.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/CUOTAGASTOS.jpg)

Además, haciendo clic sobre el botón "¿Por qué sobrepasé la cuota de gastos?" podremos acceder a un nivel más detallado:

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/DETALLEGASTOS.jpg)


## Tablero: Cuota de Utilidad

En este tablero podremos tener el detalle sobre la utilidad en el año y mes seleccionado. Nos permitirá rápidamente saber, si hemos alcanzado o no las metas.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/CUOTAUTILIDAD.jpg)

Además, haciendo clic sobre el botón "Detalle de utilidad" podremos acceder a un nivel más detallado:

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/DETALLEUTILIDAD.jpg)




## Tablero: Cuota de Saldos

En este tablero podremos tener el detalle sobre el saldo en el año y mes seleccionado. Nos permitirá rápidamente saber, si hemos alcanzado o no las metas de acuerdo con la utilidad y el saldo obtenido, versus el esperado.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/FINANCE/CUOTADESALDO.jpg)



Este tablero es producto del curso de **Microsoft Power BI - Curso de Power BI Desktop** de Udemy, en donde se utilizaron las 5 fases fundamentales de Power BI Desktop:  

- Get Data
- Data Preparation
- Data Modeling & DAX
- Data Visualization
- Data Reporting

Es un excelente curso, el cual recomiendo ampliamente.
https://www.udemy.com/course/curso-microsoft-power-bi/#overview
