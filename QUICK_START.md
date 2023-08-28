
## Instructions of using the body composition analysis Docker image
[Setup Docker environment](#Environment Setup)
#### Get the docker image
```
docker pull masidocker/public:lung_body_composition_v1.0.3
```
#### Prepare input data
Create an input folder which contains the CT images in NIfTI format (.nii.gz) and
(optionally) a csv file for metadata to provide the height of the patient.

Step.1 Specify the input directory 
```
INPUT_DIR=./Input
```

Step.2 Put NIfTI images under folder $INPUT_DIR/NIFTI without sub-folder

Step.3 Create a file named "metadata.csv" under $INPUT_DIR, which contains the following two fields: 
```
- Filename
- HeightMeters
```
This metadata file is optional. However, the height data are needed to get the normalized (by height squared) BC 
indexes.

#### Run docker container
Specify output location
```
OUTPUT_DIR=./Output
mkdir -p $OUTPUT_DIR
```

Run docker container
```
docker run -it \
  --gpus '"device=0"' \
  --rm \
  -v ${INPUT_DIR}:/Input:ro -v ${OUTPUT_DIR}:/Output \
  -u "$(id -u):$(id -g)" \
  masidocker/public:lung_body_composition_v1.0.3 \
  /app/src/Scripts/docker.sh
```

Note: for Linux, some [post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/) are needed 
to run Docker as non-root users. The "-u" ("--user") argument is specified to solve the 
[output file permission problem of Docker on Linux](https://vsupalov.com/docker-shared-permissions/).

#### Output
The body composition measurements will be generated in a csv file at the output location
```
measurements.csv:
+ filename - Input NIfTI file name
+ tci_T[5|8|10] - Tissue Truncation Index at level T[5|8|10] (higher value indicating more severe truncation)
+ ext_ratio_T[5|8|10] - Field-of-view extension ratio at level T[5|8|10]
+ [Muscle|SAT]_T[5|8|10]_{area|index} - Measured skeletal muscle or SAT area or area index at level T[5|8|10]
+ [Muscle|SAT]_{area|index|mean|std} - The summed area or area index, or mean attenuation or std measurements across 
three levels.
```

Intermediate files are generated under a folder named 
"Temp" at run time. This folder (can be very large) will be removed at the end of the pipeline by default.
Specify "--keep_temp" option to keep these temporary files:
```
  ...
  /app/src/Scripts/docker.sh --keep_temp
```

A list of output files
```
Contents:
+ measurements.csv - Per image file measurement results, including TCI value (truncation severity) and FOV extension ratio
+ Report_pdf - Reports in pdf format
+ Report_png - Reports in png format
+ README.txt - Document of output folder structure
+ Temp - Intermediate files, only when "--keep_temp" specified. Mainly for debugging
```

#### Example dataset
An example dataset with pre-generated results can be obtained via https://doi.org/10.5281/zenodo.8290372

Note: as we do not have the permission to share the lung screening CT data used in our publications, 
the example dataset is compiled using four thoracic CT scans selected from the following public available (CC BY 4.0) 
dataset:
[The Caner Imaging Archive (TCIA), Chest Imaging with Clinical and Genomic Correlates Representing a Rural COVID-19 Positive Population 
(COVID-19-AR)](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=70226443#70226443171ba531fc374829b21d3647e95f532c).
These chest CT imaging studies were acquired for COVID-19 patients and may not be acquired 
using lung cancer screening protocols. This demo dataset is provided only to help validate the integrity of the 
pipeline.

```
$ tree ./TCIA_demo
./TCIA_demo
├── Input
│   ├── metadata.csv
│   └── NIFTI
│       ├── COVID-19-AR-16406490.nii.gz
│       ├── COVID-19-AR-16406503.nii.gz
│       ├── COVID-19-AR-16424081.nii.gz
│       └── COVID-19-AR-16445138.nii.gz
├── Output
│   ├── measurement.csv
│   ├── README.txt
│   ├── Report_pdf
│   │   ├── COVID-19-AR-16406490.pdf
│   │   ├── COVID-19-AR-16406503.pdf
│   │   ├── COVID-19-AR-16424081.pdf
│   │   └── COVID-19-AR-16445138.pdf
│   └── Report_png
│       ├── COVID-19-AR-16406490.png
│       ├── COVID-19-AR-16406503.png
│       ├── COVID-19-AR-16424081.png
│       └── COVID-19-AR-16445138.png
└── run.sh

```

run.sh
```
#!/bin/bash

INPUT_DIR=./Input
OUTPUT_DIR=./Output

mkdir -p ${OUTPUT_DIR}

docker run -it \
  --gpus '"device=0"' \
  --rm \
  -v ${INPUT_DIR}:/Input:ro -v ${OUTPUT_DIR}:/Output \
  -u "$(id -u):$(id -g)" \
  masidocker/public:lung_body_composition_v1.0.3 /app/src/Scripts/docker.sh
```

## Environment Setup

#### Tested platform
- Ubuntu 20.04/22.04
- Docker version 20.10.14/23.0.0
- GPU memory requirement: < 6 GB

#### Install Docker
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt-get update
sudo apt-get install docker-ce
```

#### Install NVIDIA

```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
```

## Convert Image Data to NIfTI Format

#### Convert from DICOM

dcm2niix was the tool we used in our study to convert DICOM images to NIfTI format. The software is 
freely available at https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage

Use dcm2niix to convert DICOM images to NIfTI images:
```
dcm2niix -z y -f %j -o /path_to_output /path_to_input_dicom_folder
```

This will generate a NIfTI format image (.nii.gz) with `seriesInstanceUID` as the filename, along with a json file 
for key metadata.

The version of dcm2niix used in our study: `v1.0.20211006  (JP2:OpenJPEG) (JP-LS:CharLS) GCC7.5.0 x86-64 (64-bit Linux)`

#### Convert from other imaging formats

[Convert3D (c3d)](http://www.itksnap.org/pmwiki/pmwiki.php?n=Convert3D.Convert3D) 
is able to convert images generated from DICOM in other format to NIfTI. The input can be any image
format that readable by ITK.

For example, convert an image in Nrrd ("nearly raw raster data") format to NIfTI
```
c3d ./img.nrrd -o ./img.nii.gz
```

## Changelog

#### v1.0.3 (August 28, 2023)

+ Update the citations.
+ Provide the area index (area normalized by height squared) in the measurement csv file.
+ Extend the documents in README.txt.
+ Address [multiple ease-of-use problems](https://github.com/MASILab/S-EFOV/issues/1).
  + Remove intermediate files by default and provide the option "--keep_temp" to keep these files. 
  + Solve the permission problem following the second option mentioned in https://vsupalov.com/docker-shared-permissions/
+ Update the zenodo demo, to include complete folder structures for both inputs and pre-generated outputs, as well 
  as an example run.sh file on how to run the Docker image.

#### v1.0.2 (October 6, 2022)

This is version used in publication https://pubs.rsna.org/doi/10.1148/radiol.222937

+ Add the measurements for mean attenuation and standard deviation.

#### v1.0.1 (July 17, 2022)