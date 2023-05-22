# Nitrogen-Study
R markdown storyboard summarizing the analysis of data collected from agridat showing the impact of increased nitrogen fertilizer application on the yield of barley
---
title: "Impact of Nitrogen Application on Yield of Barley Accross Locations"
output: 
  flexdashboard::flex_dashboard:
    orientation: columns
    social: menu
    source_code: embed
    storyboard: True
---
### Summary Statistics for the Nitrogen Application Trials
#### Boxplots of the Nitrogen Application Rate Trials 

```{r}
library(agridat)
library(tidyverse)
library(hrbrthemes)
library(viridis)
library(plotly)
nitrogen_treatment<-as.factor(hernandez.nitrogen$nitro)
boxplot_summary <- ggplot(hernandez.nitrogen, aes(x = nitrogen_treatment, y = yield, 
                               text = paste("Yield: ", yield, "N Rate: ", nitro, "Location: ", loc))) + 
  geom_boxplot(color = "green", fill = "darkgreen", alpha = .5) +
  geom_jitter(colour = "blue", position = "jitter", alpha = .50)
ggplotly(boxplot_summary, tooltip = "text")
```

---

Based on the presented boxplot graph it is clear that the data obtained from the studies there the information is a strong candidate for analysis with only 2 potential outliers observed in the data set.  These two data points were evaluated to determine if there was a reason for removal which was not determined to be needed based on the smaller number of replications available at this rate.

#### Summary of ANOVA for the nitrogen application trials

```{r}
library(agridat)
library(pander)
nitrogen_treatment<-as.factor(hernandez.nitrogen$nitro)
site_anova <- aov(yield  ~ nitrogen_treatment + site*nitrogen_treatment, data=hernandez.nitrogen)
pander(anova(site_anova), add.significance.stars = TRUE)
```

---

The analysis of the nitrogen applications to the various plots shows a significant effect on the yield as measured in the trials.  The study shows that the replications at each site did not have a significant difference as observed the replicate ANOVA however there was a significant difference in the yield performance between locations which is accounted for in the analysis.  

### Summary of nitrogen application rates across multiple locations

```{r}
library(plotly)
library(ggplot2)
ggplot(hernandez.nitrogen, aes(nitro, yield)) + geom_point() + geom_smooth(se = FALSE)

```

---

As observed in this graph, the increase of nitrogen application has a positive impact until the rate increases above 120lb/ac where the increases appear to plateau.  This indicates diminishing return on application rates of nitrogen levels beyond this rate.

### Nitrogen trial Location data showing yield impact

```{r out.width="200%"}
fig_location <- ggplot(hernandez.nitrogen, aes(x=nitro, y=yield)) +
  theme(legend.position = "bottom") +
  geom_point(aes(color=site)) +
  facet_grid(. ~ loc) +
  geom_smooth(color="blue")
fig_location
```

---

As observed in the ANOVA presented earlier, there is was a significant difference in the effect of the nitrogen application rates between sites.  This is illustrated in the graphs showing the data collected at each location individually.

### Spacial representation of impact on yield due to increased nitrogen analysis

```{r}
library(viridis)
library(hrbrthemes)
ggplot(hernandez.nitrogen, aes(loc, nitrogen_treatment, fill= yield)) + 
  geom_tile() +
  scale_fill_viridis(discrete=FALSE) +
  theme_ipsum()
```

---

A look at the heatmap created showing the treatments accross each location.,  the impact of the nitrogen application between locations shows trends towards increased yield with increased application rates to a limit of 120lbs./ac which shows yield reduction or st
