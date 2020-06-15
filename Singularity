BootStrap: library
From: ubuntu:20.04

%files
  renv.lock
  environment.yml

%post
  # Install R
  sed -i 's/main/main restricted universe/g' /etc/apt/sources.list
  echo 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/' >> /etc/apt/sources.list
  apt-get -y install gnupg ca-certificates
  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
  apt update

  apt-get -y install locales r-base r-base-dev
  apt-get clean
  locale-gen en_US.UTF-8

  # R packages
  apt-get -y install libxml2-dev libcurl4-openssl-dev libssl-dev libgdal-dev libudunits2-dev pandoc
  Rscript -e 'install.packages("renv", repos="https://cloud.r-project.org/")'
  mkdir /opt/renv
  echo "RENV_PATHS_CACHE = /opt/renv" >> $(R RHOME)/etc/Renviron.site
  Rscript -e "options(renv.consent = TRUE); renv::restore()"

  # Install Anaconda
  apt-get -y install wget
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  bash Miniconda3-latest-Linux-x86_64.sh -bfp /usr/local

  # Python packages
  conda env create -f environment.yml

%test
  #!/bin/sh
  exec Rscript -e "library(dplyr)"
