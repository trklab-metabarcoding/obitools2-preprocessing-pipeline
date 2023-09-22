# obitools2-pipeline

This repository contains the code to process metabarcoding data with OBITools v.1.2.12 on Brown's high-performance cluster, Oscar.
The following steps provide guidance on connecting to an RStudio server on Oscar and running the interactive R Notebooks.

## Connecting to Oscar

There are three ways to interact with Oscar: 

1. Through a virtual linux desktop called [Open OnDemand](https://docs.ccv.brown.edu/oscar/connecting-to-oscar/open-ondemand) (full desktop with access to files, a command-line shell, and RStudio

2. Through an [RStudio Server](https://docs.ccv.brown.edu/oscar/connecting-to-oscar/open-ondemand/using-rstudio) hosted on Open OnDemand. All interactions are through the various RStudio panes.

3. Through a [SSH tunnel](https://docs.ccv.brown.edu/oscar/getting-started) in a terminal (command-line only)

Option #2 is recommended for this use case, and allows us to choose a newer version of R.

- [ ] Navigate to the link in #2 and choose R version 4.2.0. 
- [ ] Launch the session once it has been allocated. 
- [ ] Go to the terminal pane and `cd /oscar/data/tkartzin/<your folder>` (replace <your folder> with your user folder here)
- [ ] Now `git clone https://github.com/trklab-crisprsites/obitools2-pipeline.git`
- [ ] Double-click the `.obitools2-pipeline.Rproj` file to set the project working directory. All of the notebooks are built from this working directory.
![Rproj example](images/Rproj-example.png)

**Note:** Global reference databases will be stored in the shared lab directory: `/oscar/data/tkartzin/global_ref_lib_plants`

## Workflow structure

The main folders in this repository are:
`data`: this contains a `test_data` folder
`documents`: for any docs you want to store with your analysis
`results`: all the results from running cutadapt and obitools commands
`src`: the notebooks for running the code, numbered 1, 2a, 2b, 2c, 3

## Running the Notebooks

Navigate to the `src` folder to view the available notebooks.
### 1. `Step1_env_setup.Rmd`
This first notebook generates a new folder with today's date for you analysis, and copies over test data, source notebooks, and the empty results folder.
### 2. `Step2a_data_prep.Rmd`
The second notebook is where you set all of your parameters for trimming, filtering, primers, etc. This notebook also runs `cutadapt` to trim off primers.
### 3. `Step2b_data_processing.Rmd`
The third notebook filters files and merges all the reads into one file. There are interactive steps at the end to investigate controls and move any suspicious samples out of the analysis. 
### 4. `Step2c_data_cleaning.Rmd`
The fourth notebook re-runs the merging steps with the bad samples removed and cleans the data set to perpare if for taxonomy assignment.
### 5. `Step3_taxonomy_assignment.Rmd`
The final notebook is for assigning taxonomy to your cleaned reads!

## Tips for Development

Useful git commands:

* `git status` - check which branch you are on and view staging area
* `git add <file>` - add a file to the staging area
* `git commit -m "<descriptive message>"` - commit the staged changes with a message (required)
* `git switch <branch>` - change to a different branch
* `git checkout -b <branch>` - make a new branch; just be aware of which branch you are currently on
* `git pull` - pull the latest changes from the remote repo; a good habit every time you switch to main
* `git stash` - stash the changes so your branch is clean before you switch to another branch
* `git stash pop` - pop the changes back out after you have switched to the desired branch