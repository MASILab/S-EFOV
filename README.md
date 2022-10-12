# Thoracic CT-based Body Composition Analysis with Semantically Extended FOV
###  [[Project page]](https://github.com/MASILab/S-EFOV/)[[Paper (arXiv)]](https://arxiv.org/abs/2207.06551) 

Body composition (BC) analysis, which captures the physical and
constitutional characteristics of the human body, can provide valuable
predictive information for various health
conditions. In this work, we developed a fully automatic BC measurement pipeline using routine lung screening chest 
low-dose computed tomography (LDCT). To overcome the systematic
field-of-view (FOV) limitations that causing body tissue truncation, we proposed a two-stage method to 
extend the image border and generate anatomically consistent body tissues in the truncated regions. 

A typical result report obtained for a lung cancer screening CT: 
<img src="https://github.com/MASILab/S-EFOV/blob/master/materials/report_example.jpg" width="600px"/>

## Copyright

The contents covered by this repository, including code and pretrained models in the docker container, 
are free for noncommercial usage (CC BY-NC 4.0). Please check the LICENSE.md file for more details of the copyright 
information.

## Citation

If you find this study can help your work, please cite the following papers:

[1] Kaiwen Xu, Thomas Li, Mirza S. Khan, Riqiang Gao, Sanja L. Antic, Yuankai Huo, 
Kim L. Sandler, Fabien Maldonado, Bennett A. Landman. (2022). 
Body Composition Assessment with Limited Field-of-view Computed Tomography: 
A Semantic Image Extension Perspective. https://arxiv.org/abs/2207.06551. 
(Submitted to Medical Image Analysis)

[2] Kaiwen Xu, Riqiang Gao, Yucheng Tang, Steve A. Deppen, Kim L. Sandler, Michael N. Kammer, Sanja L. Antic, 
Fabien Maldonado, Yuankai Huo, Mirza S. Khan, Bennett A. Landman, "Extending the value of routine lung screening 
CT with quantitative body composition assessment," Proc. SPIE 12032, Medical Imaging 2022: Image Processing, 
120321L (4 April 2022); https://doi.org/10.1117/12.2611784

## Quick Start
#### Get the docker image
```
docker pull masidocker/public:lung_body_composition_v1.0.2
```
#### Prepare input data
User needs to prepare an input folder which contains the CT images in NIfTI format (.nii.gz) and
(optionally) a csv file for metadata to provide the height of the patient.

Step.1 Specify the input directory 
```
export INPUT_DIR=/home/input_dir
```

Step.2 Put NIfTI images under folder $INPUT_DIR/NIFTI without sub-folder

Step.3 Create a metadata.csv under $INPUT_DIR, which contains the following two fields: 
```
- Filename
- HeightMeters
```
This metadata file is optional. However, the height data are needed to get the normalized BC indexes.

#### Example input dataset
An example input dataset can be obtained via https://doi.org/10.5281/zenodo.6853516

Note: as we do not have the permission to share the lung screening CT data used in our published papers, 
the example dataset are compiled using four thoracic CT scans selected from the following public available (CC BY 4.0) dataset:
[The Caner Imaging Archive (TCIA), Chest Imaging with Clinical and Genomic Correlates Representing a Rural COVID-19 Positive Population 
(COVID-19-AR)](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=70226443#70226443171ba531fc374829b21d3647e95f532c).
These chest CT imaging studies were acquired for COVID-19 patients. Though these scans were not acquired 
under lung cancer screening protocols, they can still serve for demonstration purposes. 

#### Run docker container
Specify output location
```
export OUTPUT_DIR=$INPUT_DIR/output
mkdir -p $OUTPUT_DIR
```

Run docker container with input/output locations
```
sudo docker run -it --gpus all --rm -v $INPUT_DIR:/Input -v $OUTPUT_DIR:/Output masidocker/public:lung_body_composition_v1.0.2 /app/src/Scripts/docker.sh
```

For the folder structure of the output directory, please refer to the README.txt file prepared under the output location.
Output folder structure:
```
+ measurements.csv - per image file measurement results, including TCI value (truncation severity) and FOV extension ratio
+ Report_pdf - report in pdf format
+ Temp - all intermediate results
+ log - run time log file for debug
```

## Environment Setup

#### Tested platform
- Ubuntu 20.04/22.04
- cuda 11.2/11.3
- Docker version 20.10.14
- Nvidia-docker version 2.10.0
- GPU memory requirement: 12 GB


#### Install Docker
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt-get update
sudo apt-get install docker-ce
```

#### Install Nvidia-Docker
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

## Disclaimer

The code and data of this repository are provided to promote reproducible research. 
They are not intended for clinical care or commercial use.

The software is provided "as is", without warranty of any kind,
express or implied, including but not limited to the warranties of merchantability, 
fitness for a particular purpose and noninfringement. 
In no event shall the authors or copyright holders be liable for any claim, damages or other liability, 
whether in an action of contract, tort or otherwise, arising from, 
out of or in connection with the software or the use or other dealings in the software.