# obitools2-pipeline

This repository contains the code to process metabarcoding data with OBITools v.1.2.12 on Brown's high-performance cluster, Oscar.
The following steps provide guidance on connecting to an RStudio server on Oscar and running the interactive R Notebooks.

## Connecting to Oscar

There are three ways to interact with Oscar: 

1. Through an [RStudio Server](https://docs.ccv.brown.edu/oscar/connecting-to-oscar/open-ondemand/using-rstudio) hosted on Open OnDemand. All interactions are through the various RStudio panes.

2. Through a virtual linux desktop called [Open OnDemand](https://docs.ccv.brown.edu/oscar/connecting-to-oscar/open-ondemand) (full desktop with access to files, a command-line shell, and RStudio)

3. Through a [SSH tunnel](https://docs.ccv.brown.edu/oscar/getting-started) in a terminal (command-line only)

Option #1 is recommended for this use case, and allows us to choose a newer version of R.

- [ ] If not on campus, make sure you are connected to the Brown [VPN](https://it.brown.edu/services/virtual-private-network-vpn)
- [ ] Navigate to the link in #1 and choose R version 4.2.0.
- [ ] Under Modules put `git/2.29.2 anaconda/2022.05`.
- [ ] Launch the session once it has been allocated. 
- [ ] Go to the terminal pane and `cd /oscar/data/tkartzin/<your folder>` (replace <your folder> with your user folder here)
- [ ] Now `git clone https://github.com/trklab-crisprsites/obitools2-pipeline.git`
- [ ] Also in the terminal: `cd obitools2-pipeline`
- [ ] In the Files panes, use the menu at the top right to make sure you are also at the same path.
- [ ] Double-click the `.obitools2-pipeline.Rproj` file to set the project working directory. All of the notebooks are built from this working directory.
![Rproj example](images/Rproj-example.png)

**Note:** Global reference databases will be stored in the shared lab directory: `/oscar/data/tkartzin/global_ref_lib_plants`

**Note:** If Conda is not found when running code chunks, add this line to your `.bash_profile` in your home directory on Oscar: `export PATH=~/gpfs/runtime/opt/anaconda/2022.05/bin:$PATH`

## Workflow structure

The main folders in this repository are:
- `images`: this is just for the images used in the README
- `template`: this contains `data`, `src`, and `test_data` folders
  - `data`: this can be used to copy over your dataset
  - `src`: the notebooks for running the code, numbered 1, 2a, 2b, 2c, 3
  - `test_data`: a simple dataset to use for learning the workflow

## Getting data into your workflow

The easiest way to copy over your data is through the SMB client in your local Mac Finder app. Connect as described [here](https://docs.ccv.brown.edu/oscar/connecting-to-oscar/cifs) and use the path displayed in this example:
![smb_example](images/smb_example.png)

- `cd <your_folder>/obitools2-pipeline/template/data`; now drag and drop your files from local. They will get copied over into a dated folder.

Take a look at the `sample_sheet_test.xlsx` while you have SMB mounted; copy the headers and make a `sample_sheet.xlsx` with your own metadata. Leave the sample sheet in the root directory of the repo.

## Running the Notebooks

**Note:** The first notebook is in the parent directory.
The notebooks can be opened by double-clicking from the RStudio `Files` window.
The first step is to update all of the `params` in the YAML header of the first notebook. 

### 1. `Step1_env_setup.Rmd`
This first notebook generates a new folder with today's date for you analysis, and copies over data, source notebooks, and the empty results folder.

Run all from the drop-down menu to generate parameters and create environment variables.

Next navigate to the `src` folder inside the new folder with today's date to view the analysis notebooks.

### 2. `Step2a_data_prep.Rmd`
The second notebook is where you set all of your parameters for trimming, filtering, primers, etc. This notebook also runs `cutadapt` to trim off primers.

### 3. `Step2b_data_processing.Rmd`
The third notebook filters files and merges all the reads into one file per sample. There are interactive steps at the end to investigate controls and move any suspicious samples out of the analysis.

### 4. `Step2c_data_cleaning.Rmd`
The fourth notebook re-runs the merging steps with the bad samples removed and cleans the data set to prepare if for taxonomy assignment.

### 5. `Step3_taxonomy_assignment.Rmd`
The final notebook is for assigning taxonomy to your cleaned reads!

## Check for the latest code

There are a few git commands that can help make sure you always have the latest code versions when running your analyses.
* `git status` - check which branch you are on and view staging area; you should see the `main` branch 
* `git pull` - this is always good to run after you verify you are on main. It will pull down any changes since the last time you ran an analysis.

## Tips for Development

Useful git commands:

* `git add <file>` - add a file to the staging area
* `git commit -m "<descriptive message>"` - commit the staged changes with a message (required)
* `git switch <branch>` - change to a different branch
* `git checkout -b <branch>` - make a new branch; just be aware of which branch you are currently on
* `git pull` - pull the latest changes from the remote repo; a good habit every time you switch to main
* `git stash` - stash the changes so your branch is clean before you switch to another branch
* `git stash pop` - pop the changes back out after you have switched to the desired branch

## Troubleshooting

* If your R session hangs, the environment variables will be lost, so it is best to start back at the top with Step 1.
* When creating the conda environments in Steps 2a and 2b, they only need to be created once. They will take some time to resolve dependencies when first created, but then can simply be activated each time they are needed thereafter.