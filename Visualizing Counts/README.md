## Functions!

Function to detect the sample groups:

```
detectGroups <- function (x){  # x are col names
  tem <- gsub("[0-9]*$","",x) # Remove all numbers from end
  #tem = gsub("_Rep|_rep|_REP","",tem)
  tem <- gsub("_$","",tem); # remove "_" from end
  tem <- gsub("_Rep$","",tem); # remove "_Rep" from end
  tem <- gsub("_rep$","",tem); # remove "_rep" from end
  tem <- gsub("_REP$","",tem)  # remove "_REP" from end
  return( tem )
}
```

Function for calculating distance between samples for the heatmap:

```
dist2 <- function(x, ...)   # distance function = 1-PCC (Pearson's correlation coefficient)
  as.dist(1-cor(t(x), method="pearson"))
```

Code block to plot the heatmap from what was previously run in the project:

```
library(gplots)

hclust2 <- function(x, method="average", ...)  # average linkage in hierarchical clustering
  hclust(x, method=method, ...)

n=100 # number of top genes by standard deviation

x = assay(rld)
if(n>dim(x)[1]) n = dim(x)[1] # max	as data

x = x[order(apply(x,1,sd),decreasing=TRUE),]  # sort genes by standard deviation

x = x[1:n,]   # only keep the n genes

# this will cutoff very large values, which could skew the color 
x=as.matrix(x[1:n,])-apply(x[1:n,],1,mean)
cutoff = median(unlist(x)) + 4*sd (unlist(x)) 
x[x>cutoff] <- cutoff
cutoff = median(unlist(x)) - 4*sd (unlist(x)) 
x[x< cutoff] <- cutoff
	
groups = detectGroups(colnames(x) )
groups.colors = rainbow(length(unique(groups) ) )


	lmat = rbind(c(5,4),c(0,1),c(3,2))
	lwid = c(1.5,4)
	lhei = c(1,.2,4)


heatmap.2(x, distfun = dist2,hclustfun=hclust2,
	 col=greenred(75), density.info="none", trace="none", scale="none", keysize=.5
	,key=T, symkey=F
	,ColSideColors=groups.colors[ as.factor(groups)]
	,margins=c(8,12)
	,cexRow=1
	,srtCol=45
	,cexCol=1.  # size of font for sample names
	,lmat = lmat, lwid = lwid, lhei = lhei
	)
```

As always the project link is found in my github repo 'Bioinformatics_RNAseq' here:

https://github.com/ACSoupir/Bioinformatics_RNASeq/blob/master/Mouse_RNA_Seq_p53_genotoxic.md
