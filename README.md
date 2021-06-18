# Joe Wu's Lab Computational Biology Subgroup  

### Meeting on 18 June 2021  
#### R package for the Month: dplyr  

Packages Required:  
```
require(tidyverse)
require(dplyr)
require(ggplot2)
```  

  
##### Goal: Transform single-cell RNA-seq metadata into Pie and Donut Plot  
```
### read in the metadata downloaded from NCBI
data = read.table("data/Meeting_20210618/tissueadded_GSE165837_CARE_ATAC_merged_umap_cluster_labels.tsv",header=TRUE,sep="\t")
summary(data)
head(data)


### Summarize the data according to your questions
### Filter: Subset row that satisfy equation
### group_by: arrange rows based on specific groups
### count: count the number of occurence
### arrange: sort / order the frame based on specific values
### mutate: add new columns to the existing data frame

### Question 1: To count the cell types in LV only
celltype.for.LV = data %>% filter(tissue == "LV") %>% group_by(Cluster) %>% count() 

### Question 2: To count the cell types in RV only
celltype.for.RV = data %>%  filter(tissue == "RV") %>%  group_by(Cluster) %>% count() 

### Question 3: To count the cell types in both LV and RV only
celltype.for.RV.and.LV = data %>%  group_by(tissue, Cluster) %>% count() 


### We want to plot the Donut for the LV composition  
celltype.for.LV = data %>%  filter(tissue == "LV") %>%  group_by(Cluster) %>%  count() %>%  ungroup()%>%  arrange(desc(n)) %>% mutate(percentage = round(n/sum(n),4)*100)
		 

### Now we are ready to plot  Pie
p1 <- ggplot(data = celltype.for.LV, aes(x = "", y = percentage, fill = Cluster))
p1 <- p1 + geom_bar(stat = "identity")+ coord_polar("y", start = 200) 
p1 <- p1 + theme_void() + scale_fill_brewer(palette = "Paired")

### Now we are ready to plot Donut plot
p1 <- ggplot(data = celltype.for.LV, aes(x = 2, y = percentage, fill = Cluster))
p1 <- p1 + geom_bar(stat = "identity")
p1 <- p1 + coord_polar("y", start = 200)
p1 <- p1 + theme_void()
p1 <- p1 + scale_fill_brewer(palette = "Paired") + xlim(.2,2.5)
```  




 
