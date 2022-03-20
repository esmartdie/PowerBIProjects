# Análisis de datos de empleados para RRHH

##### El paso a paso del análisis de los empleados para el departamento de RRHH

### ¿Por qué un tablero para RRHH?

El capital humano de una empresa es sin duda su recurso mas importante. Representa el principal factor de la producción, siendo la pieza principal en el logro de metas, que conllevan al crecimiento de la empresa.  
Es imprescindible realizar una buena gestión de talento, por lo que la analítica es una herramienta esencial para ayudar a la correcta toma de decisiones.   
Con análisis de datos, se pone a disposición del área de RRHH la posibilidad, no solo de poder medir resultados generales, sino que permite planificar actividades y proyectos que se orienten a potenciar y retener el talento humano en la empresa, ya que al fin y al cabo todas las organizaciones están compuesta por personas.

### ¿Qué me permite visualizar?

- Visualización rápida y clara sobre la cantidad de empleados, tanto totales, como agrupados por estado.
- Visualizar el detalle de los empleados, con sus datos más relevantes.
- Permite navegar por la información de forma intuitiva y se podrán exportar los informes de acuerdo con sus preferencias.
- Visualizar el detalle de los empleados por grupo de evaluación, sueldo, departamento y edad.
- Detalle sobre evaluaciones de los empleados.

### ¿Qué me permite medir?
- Medición sobre el sueldo promedio por posición
- Agrupación de empleados de acuerdo con el salario
- Gráfico de dispersión para detectar variaciones
- Distribución de sueldos por departamentos
- Promedio de evaluación
- Agrupación sobre el rendimiento de los empleados
- Proyección sobre salarios del próximo ejercicio

### Fuente de Datos

> Modelo de datos

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/DDBB.jpg)


El proyecto parte de una información suministrada en un archivo excel, el cuál luego de ser analizado genera la siguiente serie de tablas y medidas para nutrir la información de los diferentes informes.

**Tabla Sueldo:**

|Nombre de Campo  |Tipo  |Descripción |
| :------------ |:---------------:| :------------|
|Bono del ejercicio      | Campo Calculado |Bono del ejercicio teniendo en cuenta la columna calculada “Desempeño” de la tabla Evaluación. |
|Grupo sueldo      | Campo calculado        |Sueldo agrupado por rango |
|ID empleado | Campo original  |ID único por empleado |
|Sueldo | Campo original       |Sueldo del empleado |
|Sueldo próximo ejercicio | Campo calculado       |Sueldo NY del ejercicio teniendo en cuenta la columna calculada “Desempeño” de la tabla Evaluación. |


**Tabla Empleados:**

| Nombre de Campo  | Tipo  | Descripción |
| :------------ |:---------------:| :------------|
| Departamento     | Campo original | Departamento del empleado |
| Edad    | Campo original        |   Edad del empleado |
|Estado | Campo original        |   Lugar geográfico de ubicación del empleado |
|Género | Campo original       |  Género del empleado |
|Grupo Edad | Campo calculado      |   Edad agrupada por diferentes rangos |
| ID Empleado     | Campo original | Id interno del empleado |
| Nacimiento    | Campo original        |   Fecha completa de nacimiento |
|Nombre Empleado | Campo original        |    Nombre del empleado |
|Nombre jefe | Campo original       |  Nombre del jefe del empleado |
|País | Campo transformado     |   Split de la columna Estado |
|Posición | Campo original       |   Cargo del empleado |

**Tabla Evaluación:**

| Nombre de Campo  | Tipo  | Descripción |
| :------------ |:---------------:| :------------|
| Calificación     | Campo calculado | Evaluación agrupada por desempeño empleando escala de "estrellas" |
| Desempeño    | Campo calculado        |   Evaluación agrupada por rango de evaluación |
|Evaluación | Campo original        |    Evaluación del empleado |
|Grupo Evaluación | Campo calculado       |  Evaluación agrupada por rango de evaluación |
|ID Empleado | Campo original       |   ID único por empleado |


**Medidas DAX:**

| Nombre de Campo  | Cálculo |
| :------------ | :------------|
| Rendimiento Brillante    | CALCULATE( [Total empleados], 'Tabla Evaluacion'[Evaluación]> 8)  | 
|Rendimiento Bueno | CALCULATE( [Total empleados], 'Tabla Evaluacion'[Desempeño] = "Bueno")        |
|Retiro Empleados | VAR edad_retiro = Calculate( [Total empleados],'Tabla Empleados'[Edad] >= 67) RETURN IF(ISBLANK(edad_retiro),0,edad_retiro) | 
|Sueldo por categoría | countrows ()      | 
| Sueldo promedio     | VAR Sueldo_Promedio = AVERAGE('Tabla Sueldo'[Sueldo]) Return IF(Isblank(Sueldo_Promedio),0,Sueldo_Promedio)  |
| Sueldo promedio NY     | VAR Sueldo_Promedio_NY = AVERAGE('Tabla Sueldo'[Sueldo Prox Ejercicio]) RETURN IF(Isblank(Sueldo_Promedio_NY),0,Sueldo_Promedio_NY)  |
| Total empleados     | VAR Total_empleados = countrows('Tabla Empleados') Return  If(Isblank(Total_empleados),0,Total_empleados)  | 


