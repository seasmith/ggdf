# ggdf

ggplot2 dream functions

## Intent

Document functions I have no real intention of implementing but would love to see in `ggplot2` or some extension.

## Facets

### facet_break

```r
library(ggplot2)
library(dplyr)

# Current implementation ----------------------------------------------

txhousing %>%
    filter(city == "Abilene") %>%
    mutate(date = paste(year, month, "1", sep = "-"),
           date = as.Date(date),
           rbreak = ceiling(seq(1, 187) / 34)) %>%
    ggplot(aes(date, sales)) +
    geom_line() +
    facet_wrap(vars(rbreak), scales = "free_x", ncol = 1)

    
# Idealized implementation --------------------------------------------

txhousing %>%
    ggplot(aes(date, sales)) +
    geom_line() +
    facet_break(vars(date), breaks = 6)
```

![](figs/facet-break.png)

### facet_kmeans

```r
# Current Implementation ----------------------------------------------

# Number of clusters
n <- 2:4 

kdata <- iris[, c("Sepal.Length", "Petal.Length")]

# Run kmeans
klusters <- lapply(n, kmeans, x = kdata)

# Extract clusters
clusters <- Reduce(cbind, lapply(klusters, `[[`, "cluster"))

colnames(clusters) <- paste0("k", n)

kdata_long <- cbind(kdata, clusters) %>%
  tidyr::gather(... = k2:k4)

kdata_long %>%
  ggplot(aes(x = Sepal.Length, 
             y = Petal.Length, 
             color = factor(value))) +
  geom_point() +
  facet_wrap(vars(key))


# Idealized Implementation --------------------------------------------

iris %>%
  ggplot(aes(x = Sepal.Length,
             y = Petal.Length)) +
  geom_point() +
  facet_kmeans(
    k = 2:4,
    aesthetics = c("color") # or "fill" if pch = 21
    )

# Also facet_sf_kmeans()...wip
```

![](figs/facet-kmeans.png)