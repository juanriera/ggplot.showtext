# Ensayo para probar la librería `showtext`en R utilizando `ggplot2`
Este repositorio es la síntesis de una [serie de posts](https://community.rstudio.com/t/legend-in-ggplot2/156765) publicados en [Posit Community](https://community.rstudio.com/). El objetivo fue recrear de la manera más fiel posible en `ggplot2` el gráfico de presentación de la librería `showtext` de R, que utiliza las funciones gráficas básicas de R. Es un ejercicio de configuración de `ggplot2`. El gráfico original puede verse en el [repositorio](https://github.com/yixuan/showtext) de su autor, [Yixuan Qiu](https://github.com/yixuan)
El resultado se presenta en el archivo R `ggplot2_showtext.R` incluido en este repositorio. Posiblemente puede haber otras soluciones más sencillas que las que he encontrado, estaré encantado de conocerlas.

This repository is the synthesis of a [series of posts](https://community.rstudio.com/t/legend-in-ggplot2/156765) published in [Posit Community](https://community.rstudio.com/). The objective was to recreate in the most faithful way possible in `ggplot2` the presentation plot of the `showtext` R library, which uses the basic graphics functions of R. It's a `ggplot2` configuration exercise. The original plot can be seen in the [repository](https://github.com/yixuan/showtext) of its author, [Yixuan Qiu](https://github.com/yixuan). The result is presented in the included R file `ggplot2_showtext.R` in this repository. Possibly there may be other simpler solutions than the ones I've found, I'll be happy to hear about them.

### Gráfico original de [Yixuan Qiu](https://github.com/yixuan)
![Yixuan](original_yixuan.png)



### Mi versión en `ggplot2`
![Mi versión](ggplot2-version.png)

El código de mi versión se incluye a continuación y en el archivo `ggplot2_showtext.R`

```r
# ----------------------------------------------------------------------------
# Mi versión con ggplot
library(tidyverse)
library(showtext)
library(scales)

## Loading Google fonts (https://fonts.google.com/)
font_add_google("Gochi Hand", "gochi")
font_add_google("Schoolbell", "bell")
font_add_google("Covered By Your Grace", "grace")
font_add_google("Rock Salt", "rock")

## Automatically use showtext to render text for future devices
showtext_auto()

## Tell showtext the resolution of the device,
## only needed for bitmap graphics. Default is 96
showtext_opts(dpi = 96)

set.seed(123)
x  <- rnorm(10)
y  <-  1 + x + rnorm(10, sd = 0.2)
y[1] <-  5
mod <- lm(y ~ x)

df <- data.frame(x,y)

df |> ggplot(aes( x = x, y = y)) +
  geom_point(size = 3, colour = "steelblue") + # colores: cadetblue4, dodgerblue3, firebrick4, 
  geom_smooth(aes(colour = "black"), method = 'lm', formula = y~x, se = F) +
  geom_abline(aes(slope = 1, intercept = 1, colour = "red")) +
  scale_colour_manual(values=c("black", "red"), labels = c("OLS", "Truth")) +
  scale_x_continuous(labels = label_number(accuracy = 0.1), breaks = breaks_extended(6)) +
  labs(
    x = "x variable",
    y = "y variable",
    title = "Draw Plots Before You Fit A Regression",
  ) +
  geom_text(
    aes(x, y), 
    data = data.frame(x = -0.5, y = 4.7), 
    label = "This is the outlier",
    colour = "steelblue",
    family = "grace",
    size = 7
  ) + 
  geom_text(
    aes(x,y),
    data = data.frame(x = 1, y = 1.2), 
    label = expression(paste("True model: ", y == x + 1)),
    family = "rock",
    angle = 30,
    size = 6, 
    colour = "red" 
  ) +
  geom_text(
    aes(x,y),
    data = data.frame(x = 0, y = 2), 
    label = expression(paste("OLS: ", hat(y) == 0.79 * x + 1.49)),
    family = "rock",
    angle = 22,
    size = 7, 
    colour = "black" 
  ) +
  theme_bw() + 
  theme(
    plot.title   = element_text(size = 22, family = "bell"),
    axis.text.x  = element_text(size = 18, family = "gochi"),
    axis.text.y  = element_text(size = 18, family = "gochi"),
    axis.title.x = element_text(size = 24, family = "gochi"),
    axis.title.y = element_text(size = 24, family = "gochi"),
    panel.grid.minor.x = element_blank(),
    legend.position = c(0.895, 0.932),
    legend.background = element_rect(colour = "black", fill="white", linewidth=0.5, linetype="solid"),
    legend.text =  element_text(size=14, family = "rock"),
    legend.title = element_blank(),
  )

```
