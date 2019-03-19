    library(tidyverse)

    ## -- Attaching packages --------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.0.0     v purrr   0.2.5
    ## v tibble  1.4.2     v dplyr   0.7.6
    ## v tidyr   0.8.1     v stringr 1.3.1
    ## v readr   1.1.1     v forcats 0.3.0

    ## Warning: package 'ggplot2' was built under R version 3.5.1

    ## Warning: package 'tidyr' was built under R version 3.5.1

    ## Warning: package 'purrr' was built under R version 3.5.1

    ## Warning: package 'dplyr' was built under R version 3.5.1

    ## -- Conflicts ------------------------------------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

Unsupervised learning ...

K Means Clustering
------------------

    data1 <- read_csv("../../data/kmeans_ex1.csv", col_types = cols())

    plot(data1, main = "Example1: Data with 2 clusters", 
         xlab = "", ylab = "", 
         type = "p", pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    data1_km <-  kmeans(data1, centers = 2, nstart = 20)

    plot(data1, col = data1_km$cluster, 
         main = "Example 1.1: K-means Output", 
         type = "p", pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-2-1.png)

    data2 <- read_csv("../../data/kmeans_ex2.csv", col_types = cols())

    plot(data2, main = "Example2: Data with 3 clusters", 
         xlab = "", ylab = "", 
         type = "p", pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-3-1.png)

    data2_km <-  kmeans(data2, centers = 3, nstart = 20)

    plot(data2, col = data2_km$cluster, 
         main = "Example 2.1: K-means Output", 
         xlab = "", ylab = "", 
         type = "p", pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    par(mfrow = c(2, 3))

    set.seed(1)

    for (i in 1:6) {
      
      model <-  kmeans(data2, centers = 3, nstart = 1, algorithm = "Forgy")
      
      mytitles <- paste0("WCSS: ", round(model$tot.withinss,4))
      
      plot(data2, col = model$cluster, 
         main = mytitles, 
         xlab = "", ylab = "", 
         type = "p", pch = 19)
      
    }

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    n <- 10
    wcss <- vector(mode = "numeric", length = n)

    for (i in 1:n) {
      
      model <- kmeans(data2, centers = i, nstart = 20)
      wcss[i] <- model$tot.withinss
      
    }


    plot(wcss, type = "b", 
         main = "Example 2.2: Scree Plot", 
         xlab = "Number of Clusters",
         ylab = "WCSS",
         pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Clustering Pokemon
------------------

    pokemon_raw <- read_csv("../../data/Pokemon.csv", col_types = cols()) # https://www.kaggle.com/abcsds/pokemon

    library(stringr)

    # Rename Raw Columns
    pokemon_raw <- pokemon_raw %>% 
        rename(Type1 = `Type 1`,
               Type2 = `Type 2`,
               SpecialAttack = `Sp. Atk`,
               SpecialDefense = `Sp. Def`)

    # Create K-means dataset

    pokemon <- pokemon_raw %>% 
      select(HP:Speed)

    pokemon %>% 
      head()

    ## # A tibble: 6 x 6
    ##      HP Attack Defense SpecialAttack SpecialDefense Speed
    ##   <int>  <int>   <int>         <int>          <int> <int>
    ## 1    45     49      49            65             65    45
    ## 2    60     62      63            80             80    60
    ## 3    80     82      83           100            100    80
    ## 4    80    100     123           122            120    80
    ## 5    39     52      43            60             50    65
    ## 6    58     64      58            80             65    80

    pokemon %>% 
      summary()

    ##        HP             Attack       Defense       SpecialAttack   
    ##  Min.   :  1.00   Min.   :  5   Min.   :  5.00   Min.   : 10.00  
    ##  1st Qu.: 50.00   1st Qu.: 55   1st Qu.: 50.00   1st Qu.: 49.75  
    ##  Median : 65.00   Median : 75   Median : 70.00   Median : 65.00  
    ##  Mean   : 69.26   Mean   : 79   Mean   : 73.84   Mean   : 72.82  
    ##  3rd Qu.: 80.00   3rd Qu.:100   3rd Qu.: 90.00   3rd Qu.: 95.00  
    ##  Max.   :255.00   Max.   :190   Max.   :230.00   Max.   :194.00  
    ##  SpecialDefense      Speed       
    ##  Min.   : 20.0   Min.   :  5.00  
    ##  1st Qu.: 50.0   1st Qu.: 45.00  
    ##  Median : 70.0   Median : 65.00  
    ##  Mean   : 71.9   Mean   : 68.28  
    ##  3rd Qu.: 90.0   3rd Qu.: 90.00  
    ##  Max.   :230.0   Max.   :180.00

    n <- 15
    pokemon_wcss <- vector(mode = "numeric", n) 

    for (i in 1:n) {
      
      model <- kmeans(pokemon, centers = i, iter.max = 50, nstart = 20)

      pokemon_wcss[i] <- model$tot.withinss
      
    }

    plot(pokemon_wcss,
         main = "Pokemon: Scree Plot",
         type = "b",
         xlab = "Number of Clusters",
         ylab = "WCSS",
         pch = 19)

![](2019-03-11-Clustering-Pokemon_files/figure-markdown_strict/unnamed-chunk-9-1.png)
