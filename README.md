# Análisis sobre los egresos psiquiatricos en México en 2022
Se creó una base de datos basada en el "2º Diagnóstico Operativo de Salud Mental y Adicciones" publicado en el 2022. Se centralizó en la los egresos por region, tipo de hospital y el sexo de los pacientes egresados.
_Visita el informe en:_ https://www.gob.mx/cms/uploads/attachment/file/730678/SAP-DxSMA-Informe-2022-rev07jun2022.pdf
### Base de datos
[datos.csv](https://github.com/user-attachments/files/20165833/datos.csv)
``` r
#Cargar la libreria
library(readr)
datos <- read_csv("Ruta de acceso") #Pon tu propia ruta de acceso
View(datos)
```
## Anova
Se realizó una ANOVA de tres factores (region, tipo de hospital y sexo). En el modelo se eliminaron todas las interaccion dobles y triples debido a que estan no aportaban algo estadisticamente significativo.
```r
#ANOVA
modelo_anova <- aov(egre ~ region+ sex+ hos, data = datos)
#Mostrar el ANOVA
summary(modelo_anova)
```
### Resultado
![Captura de pantalla 2025-05-12 111432](https://github.com/user-attachments/assets/524ce181-2cb0-4032-85ed-867fe7db9b4f)

Se observa que el factor hospital es el unico que presenta diferencia significativa con respecto a los demas. Por lo cual solo se tomara en cuenta el mismo para el análisis post- hoc.
## Prueba de supuestos
#### Prueba de normalidad
![image](https://github.com/user-attachments/assets/ef7338fa-5a45-4ff0-8ca4-7c274383e8e4)
```r
shapiro.test(residuals(modelo_anova))
```
Como el p-valor > 0.05 en shapiro.test, los residuos se consideran **normalmente distribuidos**.

#### Prueba de homocedasticidad.



