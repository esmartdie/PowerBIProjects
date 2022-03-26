# IT AGENTS DASHBOARD

##### El paso a paso del análisis de tickets del área de soporte de TI

### ¿Por qué un tablero para gestión de tickets?

Analizar los tickets de un área de soporte es una manera de entender lo que se debe hacer para asegurar la satisfacción de los clientes, pudiendo prever si algo no está funcionando bien antes de que se llegue a convertir en un problema.

Este tablero tiene como finalidad evaluar el desempeño, cantidad de tickets y porcentaje de satisfacción de los usuarios de un área de soporte de IT, que se encarga de evaluar y tomar a cargo incidentes de distintas áreas de una empresa durante un período de 4 años.

### ¿Qué me permite visualizar?

- Visualización rápida y clara sobre cantidad de tickets atendidos en un período
- Posibilidad de una vista por segmentación de tickets
- Permite navegar por la información de forma intuitiva y se podrán exportar los informes de acuerdo con sus preferencias.
- Visualizar gráficos por cantidad de tickets, categoría, satisfacción y nivel de apertura
- Posibilidad de cambiar entre modo oscuro y modo claro


### ¿Qué me permite medir?
- Medición sobre los tickets que se han abierto por fecha 
- Comparativo de tickets según categorías principales
- Total de tickets según el tiempo de duración
- Medir el nivel de satisfacción general de los usuarios


### Fuente de Datos

> Modelo de datos

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/ITAgentsDashboard/BBDD.jpg)

El proyecto parte de una información suministrada en un archivo excel,  la cual tuvo que sufrir varias transformaciones para poder ser estructurarada de forma tal que se pueda explotar de la forma más eficiente.

Se crearon las siguientes tablas:

**Conjunto de tablas DIM:**
- Dim_Agentes de TI
- Dim_Dias
- Dim_Satisfaccion
- Dim_Empleados
- Dim_Calendario  

*Las tablas DIM sufrieron una serie de transformaciones, por ejemplo la fecha de nacimiento, la cual es el concatenado de día, mes y año. Luego hay columnas calculadas como el "Grupo Etario" la cual es una nueva columna, teniendo en cuenta la edad del empleado.*

*En el caso del género del empleado, se utilizó una tabla para obtener la descripción.*  

*También se tiene una conexión por medio de una URL, a información que está contenida en un dropbox.*  

*Se realizó limpieza de archivos excel, eliminando columnas innecesarias y reubicando datos. Las tablas de "Plantas" se unieron en una sola.*    

*Se creo una tabla calendario, de las fechas de operación. Se añadió como campo calculado, una columna que indica si es día laboral o no, el cual tiene en cuenta los fines de semana. Adicionalmente, se creo un campo de "Short Description" para el día de la semana*

**Conjunto de tablas de hecho:**
- Fact_Tickets : Es la conjunción de los 5 archivos excel. Los datos tendrá el dato fuente para conocer su origen.



**Medidas DAX:**

