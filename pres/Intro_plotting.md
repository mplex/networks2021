---
title: Introduction -- Monastery novices network
subtitle: <em>Algebraic analysis of complex social networks</em> <br><br><span style="font-size:10pt;">Networks 2021 Conference -- Workshop 8 (part 1)</span>
#date: "24 juni 2021"
date:  "24 June 2021" 
author: 
  - name: "Antonio Rivero Ostoic"
    affiliation: <center>Sch Culture & Soc, Aarhus University</center>
    email: <center>jaro@cas.au.dk</center>
output:
  html_document:
    theme: cerulean
    highlight: tango
    code_folding: none
    keep_md: true
    keep_tex: true
  pdf_document: default
geometry: a4paper,margin=1in
---

<style type="text/css">
h1, h4 {
  color: DarkRed;
}
h1.title {
  font-size: 24pt;
  text-align: center;
}
h3.subtitle {
  font-size: 12pt;
  text-align: center;
  padding-bottom: 40px;
}
h2 {
  font-size: 22pt;
}
h3 {
  font-size: 18pt;
}
h4.author, h4.date {
  text-align: center;
}

p.output {
background-color: #FFFFFF;
padding: 10px;
border: 1px solid #C0C0C0;
margin-left: 0px;
border-radius: 5px;
font-family: monospace;
font-size: 10pt;
font-weight:bold;
}

div.see {
background-color: #99FFFF;
padding: 0px;
padding-left: 20px;
border: 1px solid #FFFFCC;
margin: 60px;
margin-right: 240px;
border-radius: 5px;
}

</style>








<div style="margin-bottom:60px;"> </div>

## Plotting in R with `"multigraph"` 

<div style="margin-bottom:20px;"> </div>


```r
#install the packages from CRAN
install.packages("multiplex", "multigraph")

# or their beta versions from GitHub
devtools::install_github("mplex/multiplex", ref="beta")
devtools::install_github("mplex/multigraph", ref="beta")
```

<div style="margin-bottom:80px;"> </div>


## Monastery novices network 

The monastery novices network is a directed, multiplex, signed, valued (and longitudinal) configuration. 

<div style="margin-bottom:30px;"> </div>



```r
# load the "multiplex" package
library("multiplex")
```

```r
# read sampson monastery dataset as Ucinet DL file
samp <- read.dl("http://vlado.fmf.uni-lj.si/pub/networks/data/ucinet/sampson.dat")
```

```r
# what types of tie the network has?
dimnames(samp)[[3]]
```

```
 [1] "SAMPLK1" "SAMPLK2" "SAMPLK3" "SAMPDLK" "SAMPES"  "SAMPDES" "SAMPIN"  "SAMPNIN" "SAMPPR"  "SAMNPR" 
```

<div style="margin-bottom:40px;"> </div>

These relations are "like T1-T3", "dislike", "esteem", "disesteem", "influence" (pos/neg), "praise" (pos/neg).


<div style="margin-bottom:40px;"> </div>




```r
# load the "multiplex" package
library("multigraph")
```


```r
# plot as valued multigraph
multigraph(samp, valued=TRUE)
```

<div class="figure" style="text-align: center">
<img src="Intro_plotting_files/figure-html/unnamed-chunk-6-1.png" alt="&lt;br&gt;&lt;em&gt;Monastery novices network with circular default layout&lt;/em&gt;" width="50%" />
<p class="caption"><br><em>Monastery novices network with circular default layout</em></p>
</div>


<div style="margin-bottom:80px;"> </div>



```r
# plot valued multigraph 
multigraph(samp, valued=TRUE, bwd=.1, pos=0, fsize=6)
```

<div class="figure" style="text-align: center">
<img src="Intro_plotting_files/figure-html/unnamed-chunk-7-1.png" alt="&lt;br&gt;&lt;em&gt;Monastery novices network with customized values&lt;/em&gt;" width="50%" />
<p class="caption"><br><em>Monastery novices network with customized values</em></p>
</div>



<div style="margin-bottom:40px;"> </div>
<hr>
<div style="margin-bottom:40px;"> </div>



### Bundle patterns


```r
# enumeration of bundle class types
bundle.census(samp)
```

```
      BUNDLES NULL ASYMM RECIP T.ENTR T.EXCH MIXED FULL
TOTAL     134   19    20     1     37      8    68    0
```


<div style="margin-bottom:60px;"> </div>



```r
# bundle patterns in the monastery novices network
summaryBundles(bundles(samp))
```
```
                                                                           Bundles
Asym1                                             ->{SAMPLK1} (WINF_12, BONAVEN_5)
Asym2                                              ->{SAMPLK1} (BASIL_3, ROMUL_10)
...                                                                            ...
Asym20                                              ->{SAMNPR} (AMAND_13, SIMP_18)
Recp                                              <->{SAMPLK3} (BONI_15, VICTOR_8)
Tent1                     ->{SAMPDLK} ->{SAMPDES} ->{SAMNPR} (ALBERT_16, ELIAS_17)
Tent2          ->{SAMPDLK} ->{SAMPDES} ->{SAMPNIN} ->{SAMNPR} (ALBERT_16, PETER_4)
...                                                                            ...
```


