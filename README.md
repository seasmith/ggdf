# ggdf

ggplot2 dream functions

## Intent

Document functions I have no real intention of implementing but would love to see in `ggplot2` or some extension.

## Facets

### facet_break

```r
library(ggplot2)
library(dplyr)

# Current implementation

txhousing %>%
    filter(city == "Abilene") %>%
    mutate(date = paste(year, month, "1", sep = "-"),
           date = as.Date(date),
           rbreak = ceiling(seq(1, 187) / 34)) %>%
    ggplot(aes(date, sales)) +
    geom_line() +
    facet_wrap(vars(rbreak), scales = "free_x", ncol = 1)
    
# Idealized implementation
txhousing %>%
    ggplot(aes(date, sales)) +
    geom_line() +
    facet_break(vars(date), breaks = 6)
```

![facet-breaks](figs/facet-break.png)