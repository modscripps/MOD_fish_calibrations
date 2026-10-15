# MOD_fish_calibrations

Calibration files needed for MOD_fish_acquisition and MOD_fish_processing

- Initial commit with .CAL files has the oldest calibration files for each Seabird 49. 
- Later commits replace the calibration files when new ones come in from Seabird. 
- There will only ever be one file per instrument. To use previous files, see [Using old calibration files to reprocess data](#using-old-calibration-files-to-reprocess-data) - don't just check out an old commit.

### Files every processing run needs

Besides the per-instrument calibrations (`SBE/`, `SHEAR_PROBES/`), MOD_fish_processing needs:

| File | What it's for |
|---|---|
| `SHEAR/cap1nFres200Meg_5KohmInput.mat` | Shear charge-amp electronics response - without it epsilon is computed with no electronics correction |
| `FPO7/FPO7_benchnoise.mat` | FP07 bench noise floor - used to find where an FP07 spectrum drops into noise for chi |

These were added later than many calibration tags, so an old commit or tag may not have them.


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
cp NEWCALS/0387.CAL SBE/0387.CAL
cp NEWCALS/0537.CAL SBE/0537.CAL
git add SBE/0387.CAL SBE/0537.CAL
git commit -m "Update 0387.CAL and 0537.CAL - factory calibrations March 2023"
git tag -a cal/2023-03 -m "Calibration update March 2023"
```
## Working with tags


### Tag naming convention
```
sbe/YYYY-MM           # marks when a SBE calibration file was updated
shear/YYYY-MM         # marks when new shear probe calibration files were uploaded or existing files updated
cruise/YYYY-MM-CRUISE # marks which files were on the instruments for a cruise
```

### See all tags

```
git tag -l "sbe/*"       # see all SBE calibration date tags
git tag -l "cruise/*"    # see all cruise tags
```

### See which files changed between two tags

```
git diff sbe/2023-03 sbe/2025-03
```

### See the history of a specific instrument:
```
git log --oneline -- SBE/0387.CAL
```

## Using old calibration files to reprocess data

If you need to reprocess data from an old cruise with the calibrations that were in use then, **don't just check out (or archive) the old commit** - an old snapshot can be missing files processing now needs (see [Files every processing run needs](#files-every-processing-run-needs)), and processing would quietly run without them. Instead, start from the current files and replace only the calibration files you need from the old tag. Do it in a new folder, so the calibrations repo itself stays untouched and on main.

```
mkdir ~/Desktop/reprocess_CRUISE_NAME
cd ~/GitHub/MOD_fish_calibrations
git archive main | tar -x -C ~/Desktop/reprocess_CRUISE_NAME     # everything, current
# then each old file you need, on top (here SBE 0387 and 0537 as of March 2023):
git show sbe/2023-03:SBECAL/0387.CAL > ~/Desktop/reprocess_CRUISE_NAME/SBE/0387.CAL
git show sbe/2023-03:SBECAL/0537.CAL > ~/Desktop/reprocess_CRUISE_NAME/SBE/0537.CAL
```

Note the old path: before the folder was renamed (commit `d14f32c`), the SBE files were in `SBECAL/`, so tags up to and including `sbe/2025-03` have them there, while the current files are in `SBE/`. `git ls-tree -r --name-only <tag>` lists what a tag holds. For an old shear probe calibration, copy from `SHEAR_PROBES/<SN>/Calibration_<SN>.txt` the same way.

Point your processing scripts at ```~/Desktop/reprocess_CRUISE-NAME```
instead of the calibrations repo for the duration of reprocessing.