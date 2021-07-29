# D2U-artifacts
Training data and example configuration files for the CSET paper "D2U: Data Driven User Emulation for the Enhancement of Cyber Testing, Training, and Data Set Generation" - see https://cset21.isi.edu/program.html. 

Folder structure with brief explanations:
├── participant1 - *indicates data pertaining to user 1*
│   ├── collected-data - *folder of .csvs with app use per second collected from user 1*
│   │   └── p1_*.csv - *two-column csv of form `timestamp, app_name`*
│   ├── example-configuration-files - *folder of .json files*
│   │   └── user\*.json *each gives commands to an emulated users as sampled from the model*
│   └── model-output - *folder of .json files, each is a python list of dicts. Each dict has the app, it's start time, the duration in seconds to remain in the app*
│   │   └── <model info>_*.json
└── participant2
    ├── collected-data
    ├── example-configuration\ files
    └── model-output

## collected-data/
- each csv is two columns, form `timestamp, app_name`*
- this data was collected off a real user and used for training the models.
- example:
```
1586199368.930281	Terminal
1586199369.543644	Terminal
1586199370.149858	Slack
1586199370.760703	Slack
...

```

## example-configuration-files/
- each user*.json is a created from the model outputs and contains information to tell each emulated user what to do

## model-output/
- each .json file provides a sequence of app usage for an emulated user as sampled from a model.
- each .json is a python list of dicts
- each dict includes the time, app, and duration to remain in that app
- Example:
```
[
    {
        "application":"loginwindow",
        "duration":3080,
        "starttime":"08:00:00"
    },
    {
        "application":"Teams",
        "duration":95,
        "starttime":"08:51:20"
    },
    ...
]
```
- these are processed into the corresponding example-configuration-files, which provide the details of how to take each app action to the emulated user.
- naming convention: <model_info>_<index>.json, for example `mm_dsds_order_2_temporal_window_1hr_clusters_7_0001.json` indicates
  - `mm` and `order_2` indicates this data was sampled from a markov model of order 2
  - `dsds` indicates the DSDS structure was used (see paper, this is the view of the data where each app sequence is considered as [(app1, time duration to remain in app1), (app2, time duration to remain in app2), ...]),
  - `temporal_window` indicates the hierarchical structure was used, which entails clustering 1-hour windows with k-means.
    - `clusters_7` indicates k-means was used with `k = 7` clusters
  - the index `0001` indicates it's the first sample from this model.  
