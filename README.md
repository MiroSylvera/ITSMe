
<!-- README.md is generated from README.Rmd. Please edit that file -->
<p align="center">
<img src="man/figures/logo.png" height="200" >
</p>
<!-- badges: start -->
<!-- badges: end -->

## Goal

The goal of the ITSMe R package is to provide easy to use functions to
quickly obtain structural metrics from individual tree point clouds and
their respective TreeQSMs.

## Installation

You can install the development version of ITSMe from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("lmterryn/ITSMe")
```

## Input

The functions are developed for tree point clouds obtained with TLS and
quantitative structure models (QSMs) obtained with TreeQSM
(<https://github.com/InverseTampere/TreeQSM>). The tree point cloud
based functions can also be used for tree point clouds obtained from MLS
and UAV-LS if their point densities are high enough (e.g. sufficient
stem points for dbh/dab estimation).

## Individual tree structural metrics

Structural metrics which can be calculated with the ITSMe package are
summarized in the tables below.

### Basic structural metrics

| structural metric                    |          function name          |            input |
|--------------------------------------|:-------------------------------:|-----------------:|
| diameter at breast height (m)        |         dbh_pc, dbh_qsm         | point cloud, QSM |
| diameter above buttresses (m)        |             dab_pc              |      point cloud |
| tree height (m)                      | tree_height_pc, tree height_qsm | point cloud, QSM |
| projected crown area (m<sup>2</sup>) |     projected_crown_area_pc     |      point cloud |
| crown volume (m<sup>3</sup>)         |         volume_crown_pc         |      point cloud |
| tree volume (m<sup>3</sup>)          |         tree_volume_qsm         |              QSM |
| trunk volume (m<sup>3</sup>)         |        trunk_volume_qsm         |              QSM |
| total branch volume (m<sup>3</sup>)  |     total_branch_volume_qsm     |              QSM |
| total branch length (m)              |     total_branch_length_qsm     |              QSM |
| total cylinder length (m)            |      total_cyl_length_qsm       |              QSM |

### Structural metrics from Terryn et al. (2020)

These are the metrics defined in [Terryn et
al. (2020)](https://doi.org/10.1016/j.isprsjprs.2020.08.009) and were
copied and adapted from [Akerblom et al.,
(2017)](https://doi.org/10.1016/j.rse.2016.12.002) except for the branch
angle ratio and the relative volume ratio. Definitions of the metrics
can be found in the help files of the functions and the papers of Terryn
et al. (2020) and Akerblom et al. (2017). Normalisation according to
Terryn et al. (2020) as well as Akerblom et al. (2017) is possible
through the normalisation parameter included in the functions of the
metrics that were adapted by Terryn et al. (2020). If the tree point
cloud is provided along with the TreeQSM in the functions, dbh and tree
height values are based on the point clouds rather than the QSMs. When
the buttress parameter is indicated “TRUE” the diameter above buttresses
rather than the dbh is used.

| structural metric                             |          function name           |              input |
|-----------------------------------------------|:--------------------------------:|-------------------:|
| stem branch angle (degrees)                   |      stem_branch_angle_qsm       |                QSM |
| stem branch cluster size                      |   stem_branch_cluster_size_qsm   |                QSM |
| stem branch radius (-/m)                      |      stem_branch_radius_qsm      | QSM (+point cloud) |
| stem branch length (-/m)                      |      stem_branch_length_qsm      | QSM (+point cloud) |
| stem branch distance (-/m)                    |     stem_branch_distance_qsm     | QSM (+point cloud) |
| dbh tree height ratio                         |       dbh_height_ratio_qsm       | QSM (+point cloud) |
| dbh tree volume ratio (m<sup>−2</sup>)        |       dbh_volume_ratio_qsm       | QSM (+point cloud) |
| volume below 55                               |       volume_below_55_qsm        |                QSM |
| cylinder length volume ratio (m<sup>−2</sup>) | cylinder_length_volume_ratio_qsm |                QSM |
| shedding ratio                                |        shedding_ratio_qsm        |                QSM |
| branch angle ratio                            |      branch_angle_ratio_qsm      |                QSM |
| relative volume ratio                         |    relative_volume_ratio_qsm     |                QSM |
| crown start height                            |      crown_start_height_qsm      | QSM (+point cloud) |
| crown height                                  |         crown_height_qsm         | QSM (+point cloud) |
| crown evenness                                |        crown_evenness_qsm        |                QSM |
| crown diameter crown height ratio             |  crown_diameterheight_ratio_qsm  | QSM (+point cloud) |
| dbh minimum tree radius ratio                 |     dbh_minradius_ratio_qsm      | QSM (+point cloud) |

## Examples

Calculating the diameter at breast height versus the diameter above
buttresses of a tree:

``` r
library(ITSMe)
#Specify path to the tree point cloud file
PC_path <- system.file("extdata", "example_pointcloud_1.ply", package = "ITSMe")
#Read the point cloud file
pc <- read_tree_pc(path = PC_path)
#Use dbh_pc function with default parameters and plot the fit
dbh <- dbh_pc(pc = pc,plot = TRUE)
```

<img src="man/figures/README-unnamed-chunk-2-1.png" width="100%" />

``` r
#Use dab_pc function with default parameters and plot the fit
dab <- dab_pc(pc = pc,thresholdbuttress = 0.001,maxbuttressheight = 9,
              plot = TRUE)
```

<img src="man/figures/README-unnamed-chunk-2-2.png" width="100%" />

Calculating the stem branch distance of a TreeQSM:

``` r
library(ITSMe)
#Specify path to the TreeQSM file
QSM_path <- system.file("extdata", "example_qsm_1_1.mat", package = "ITSMe")
#Read the TreeQSM file
qsm <- read_tree_qsm(path = QSM_path)
#Use stem_branch_distance_qsm function
sbd <- stem_branch_distance_qsm(cylinder = qsm$cylinder,treedata = qsm$treedata,
                                normalisation = "dbh")
#Using the point cloud information for more accurate dbh normalisation
PC_path <- system.file("extdata", "example_pointcloud_1.ply", package = "ITSMe")
pc <- read_tree_pc(path = PC_path)
sbd <- stem_branch_distance_qsm(cylinder = qsm$cylinder,treedata = qsm$treedata,
                                normalisation = "dbh",pc = pc,buttress = TRUE)
```

Calculating a summary data.frame with the basic structural metrics (tree
position, dbh, dab, tree height, projected crown area, crown volume)
that can be obtained from individual tree point clouds for all point
clouds in a specific folder:

``` r
library(ITSMe)
#Specify the path to the folder containing multiple tree point cloud files
PCs_path <- "path/to/point/cloud/folder/"
#Run summary function with default parameter settings
basic_summary <- summary_basic_pointcloud_metrics(PCs_path = PCs_path, 
                                                  extension = ".txt")
```

Calculating a summary data.frame with the structural metrics defined by
Terryn et al. (2020) for all TreeQSMs in a specific folder:

``` r
library(ITSMe)
#Specify the path to the folder containing the respective tree point cloud files
QSMs_path <- "path/to/QSM/folder/"
#If you cant DBH/DAB and height to be calculated based on tree point clouds:
#Specify the path to the folder containing the respective tree point cloud files
PCs_path <- "path/to/point/cloud/folder/"
#Run summary function with default parameter settings
Terryn_summary <- summary_Terryn_2020(QSMs_path = QSMs_path,version = "2.4.0",
                                      PCs_path = PCs_path,buttress = TRUE, 
                                      extension = ".txt")
```
