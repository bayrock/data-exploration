Data Exploration for Devinity.org (Garry’s Mod Flood)
================

### Prop spawns

Let’s start off by importing and cleaning our prop statistics. I’ll
filter props with 0 spawns, as they should be irrelevant to the rest of
our analysis:

``` r
props <- read.csv(file = 'data/propstatistics.csv') %>%
  filter(spawns != 0) %>%
  #filter(!str_detect(model, "")) %>%
  mutate(model = str_replace(model, ".+/", "")) %>%
  mutate(model = str_replace(model, ".mdl", "")) %>%
  arrange(desc(spawns))

top10props <- props %>%
  slice(1:10)
```

Now let’s check out the data set:

``` r
top10props
```

    ##                         model spawns
    ## 1               bluebarrel001 973539
    ## 2       furniture_bookshelf01 848195
    ## 3  vendingmachinesoda01a_door 734702
    ## 4       vendingmachinesoda01a 608348
    ## 5                    boat002b 488792
    ## 6              wood_crate001a 458136
    ## 7          dock03_pole01a_256 419126
    ## 8        concrete_barrier001a 336012
    ## 9          furniture_shelf01a 316081
    ## 10             wood_crate002a 310211

That’s a useful overview, but let’s also see some visualizations:

``` r
ggp <- ggplot(top10props, aes(reorder(model, spawns), spawns)) +
  geom_col() +
  coord_flip() +
  labs(title="Top 10 most spawned props", caption="Devinity.org") +
  theme(axis.text.x = element_text(angle=90, vjust=0.6))
  
ggp
```

![](devinity-analysis_files/figure-gfm/Top%2010%20most%20spawned%20props-1.png)<!-- -->

``` r
bottom10props <- props %>%
  slice(n():(n()-10))

ggp2 <- ggp %+% bottom10props +
  aes(reorder(model, -spawns), spawns) +
  labs(title="Top 10 least spawned props")

ggp2
```

![](devinity-analysis_files/figure-gfm/Top%2010%20least%20spawned%20props-1.png)<!-- -->