| Nombre de Campo  | Cálculo |
| :------------ | :------------|
|% Positivo | Divide([Total Tickets positivos],[Total_Tickets],0)|
|% Problema | Divide([Total Tickets Problema],[Total_Tickets],0)|
|Promedio dias abiertos | Average(Fact_Tickets[Días Resolución])|
|Promedio Satisfaccion |Average(Fact_Tickets[Satisfacción])|
|RANK dias |rankx(all('Dim_Agentes de TI'),[Promedio dias abiertos],,asc)|
|Rank Final |rankx(all('Dim_Agentes de TI'), [Rank Total],,asc)|
|RANK Satisfaccion | rankx(all('Dim_Agentes de TI'),[Promedio Satisfaccion])|
|Rank Total |[Ranking Tickets] + [RANK dias] + [RANK Satisfaccion] + [Tie Breaker RANKX]|
|Ranking Tickets |Rankx( all('Dim_Agentes de TI'), [Total_Tickets])|
|Ranking Tickets ALLS | Rankx( ALLSELECTED('Dim_Agentes de TI'), [Total_Tickets])|
|Tickets por agente | round(Divide([Total_Tickets],[Total Agentes],0),0)|
|Tickets por Agente All S | calculate ([Tickets por agente], ALLSELECTED(Fact_Tickets))|
|Tickets por agente Var grupo | If ([Tickets por Agente Variacion] >= 0, "Arriba del promedio", "Debajo del promedio")|
|Tickets por Agente Variacion |[Total_Tickets] - [Tickets por Agente All S]|
|Tie Breaker RANKX | divide (convert (Max('Dim_Agentes de TI'[Nacimiento]) , integer), 100000)|
|Total Agentes | countrows('Dim_Agentes de TI')|
|Total Tickets ALL |calculate ([Total_Tickets], ALL(Fact_Tickets))|
|Total Tickets ALL S |calculate ([Total_Tickets], ALLSELECTED(Fact_Tickets))|
|Total Tickets positivos | calculate([Total_Tickets], Dim_Satisfaccion[Estatus] = "Positivo" )|
|Total Tickets Problema | calculate([Total_Tickets], Fact_Tickets[Tipo] = "Problema" )|
|Total_Tickets | if(isblank(countrows(Fact_Tickets)),0,(countrows(Fact_Tickets)))|
|Total_Tickets NOBLANK | Countrows(Fact_Tickets)|

### Tablero: Página principal

En esta página se puede apreciar un resumen de los datos mas relevantes: Cantidad total de tickets, cantidad promedio de tickets por agente, un promedio de satisfacción del cliente y por último pero no menos importante, un promedio de días abiertos.  
Se pueden visualizar una serie de gráficos que permitirán medir de forma rápida y asertiva, la cantidad de tickets de acuerdo al mes seleccionado en el filtro, la distribución de los tickets según su categoría y los días abiertos.  
También se aprecia en un gráfico tipo dona, el promedio de satisfacción del usuario.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/ITAgentsDashboard/MainPage.jpg)

### Tablero: Ranking

En esta página se puede visualizar un ranking de los empleados, bien sea de los primeros lugares o los últimos, esto dependerá del botón a la izquierda de la página, que cambiará automáticamente de nombre de acuerdo a la selección actual:  
- Si se ha seleccionado "Ver primeros lugares", entonces el título de la hoja cambiará a "DESEMPEÑO DE LOS AGENTES - RANKING PRIMEROS LUGARES" y el botón cambiará a "Ver últimos lugares"  

- Si se ha seleccionado "Ver últimos lugares", entonces el título de la hoja cambiará a "DESEMPEÑO DE LOS AGENTES - RANKING ULTIMOS LUGARES" y el botón cambiará a "Ver primeros lugares"  

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/ITAgentsDashboard/RankingDetail.jpg)

### Tablero: Zoom

Es posible realizar un zoom sobre los empleados para obtener más información, para esto es necesario posicionar el botón del puntero del mouse sobre alguno de los empleados distribuidos en la lista izquierda.  

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/ITAgentsDashboard/EmployeeDet.jpg)

### ¡De día o de noche!

En los últimos años, se ha puesto de moda el modo oscuro, que además de reducir la fatiga visual del usuario, permite ahorrar el consumo de batería en los dispositivos y ayudar a la lectura.  
Este Dashboard permite cambiar el modo de visualización para un modo Light con colores claros o un modo oscuro.  

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/ITAgentsDashboard/DarkPage.jpg)


### Resumen.

Este tablero es producto del curso de **Microsoft Power BI - Curso de Power BI Desktop** de Udemy, en donde se utilizaron las 5 fases fundamentales de Power BI Desktop:  

- Get Data
- Data Preparation
- Data Modeling & DAX
- Data Visualization
- Data Reporting

Es un excelente curso, el cual recomiendo ampliamente.
https://www.udemy.com/course/curso-microsoft-power-bi/#overview
