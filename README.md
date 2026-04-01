# MOD_fish_calibrations

Calibration files needed for MOD_fish_acquisition and MOD_fish_processing

- Initial commit with .CAL files has the oldest calibration files for each Seabird 49. 
- Later commits replace the calibration files when new ones come in from Seabird. 
- There will only ever be one file per instrument. To use previous files, checkout the appropriate tagged commit.


### Initial commit:


| SBE Calibration File | Calibration Date |
|-------------------|------------------|
| 0057.CAL | 2022_04 |
| 0060.CAL | 2022_04 |
| 0131.CAL | 2022_04 |
| 0133.CAL | 2019_02 |
| 0237.CAL | 2023_03 |
| 0387.CAL | 2019_02 |
| 0536.CAL | 2020_03 |
| 0537.CAL | 2020_03 |
| 0664.CAL | 2023_01 |
| 0674.CAL | 2023_04 |

### Example of later commits:

SBE 0387 and 0537 came back from factory calibration in March 2023.

```
cp NEWCALS/0387.CAL SBECAL/0387.CAL
cp NEWCALS/0537.CAL SBECAL/0537.CAL
git add SBECAL/0387.CAL SBECAL/0537.CAL
git commit -m "Update 0387.CAL and 0537.CAL - factory calibrations March 2023"
git tag -a cal/2023-03 -m "Calibration update March 2023"
```
## Working with tags


### Tag naming convention
```
cal/YYYY-MM          # marks when a calibration file was updated
cruise/YYYY-MM-CRUISE  # marks which files were on the instruments for a cruise
```

### See all tags

```
git tag -l "cal/*"       # see all calibration date tags
git tag -l "cruise/*"    # see all cruise tags
```

### See which files changed between two tags

```
git diff cal/2023-03 cal/2025-03
```

### See the history of a specific instrument:
```
git log --oneline -- SBECAL/0387.CAL
```

## Using old calibration files to reprocess data

If you need to reprocess data from an old cruise and want al files to reflect the state they were in at that time, extract the complete snapshot of the repo into a new folder on your machine. This will leave the calibrations repo completely untouched so you don't accidentally make changes started at an old state or forget to go back to main.

```
mkdir ~/Desktop/reprocess_CRUISE_NAME
cd ~/GitHub/MOD_fish_calibrations
git archive cal/2023-03 | tar -x -C ~/Desktop/reprocess_CRUISE_NAME
```

Point your processing scripts at ```~/Desktop/reprocess_CRUISE-NAME```
instead of the calibrations repo for the duration of reprocessing.