---
tags:
  - Free
---

# WhiteboxTools

[WhiteboxTools](https://www.whiteboxgeo.com/manual/wbt_book/intro.html) is an advanced geospatial data analysis platform which includes more than 480 tools. Many tools operate in parallel, taking full advantage of your multi-core processor.

## Available

WhiteboxTools is available in Puhti with following versions:

* 2.1.0. Only WhiteboxTools Open Core tools are available on Puhti.

## Usage

WhiteboxTools is available in the __WhiteboxTools__ module and can be loaded with

```
module load whiteboxtools
whitebox_tools --version
```

Here is an example of calculating Hillshade for a 10m DEM. 

```
whitebox_tools -r=Hillshade -v -i=/appl/data/geo/mml/dem10m/2019/M3/M34/M3444.tif -o=test_hillshade.tif --azimuth=315.0 --altitude=30.0
```

!!! note
    Whiteboxtools seem to have problems with Finnish NLS lidar files.

## Example batch job script

```
#!/bin/bash
#SBATCH --account=<YOUR-PROJECT>
#SBATCH --cpus-per-task=1
#SBATCH --partition=small
#SBATCH --time=00:10:00
#SBATCH --mem=2G

module load whiteboxtools
whitebox_tools -r=Hillshade -v -i=/appl/data/geo/mml/dem10m/2019/M3/M34/M3444.tif -o=test_hillshade.tif --azimuth=315.0 --altitude=30.0
```

## License and acknowledgement

The WhiteboxTools open-core platform is distributed under the [MIT license](https://www.whiteboxgeo.com/manual/wbt_book/license.html)

Please acknowledge CSC and Geoportti in your publications, it is important for project continuation and funding reports.
As an example, you can write "The authors wish to thank CSC - IT Center for Science, Finland (urn:nbn:fi:research-infras-2016072531) and the Open Geospatial Information Infrastructure for Research (Geoportti, urn:nbn:fi:research-infras-2016072513) for computational resources and support".

### References

* [WhiteboxTools User Manual](https://www.whiteboxgeo.com/manual/wbt_book/intro.html)
* [Whitebox Geospatial Inc](https://www.whiteboxgeo.com/)
* [WhiteboxTools Github](https://github.com/jblindsay/whitebox-tools)



