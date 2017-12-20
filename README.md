# genome-strip-documentation
The code that Alden Huang worked on is at [Genome Strip Code](https://bitbucket.org/furbelows/bipolar_sv/src/79ea9a73ed57?at=master)
The code can also be found at `/u/home/p/paulinal/genome-strip/alden_code`

The code that Paulina Lei modified from Alden Huang is in the directory `/u/home/p/paulinal/genome-strip`

## Set-up
* You must be in an interactive node
  * Run the command `qrsh -l h_data=(space),highp,h_rt=(time)`
    * This lets you work in an interactive node
    * `h_data` specifies space you need
      * Ex: `h_data=4G`
      * May need to increase the space
    * `highp` means high priority
    * `h_rt` specifies the running time
      * Ex: `h_rt=36:00:00` is 36 hours
      * May need to increase the time

The order to run things in is:
1. preprocess.sh
  * This correlates to SVPreprocess
2. Can choose from:
  * cnv_discovery.sh
    * This correlates to CNVDiscovery
    * Takes more time and resources than SVDiscovery
  * del_deletions_100k.sh
    * This correlates to SVDiscovery
  * del_deletions_10M.sh
    * This correlates to SVDiscovery
  * CNVDiscovery takes more time and more resources, so it is preferable to run SVDiscovery first
  * Run the 100K SVDiscovery first to test, but the 10M SVDiscovery should be more accurate
  * These scripts use the same metadata from SVPreprocess
3. genotype.sh
  * This correlates to SVGenotyper
4. base_annotator.sh
  * Can run this to find differences between 100K SVDiscovery and 10M SVDiscovery
  * Run only having finished the Discovery step

## Running SVPreprocess
* Specify a file ending in `.list` that contains all the BAM files of the genome data
  * Need at least 20 to 30 BAM files in order for metadata to generate
* Run `sh preprocess.sh batch_name.list`

To test if it is working:
* Remove the `-run \` flag in the script and run the above command
* If something like `Website: http://broadinstitute` shows up, the script is running
* Can cancel and submit the job so you can close the terminal or just have the terminal running

To submit the job:
* Create a script and inside the script, place the commands `sh preprocess.sh batch_name.list`
* Run `qsub -l h_rt=(time),h_data=(space),highp submitting_job_script.sh`, where `submitting_job_script.sh` is the script you created
* Process can take about 4 days for 20 to 30 BAM files
  * Once the job is complete, there should be logs that would be named
    * `submitting_job_script.sh.e#######` for the error logs, where # is some number
    * `submitting_job_script.sh.o#######` for the output logs, where # is some number

### Troubleshooting
* If you have submitted the job but there is no error or output logs and the metadata is not being generated, there may not have been enough space to store the metadata
  * Move output directories where the metadata should be to scratch space and the output should be there
* If output logs are not showing, try removing the flags `-jobLogDir`, `-jobRunner Drmaa`, and `-jobNative` and run the script without submitting a job in the interactive node
  * This will output the error logs directly to terminal

## Running SVDiscovery
* Make sure you have metadata from SVPreprocess
* There is a flag that specifies where the metadata should be and this is the `-md` flag
  * Make sure the directory goes to the specific metadata directory for each batch
  * If you want to run multiple batches of data, just keep adding the `-md` flag with the corresponding metadata directory

### Troubleshooting
* Remove `-jobLogDir` and `-jobRunner Drmaa` to have error logs printed directly to the terminal
