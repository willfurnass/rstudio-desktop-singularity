# Running RStudio Desktop in a Singularity container on the University of Sheffield's ShARC cluster

**Work in progress**

## Building a Singularity container with R, RStudio and VirtualGL

From a machine with user namespaces enabled (typically not the case on HPC):

```sh
singularity build --fakeroot rstudio-desktop.simg rstudio-desktop.def
```

(or if user namespaces are disabled omit `--fakeroot` and prefix the line with `sudo`)

## Running with hardware-accelerated graphics

 1. Copy the built image to HPC
 1. Start a hardware-accelerated visualisation session on a worker node
 1. Connect to that via TigerVNC
 1. Run on the node via TigerVNC shell: 

    ```sh
    singularity exec --nv rstudio-desktop.simg vglrun rstudio
    ```

## Running without hardware-accelerated graphics

 1. Copy the built image to HPC
 1. SSH to HPC with X forwarding enabled
 1. Start an interactive session on a worker node with X forwarding enabled
 1. Tell RStudio not to try using hardware-accelerated graphics by adding the following to `$HOME/.config/RStudio/desktop.ini` beneath a `[General]` heading:

    ```ini
    desktop.renderingEngine=software
    general.disableGpuDriverBugWorkarounds=false
    general.ignoreGpuBlacklist=false
    ```
    
 1. Run on that node: 

    ```sh
    singularity exec rstudio-desktop.simg rstudio
    ```
