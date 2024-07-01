FROM r-base:4.4.1

RUN R -e "install.packages(c('devtools', 'testthat', 'roxygen2', 'logger'))"

RUN R -e "remotes::install_github('TheJacksonLaboratory/bioconnect-jbqr')"
