
<!-- README.md is generated from README.Rmd. Please edit that file -->

# StructMetQSM

<!-- badges: start -->
<!-- badges: end -->

The goal of StructMetQSM is to provide easy to use functions to quickly
obtain structural metrics from treeQSMs obtained with version 2.4.0 of
TreeQSM (<https://github.com/InverseTampere/TreeQSM>).

## Installation

You can install the development version of StructMetQSM from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("lmterryn/StructMetQSM")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
library(StructMetQSM)
QSM_path <- "C:/Users/lmterryn/example_qsm.mat"
read_tree_qsm <- function(QSM_path)
pos <- tree_position_qsm(cylinder)
#sba <- stem_branch_angle_qsm(branch)
#sbcs <- stem_branch_cluster_size_qsm(cylinder)
#sbr_th <- stem_branch_radius_qsm(cylinder, treedata, "treeheight")
#sbl_dbh <- stem_branch_length_qsm(branch, treedata, "dbh")
#sbd_dbh <- stem_branch_distance_qsm(cylinder, treedata, "dbh")
```
