# genome-strip-documentation

## Set-up
* You must be in an interactive node
  * Run the command `qrsh -l, h_data=(space),highp,h_rt=(time)`
    * This lets you work in an interactive node
    * `h_data` specifies space you need
      * Ex: `h_data=4G`
    * `highp` means high priority
    * `h_rt` specifies the running time
      * Ex: `h_rt=36:00:00` is 36 hours

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
  * Both scripts use the same metadata from SVPreprocess
3. genotype.sh

## Running SVPreprocess
* Specify a file ending in `.list` that contains all the BAM files of the genome data
* Run `sh preprocess.sh batch_name.list`

To test is if it is working:
* Remove the `-run \` flag in the script and run the above command
* If something like `Website: http://broadinstitute` shows up, the script is running

To submit the job:
* Create a script and inside the script, place the commands `sh preprocess.sh batch_name.list`
* Run `qsub -l h_rt=(time),h_data=(space),highp, submitting_job_script.sh`, where `submitting_job_script.sh` is the script you created

## Running SVDiscovery
* Make sure you have metadata from SVPreprocess
