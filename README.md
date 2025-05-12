# Análisis sobre los egresos psiquiatricos en México en 2022
Se creó una base de datos basada en el "2º Diagnóstico Operativo de Salud Mental y Adicciones" publicado en el 2022. Se centralizó en la los egresos por region, tipo de hospital y el sexo de los pacientes egresados.
_Visita el informe en:_ https://www.gob.mx/cms/uploads/attachment/file/730678/SAP-DxSMA-Informe-2022-rev07jun2022.pdf
### Base de datos
[datos.csv](https://github.com/user-attachments/files/20165833/datos.csv)
``` r
#Cargar la libreria
library(readr)
datos <- read_csv("Ruta de acceso")
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
![Captura de pantalla 2025-05-12 111432](https://github.com/user-attachments/assets/55fe053d-6f20-42db-8994-a7830a541de3)
