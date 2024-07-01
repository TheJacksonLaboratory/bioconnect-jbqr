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
