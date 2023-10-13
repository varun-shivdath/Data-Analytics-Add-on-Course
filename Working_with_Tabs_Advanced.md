## Comparing Diamond Prices by Carat Across Different Cut Qualities {.tabset} 

```{r, results='asis'}
tabs <- sort(unique(diamonds$cut))



for(tab in tabs) {

  cat('\n')

  cat('### ', tab, '   \n')

  cat('\n')



  plot_values <- diamonds %>% filter(cut == tab)



  print(ggplot(plot_values, aes(x = carat)) +

          geom_point(aes(y = price, col = color)) +

          labs(x = '\nCarat',

               y = 'Price (USD $)\n',

               col = 'Color') +

          theme_bw())



  cat('\n')

}
```
