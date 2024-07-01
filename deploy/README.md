docker build -t bioconnect-jbqr:0.0.1 .

docker run -ti --rm bioconnect-jbqr:0.0.1 R


docker build -t bioconnect-jbqr:0.0.1 .
docker tag 62c2feceefb3 hongpingliang/r-base_devtools:4.4.1

docker tag 397559bd80af hongpingliang/bioconnect-jbqr:0.0.1

docker run -ti --rm bioconnect-jbqr:0.0.1 R

docker tag c6f029554e3e hongpingliang/bioconnect-jbqr:0.0.1
docker push hongpingliang/bioconnect-jbqr:0.0.1

module load singularity
builder="singularity run http://s3-far.jax.org/builder/builder"
$builder bioconnect_jbqr.def bioconnect_jbqr.sif
singularity push -U bioconnect_jbqr.sif library://liangh/cube/bioconnect_jbqr.sif:0.0.1

srun -p compute -q batch --mem=128g -t 48:00:00 -c 1 --pty $SHELL
module load singularity
singularity exec bioconnect_jbqr.sif R

library("logger")
library("jbqr")

jbq_obj <- JBQ$new('JAXDP00006X.zip')

study_sources <- jbq_obj$get_study_sources()
study_sources

options("width"=200)

jbq_obj$show_files()

jbq_obj$get_files("*liver*")

## Run R on sumner2

it's already there... there are a couple of options - an interactive R session or using Rstudio on sumner2.
for an interactive R session, specify the memory and time. Here I specified 48 hrs and 128GB. Then, call a .sif -- I'm sure I have one you can access but there are a lot floating around. If you need a package not in the sif, open an interactive R session and use install.packages("packagename")
```
srun -p compute -q batch --mem=128g -t 48:00:00 -c 1 --pty $SHELL
module load singularity
singularity exec /projects/chesler-lab/sifs/rstudio.4.0.3_v1.2.simg R
```

for rstudio (I'm no expert) but this is one option...
```
Login into Sumner2:
ssh login.sumner2.jax.org
 [yanicr@sumner-log4 ~]$ module avail
Load the module:
[yanicr@sumner-log4 ~]$ module load rstudio
***Rstudio 4.3.1 loaded. Use the command 'rstudio-session-4.3.1' to start a new session.
To view the help guide and learn how you can request more resources, use the -h or --help flag:
[yanicr@sumner-log4 ~]$ rstudio-session-4.3.1 --help
Usage: rstudio-session-4.3.1 [OPTIONS...]
Option Description
 -h    print this message
 -m    amount of real memory
 -c    number of cpus required
 -y    don't ask for confirmation
Begin a session by running the rstudio-session-4.3.1 command.  If you run rstudio-session-4.3.1 binary without any arguments, the resulting session will have 2 cpus and 8G of memory by default.
```
