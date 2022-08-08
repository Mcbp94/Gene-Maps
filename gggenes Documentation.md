# gggenes Documentation

Mark Chan

*22 January 2021*

------

gggenes is a ggplot2 extension for drawing gene arrow maps. It uses R studio (R installation required), and a prior installation of ggplot2.

### 1. Installation

1. `R v4.0.3` can be downloaded from [here](https://cran.r-project.org/bin/windows/base/).

2. `R Studio v1.4.1103` can be downloaded from [here](https://rstudio.com/products/rstudio/download/).

3. Install `ggplot2`:

   ```R
   # The easiest way to get ggplot2 is to install the whole tidyverse
   > install.packages("tidyverse")
   
   # Alternatively, install just ggplot2
   > install.packages("ggplot2")
   ```

4. Install `gggenes`:

   ```R
   > install.packages ("gggenes")
   ```

5. Install `dplyr`:

   ```R
   > install.packages("dplyr")
   ```

### 2. Loading libraries

```R
> library(ggplot2)
> library(gggenes)
> library(dplyr)
> library(readxl)
```

### 3. Preparing input genes file

The `example_genes` data file from `gggenes` contains 6 variables:

| variable name | data type                         |
| ------------- | --------------------------------- |
| molecule      | the genome                        |
| gene          | the name of the gene              |
| start         | the starting position of the gene |
| end           | the end position of the gene      |
| strand        | the strand containing the gene    |
| orientation   | the orientation of the gene       |

```R
# Loading example_genes into viewer
> data(example_genes)

# Viewing example_genes from viewer
> View(example_genes)
```

It should look something like this:

![image-20210122143929006](C:\Users\markc\AppData\Roaming\Typora\typora-user-images\image-20210122143929006.png)

1. We can create a similar file in Excel with the above 6 headers and fill in each row with the details of the genes.

2. Save the file as an Excel workbook (.xls, .xlsx).

3. In the R console, type the following:

   ```R
   # Import workbook titled 'mercury_operon'
   > mercury_operon <- read_excel("C:/Users/markc/Desktop/mercury_operon.xlsx")
   
   # Viewing 'mercury_operon' from viewer
   > View(mercury_operon)
   ```

4. We should be able to view our data from the viewer:

   ![image-20210122152557220](C:\Users\markc\AppData\Roaming\Typora\typora-user-images\image-20210122152557220.png)

### 4. Drawing gene arrows

Now we can plot! Make sure to load the necessary `ggplot2` and `gggenes` libraries if you have not.

```R
# Drawdrawdraw
> ggplot(mercury_operon, aes(xmin = start, xmax = end, y = molecule, fill = gene)) + geom_gene_arrow() + facet_wrap(~ molecule, scales = "free", ncol = 1) + scale_fill_brewer(palette = "Set3")
```

Edited:

1. Changed first column name `molecule` to `p1`

2. Changed first column contents to `130566-137987`, `137988-148972`, `148972-155667` for whole operon being too long to fit into image

3. Grouped up metal resistance gene classes to simplify colors and ease visualization

   - *merA, merB, merC, merD ...* > *mer*
   - *aioA, aioB, arsR* > *ars*
   - hypothetical proteins > hp

4. ```R
   # Updated drawdrawdraw
   > ggplot(mercury_operon_2, aes(xmin = start, xmax = end, y = p1, fill = gene, forward = orientation)) + geom_gene_arrow() + facet_wrap(~ p1, scales = "free", ncol = 1) + scale_fill_brewer(palette = "Set3") + theme_genes() + geom_text(data = mercury_operon_2 %>% mutate(start = (start + end)/2), aes(x=start, label = gene), nudge_y = 0.2)
   ```

5. ![image-20210122163520411](C:\Users\markc\AppData\Roaming\Typora\typora-user-images\image-20210122163520411.png)

6. How to produce larger palettes: https://www.r-bloggers.com/2013/09/how-to-expand-color-palette-with-ggplot-and-rcolorbrewer/

   * RColorBrewer produces larger palettes by interpolating existing ones with the function *colorRampPalette*.

   * Example:

     ![img](https://i0.wp.com/4.bp.blogspot.com/-j0WmmybKepE/UjKb_fC6TxI/AAAAAAAAKeM/30stF6y2ptI/s1600/ggplot-mtcars-mypalette-nolegend.png?zoom=1.25&w=578&ssl=1)

**Note:** gggenes can plot conserved domains - to explore

Useful resource: https://github.com/wilkox/gggenes/issues/28

https://cran.r-project.org/web/packages/gggenes/readme/README.html









