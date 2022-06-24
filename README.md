# bTB-reprocessing
Simple approach for processing accumulated sequence data

Split data folders into batches, each of which can then be processed on separate EC2 instances 

1. Run `getbTsamples.py` to get a full list of directories containing raw data for running through the `APHA-CSU/btb-seq` pipeline.
2. Append a column and assign each directory a job number (e.g. from 1-20 if you have 20 EC2 instances to run the analysis)  Save the ammended csv (jobsheet) in an accesible s3 location, ensuring it has unix compatible line endings!
3. Each instance should be started, log on using ssh and this repo cloned: 
`git clone https://github.com/APHA-CSU/bTB-reprocessing.git`
4. Run `bash bTB-reprocessing/install.sh`, entering sudo password requested.
5. Reboot the instance and log in again via ssh
6. Type `screen` to open a new session which can be detached once the processing is running
7. Run `bash bTB-reprocessing/run_jobs.sh $databucket $jobsheet $resultsbucket $resultsfolder $jobsToRun`

    where   `$databucket` is s3 uri of the directory containing the jobsheet    
            `$jobsheet` is the filename of the jobsheet
            `$resultsbucket` is s3 uri of the output bucket
            `$resultsfolder` is the directory name for the output files from the data processing
            `$jobsToRun` the job number to be selected for running from the jobsheet
8. `ctl-A ctl-D` to detach from the screen which will leave the script running.  Lot out from the instance.
9. That's it.  The instance will automatically shutdown when the all batches have been processed.  Logs will be saved on s3 to confirm completion.