### Tablero: Página principal

En esta página se puede apreciar un resumen de los datos de los empleados, observando datos importantes en las Tarjetas como son la cantidad de empleados, sueldo promedio, etc.  
También se puede visualizar diferente agrupación de los empleados por evaluación, sueldo, departamento, etc. Mediante un gráfico de dona, se puede explorar la segmentación de empleados por género.  
Una medida importante a destacar es cuántos empleados están prontos a retirarse, la cual toma como factor la edad de retiro habitual en los Estados Unidos.
Para más información está disponible la información en modo tabla detalle. Adicionalmente, se ha incluido un mapa para ver de forma rápida la distribución de los empleados en el país.  
Esta vista principal tiene interacción con las distintas solapas a través de los íconos superiores.  

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/Home.jpg)



### Tablero: Detalle Empleados

Accediendo a través del ícono <img src="https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/Detail.jpg" width="25px" height="25px"> se puede visualizar la página “Detalle Empleados”.  
En esta página se puede realizar 4 tipos de filtros distintos:
- **Por Estado:** Es un filtro de selección múltiple
- **Por edad del empleado:** Es un filtro que permite deslizar la barra para seleccionar un rango de edad
- **Por posición:** Es un filtro de menú desplegable en el cual se podrán seleccionar múltiples opciones
- **Por calificación:** Es un filtro de menú desplegable en el cual se podrán seleccionar múltiples opciones, en este caso son íconos.

También se muestran las tarjetas con información importante como la evaluación promedio, el sueldo promedio, etc.
Aparece una tabla detalle con mayor información, la cual se puede exportar.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/EmployeeDetail.jpg)

### Tablero: Análisis de sueldo

Accediendo a través del ícono <img src="https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/Salary.jpg" width="25px" height="25px"> se puede visualizar la página “Análisis de sueldo”. Esta página contiene un análisis sobre los sueldos de los empleados.  
Se puede realizar 2 tipos de filtros distintos:
- **Por grupo de edad:** Es un filtro de menú desplegable en el cual se podrán seleccionar múltiples opciones
- **Por grupo de sueldo:** Es un filtro de menú desplegable en el cual se podrán seleccionar múltiples opciones

Se pueden visualizar diferentes gráficos con medidas variadas para un estudio del sueldo de los empleados, también se disponen las tarjetas con los totales más relevantes.  
Se destaca el gráfico de dispersión, el cual nos ayuda a visualizar rápidamente la línea de tendencia y así poder detectar los casos que se desvían.
Para más información se dispone de la tabla detalle.  
También es importante la visión del gráfico de sueldos por posición, en el cual podremos ver los sueldos por cada uno de los puestos jerárquicos de la organización y si existen o no, diferencias por género.  
Gracias al gráfico de geolocalización, podremos ver rápidamente dónde están agrupados los empleados y cuál es la predominancia de los rangos y categorías salariales.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/SalaryAnalysis.jpg)

### Tablero: Análisis de desempeño

Accediendo a través del ícono <img src="https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/Performance.jpg" width="25px" height="25px"> se puede visualizar la página “Análisis de desempeño”.Esta página permite realizar un análisis sobre el desempeño de los empleados sin que lleve demasiado tiempo.  
Dispone de distintos gráficos con distintas mediciones, las tarjetas resumiendo los empleados según su rendimiento y para más información se dispone de la tabla detalle.  
En el gráfico de geolocalización, a medida que los círculos son más grandes es donde mejor desempeño tienen los empleados. También se evalúa por jefes, a través de un gráfico de barras horizontales de visualiza el promedio de desempeño de los empleados que tiene a cargo cada jefe.

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/PerformanceAnalysis.jpg)


### Tablero: Sueldo próximo año

Accediendo a través del ícono <img src="https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/Projection.jpg" width="25px" height="25px"> se puede visualizar la página “Sueldo próximo año”. Esta página permite visualizar una proyección en cuanto a los sueldos del próximo ejercicio.  
Se visualizan las tarjetas con totales, que permiten una rápida interpretación de la información que será detallada con dos gráficos de barras en donde se compara el sueldo actual con el próximo año, así como también el sueldo promedio.  
Partiendo de las evaluaciones de desempeño que se han tenido en el año en curso, podremos medir cuanto se estaría abonando el próximo año por sueldos anuales, teniendo en cuenta un incentivo salarial y cuál es el bono asociado a cada empleado, teniendo en cuenta su evaluación de desempeño.  
Para más información se dispone de la tabla detalle, la cual puede ser exportada.  

![](https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/ProjectionNextYear.jpg)


### ¡Aún más fácil!

Cada uno de los elementos del dashboard dispone de una ayuda visual, llamada Tooltip, que nos permitirá visualizar de forma ágil y rápida un conjunto de datos personalizados cuando se coloca el cursor sobre alguno de los objetos de un determinado panel, logrando así, expandir la información a través de una ventana emergente.  
<img src="https://github.com/esmartdie/Multimedia/blob/main/IMAGES/RRHHDashboard/ToolTip.jpg" width="550px" height="300px">
