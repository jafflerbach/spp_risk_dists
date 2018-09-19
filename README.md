# Mapping status and conservation of global at-risk marine biodiversity

__Authors:__ Casey C. O'Hara<sup>1,2</sup>, Juan Carlos Villaseñor-Derbez<sup>1</sup>, Gina M. Ralph<sup>3</sup>, Benjamin S. Halpern<sup>1,2,4</sup>

1. Bren School of Environmental Science and Management, University of California, Santa Barbara CA 93106
2. National Center for Ecological Analysis and Synthesis, University of California, 735 State Street Suite 300, Santa Barbara CA 93101
3. IUCN Marine Biodiversity Unit, Department of Biological Sciences, Old Dominion University, Norfolk, VA 23529, USA
4. Department of Life Sciences, Imperial College London, Silwood Park Campus, Buckhurst Rd, Ascot, West Berkshire SL5 7PY, United Kingdom

\* Correspondence to: Casey O'Hara; cohara@bren.ucsb.edu

## Abstract: 

To conserve marine biodiversity we must first understand the spatial distribution and status of at-risk biodiversity. We combined range maps and conservation status for 5,291 marine species to map the global distribution of extinction risk of marine biodiversity.
We find that for 83% of the ocean, >25% of assessed species are considered threatened, and 15% of the ocean shows >50% of assesed species threatened when weighting for range-limited species.
Comparing marine biodiversity risk to locations of no-take marine reserves, we identify regions where reserves preferentially afford proactive protection (i.e., preserving low-risk areas) or reactive protection (i.e., mitigating high-risk areas).
In particular, elevated risk to high seas biodiversity indicates a need for credible protection and reduction of fishing effort in international waters.

## This repository

Welcome to the repository for code and data related to __Mapping status and conservation of global at-risk marine biodiversity__.  Here is an overview of the organization of files and data:

### Code

#### Run this first! Setup directory: `spp_risk_dists/_setup`

* In this directory are a sequence of files used to generate the bits and pieces that are later assembled into the rasters of biodiversity risk.
* .Rmd files are sequenced with a prefix number (and letter) to indicate the order of operations.  Briefly:
    1. Pull information from the IUCN Red List API to determine an overall species list, habitat information, and current risk (conservation status).
    2. Pull information from API on risk from regional assessments; also recode the regions according to Marine Ecoregions (Spalding et al, 2007) for later spatialization.
    3. Pull historical assessment information from API for possible trend analysis.  Note that this did not make it into the final draft of the manuscript.
    4. Set up spatial layers in Gall-Peters, 100 km<sup>2</sup> cells.  Layers include:
        * cell ID (cells are sequentially numbered for combining with tabular data)
        * ocean area
        * marine protected area (classification, year of protection, proportion of protection)
        * Exclusive Economic Zones (EEZ) and FAO fishing regions
        * Marine Ecoregions of the World
        * bathymetry
        * NOTE: these layers are all saved in the `spp_risk_dists/_spatial` directory.
    5. Convert species range maps to rasters.
        * For maps provided directly by IUCN, aggregate into multispecies files based on family.  There is some cleaning done at this stage to fix problematic extents and attributes.
        * From the list of all available maps, generate a master list of all mapped, assessed species for inclusion in the study.
        * Rasterize each species to a .csv that includes cell ID and presence.  A .csv format was used for file size and ease of reading and binding into dataframes.
    6. Aggregate individual species ranges into larger taxonomic groups, and summarize key variables (mean risk, variance of risk, number of species, etc) by group.  
        * Technically this is not necessary but makes it easier to quality check the process along the way, and supports mapping at the level of taxonomic group rather than the entire species list level.
        * This process is done twice: once for uniform weighting and once for range-rarity weighting.  Resulting files are saved separately.

#### Then run this!  Root directory: `spp_risk_dists`

* At this level there are several scripts, prefixed `1x_biodiversity_maps`, that collate the various taxonomic group level files (generated in `setup` part 6) and summarize to the global level.  
    * Note each creates a specific aggregation - comprehensively assessed species vs all available species; uniform vs range-rarity weighting.
    * The rasters generated in these scripts are saved in the `_output` folder.
* At this level are also all scripts used to generate figures for the manuscript, based on the data from the setup scripts and the rasters.
    * The figures are saved in `spp_risk_dists/ms_figures` directory.
  
### Data and output files

* The `spp_risk_dists/_data` folder contains tabular data about IUCN species used throughout the processing of this analysis.  These files are generated by scripts in the setup directory.
* The `spp_risk_dists/_spatial` folder contains general spatial data generated and/or used in the `setup` scripts.  These include:
    * rasters for cell ID, EEZ ID, marine ecoregion ID, ocean area, and bathymetry masks.   
    * tabular data of region names and lookups for IUCN regional assessment to marine ecoregion.
    * tabular data of marine protected area level/year/coverage to cell ID.
    * shapefiles used for map plotting from Natural Earth.
* The `spp_risk_dists/_output` folder contains the rasters of biodiversity risk, species richness, variance of risk, etc generated from the scripts in the base directory.