<div style="margin-bottom:40px;"> </div>
<hr>
<div style="margin-bottom:40px;"> </div>



### Relational system

Directed multiplex networks typically have differents relational systems inside. 
To look at these, recall network types of tie in the monastery novices network.

<div style="margin-bottom:20px;"> </div>


```r
# network's types of tie
dimnames(samp)[[3]]
```

```
 [1] "SAMPLK1" "SAMPLK2" "SAMPLK3" "SAMPDLK" "SAMPES"  "SAMPDES" "SAMPIN"  "SAMPNIN" "SAMPPR"  "SAMNPR" 
```
<!-- #OUT: c("Leo","Arsenius","Bruno","Thomas","Bartholomew","Martin","Brocard") -->

<div style="margin-bottom:20px;"> </div>

These ties are "like T1-T3", "dislike", "esteem", "disesteem", "influence" (pos/neg), "praise" (pos/neg). 


<div style="margin-bottom:60px;"> </div>

Extracting a system of strong bonds having positive ties is obtained with function `rel.sys()`. 


```r
# choose positive ties in the network
sampsb <- rel.sys(samp[,,c(3,5,7,9)], type="toarray", bonds="strong")
```

<div style="margin-bottom:40px;"> </div>

The system of strong bonds has four types of relations among 17 actors and where `"Amand"`is disregarded.


```r
# network length dimensions
dim(sampsb)
```

```
[1] 17 17  4
```

<div style="margin-bottom:40px;"> </div>
<hr>
<div style="margin-bottom:40px;"> </div>


### Graphs representation

We define a new scope to plot the graph of the monastery novices network. 


```r
# scope to define node / edge / graph characteristics
R> scps <- list(fsize=8, pos=0, vcol="#AEAEAE", vcol0="#808080",
+    ecol="#808080", bwd=.4, rot=145, mirrorY=TRUE)
```

```r
scps <- list(fsize=8, pos=0, vcol="#AEAEAE", vcol0="#808080", ecol="#808080", bwd=.4, rot=145, mirrorY=TRUE)
```

<div style="margin-bottom:60px;"> </div>

And then we proceed with the plotting with a force-directed layout. 



```r
# plot graph with a reproducible force-directed layout
multigraph(sampsb, layout="force", seed=12, scope=scps)
```

<div class="figure" style="text-align: center">
<img src="Intro_plotting_files/figure-html/unnamed-chunk-15-1.png" alt="&lt;br&gt;&lt;em&gt;System of strong bonds of monastery novices network&lt;/em&gt;"  />
<p class="caption"><br><em>System of strong bonds of monastery novices network</em></p>
</div>

<div style="margin-bottom:60px;"> </div>


### Graph with customized labels

It is possible with `"multigraph"` to customize node labels of the above graph.



```r
# vector with the monk names
R> monks <- c("Ramuald","Bonaventure","Ambrose","Berthold","Peter",
+          "Louis","Victor","Winfrid","JohnBosco","Gregory","Hugh",
+          "Boniface","Mark","Albert","Basil","Elias","Simplicius")
```




<div style="margin-bottom:40px;"> </div>

Then we use argument `lbs` in `"multigraph"`.


```r
# plot graph with a reproducible force-directed layout
multigraph(sampsb, layout="force", seed=12, scope=scps, lbs=monks)
```

<div class="figure" style="text-align: center">
<img src="Intro_plotting_files/figure-html/unnamed-chunk-18-1.png" alt="&lt;br&gt;&lt;em&gt;Strong bonds in monastery novices with customized node labels&lt;/em&gt;"  />
<p class="caption"><br><em>Strong bonds in monastery novices with customized node labels</em></p>
</div>



<div style="margin-bottom:180px;"> </div>



#### Note
Both `multiplex` and `multigraph` are available at CRAN.



<div style="margin-bottom:60px;"> </div>

<hr>

<div style="margin-bottom:60px;"> </div>


##### Content

<div style="margin-bottom:20px;"> </div>

* [<span style="color:#408080">Introduction -- Monastery novices network</span>](./Intro_plotting.html)
* [Elementary Algebraic Structures](./Algebraic_structures.html)
<!-- * [Multiplex Network Configurations](https://mplex.github.io/networks2021/) -->
<!-- * [Positional Analysis and Role Structure](https://mplex.github.io/networks2021/) -->
* [Role Structure in Multiplex Networks](./Role_structures.html)
* [Decomposition of Role Structures](./Decomposition.html)
<!-- * [Comparing Relational Structures](https://mplex.github.io/networks2021/) -->
* [Signed Networks](./Signed_networks.html)
* [Affiliation Networks](./Affiliation_networks.html)
<!-- * [Valued Networks](https://mplex.github.io/networks2021/) -->
* [Multilevel Networks](./Multilevel_networks.html)




<div style="margin-bottom:60px;"> </div>



&nbsp;

