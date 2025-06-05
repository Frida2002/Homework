# Análisis Sobre Los Egresos Psiquiatricos En México 2022.
Se creó una base de datos basada en el "2º Diagnóstico Operativo de Salud Mental y Adicciones" publicado en el 2022. Se centralizó en los egresos por region, tipo de hospital y el sexo de los pacientes egresados.
_Visita el informe en:_ https://www.gob.mx/cms/uploads/attachment/file/730678/SAP-DxSMA-Informe-2022-rev07jun2022.pdf
### Base de datos
[datos.csv](https://github.com/user-attachments/files/20165833/datos.csv)
``` r
#Cargar la libreria
library(readr)
datos <- read_csv("Ruta de acceso") #Pon tu propia ruta de acceso
View(datos)
```
###### *nota: En la columna CAAPS incluye el antes mencionado, hospitales comunitarios y de especialidad*
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

Se observa que el factor hospital es el unico que presenta diferencia significativa con respecto a los demas. Por lo cual solo se tomara en cuenta el mismo para el análisis.
### Anova de un solo factor
Debido a que ninguno de los otros factores tiene intercción entre ellas significativamente, se realizó otra ANOVA, solo tomando el factor de interés (Tipo de hospital).
```r
#ANOVA ONEFACTOR
onef<-aov(egre~ hos, data=datos)
summary(onef)
```
![image](https://github.com/user-attachments/assets/5387a3c9-e314-451b-938f-35a93aa27b03)

## Prueba de supuestos
#### Prueba de normalidad
```r
shapiro.test(residuals(modelo_anova))
```
![image](https://github.com/user-attachments/assets/ef7338fa-5a45-4ff0-8ca4-7c274383e8e4)

Como el p-valor > 0.05 en shapiro.test, los residuos se consideran **normalmente distribuidos**.

#### Prueba de homocedasticidad.
```r
leveneTest(egre ~hos, data = datos)
```
![image](https://github.com/user-attachments/assets/47cf996e-ed12-4801-b205-b793719b0c17)

Como el p-valor > 0.05, se asume que **las varianzas son homogéneas entre grupos**.
#### Prueba de independencia.
```r
plot(onef$residuals)
```
![image](https://github.com/user-attachments/assets/1e27c8c3-a560-4fbc-af5d-7843a0e6094f)

Como no existe un patron identificable en los datos, se determina que **hay independencia de datos**. 

## Prueba de t-student
```r
t.test(egre ~ hos, data = datos)
```
![image](https://github.com/user-attachments/assets/2c77d713-0b24-406a-8a3b-4d2cb6f82812)

Concluimos que existe evidencia estadística suficiente para afirmar que **los tipos de hospital tienen medias de egresos significativamente distintas**.
## Blox-plot 
Para visualizar los datos se obtuvo un bloxplot de los egresos psiquiatricos de cada tipo de hospital
```r
# Cargar librería
library(ggplot2)

# Crear boxplot
ggplot(datos, aes(x = hos, y = egre, fill = hos)) +
  geom_boxplot() +
  labs(title = "Comparación de egresos por tipo de hospital",
       x = "Tipo de hospital",
       y = "Egresos") +
  theme_minimal() +
  scale_fill_manual(values = c("#CDB5CD", "#FFBBFF"))  # Puedes ajustar los colores
```
![image](https://github.com/user-attachments/assets/70dda299-b79e-4617-8e42-f885e3bfab6a)

###### *Nota: Si desear agregar otros colores puedes consultarlo en: https://r-charts.com/es/colores/*

En conclusion, existen pruebas estadisticas que demuestran que hay una diferencia entre los tipos de hospitales en los que se registaron egresos psiquiatricos en 2022, se requiere de más informacion para determinar la posible causa de este suceso. Asi mismo es interesante saber que en promedio **hay más egresos psiquiatricos en hospitales psiquiatricos que en los CAAPS**, con ellos surgen preguntas como si esto se debe a el grado de especializacion, ingresos anuales, personal, etc. 







