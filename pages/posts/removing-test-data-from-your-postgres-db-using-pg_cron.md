---
  type: posts
  title: Removing test data from your AWS hosted Postgres DB automatically every day using pg_cron
  date: 2022-09-05
---
  
I love the safety of automated tests running against my changes but hate having to clean up all that test data. One solution is to delete your environment peridically, but that is bit drastic. A solution I came across recently is using pg_cron. You can run commands on a schedule. It's often used for maintainence but I thought a great usecase is deleting test data that I no longer need. I'm using AWS and they already have pg_cron installed from version 12.5 and higher. AWS have [some instructions on how to do this ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL_pg_cron.html), I'm just highlighting the important bits I overlook or forget to do.


## Make sure your database is running 12.5 or higher

This is important! Check the version and upgrade if possible before you start changing anything. You won't get any warnings if it doesn't work. In your RDS console go to the configuration tab 

![Screenshot-2022-09-05-at-03.41.15](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.41.15.png)

Then check the `Engine version`. Verify it's `12.5` or above. Mine is `12.8` so I'm ready to configure.

![Screenshot-2022-09-05-at-03.42.17](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.42.17.png)

## Creating a custom parameter group

Most likely you are using the default group. You can't edit the values on the default group, you need to make a new one. Click on `Parameter Groups` on the side navigation. You'll most likely have something that looks like the one below

![Screenshot-2022-09-05-at-03.44.19](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.44.19.png)

Select `Create parameter group`. For the parameter group details shown below: 

- Parameter group family: `postgres12`
- Type: `DB Parameter Group`
- Group Name: `enable-pg-cron`
- Description: `Enabled pg_cron in shared_preload_libraries`

![Screenshot-2022-09-05-at-03.46.27](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.46.27.png)

## Add pg_cron to the shared_preload_libraries

Now you have the custom parameter group you can edit values. Click on the newly created group and filter the list of parameters by entering `shared_preload_libraries` into the filter box shown below.

![Screenshot-2022-09-05-at-03.53.46](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.53.46.png)

Select the `shared_preload_libraries` parameter check box and press `Edit parameters` button. Add `, pg_cron` to the end of the list and save. You should see `pg_cron` added to the list of `Allowed values`

## Apply the parameter group to the database

Go back to the dashboard and select your database and press the modify button. Near the bottom you'll see the `Additional configuration` section. Select your custom parameter group from the dropdown list `DB parameter group` and hit `continue` then `Modify DB instance`. Below shows the `Additional configuration` section.

![Screenshot-2022-09-05-at-03.58.37](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-03.58.37.png)

## Restart DB instance

The changes won't take into effect until you have restarted the DB. Go to the dashboard select your database and select `Reboot`

![Screenshot-2022-09-05-at-04.01.15](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-04.01.15.png)

## Enable pg_cron in the database

To create a cron job, authenticate to your database using your database client of choice. 

Select the `postgres` database **not** the database with your application data in (this confused me the first time I did it. Below shows me interacting with the `postgres` database.

![Screenshot-2022-09-05-at-04.16.26](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2022/09/Screenshot-2022-09-05-at-04.16.26.png)

Run the following SQL command against the database

```
CREATE EXTENSION pg_cron;
```

## Create the scheduled job

The following is ran on the **postgres** database, not the database with your data in it. This is really important!

To create a scheduled job use a special function named `schedule` that was enabled when you created the `pg_cron` extension. The first argument is a human readable name to give the job, such as `delete test data`. The second is how often you want it to run `0 0 * * *` would run it at midnight every night. The last argument is the command you want to run. Using `$$` at the beginning and end of the input means you can use `'` as part of the command. See the example below that deletes every row in `PRODUCTS` where `USER` equals `00000000-0000-0000-0000-000000000000`

```
SELECT cron.schedule('delete test data', '0 0 * * *', $$DELETE FROM PRODUCTS WHERE USER = '00000000-0000-0000-0000-000000000000'$$);
```

This command will output a number, which will be `1` if it's your first job. Use that number as the `jobid` in the next statement. The name of the `database` is the one with all your data in it. In my case it's `productdatabase`

```
UPDATE cron.job SET database = 'productdatabase' WHERE jobid = 1;
```


## Verify it all works

To check the job is set up correctly run the following to output all jobs.

```
SELECT * FROM cron.job;
```

Once the job has ran, in my case after midnight, run the following command on the `postgres` database to output details from previous runs.

```
SELECT * FROM cron.job_run_details;
```

## Using pg_cron to automate your test cleanup

If you need to clean up your test data automatically pg_cron is a great option that runs transparently in the background. You may need to run multiple commands to clean up your data. For further tips on `pg_cron` [AWS have a good write up of different commands](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL_pg_cron.html#PostgreSQL_pg_cron.otherDB) that are included in the extension such as `unschedule` for removing the job.









