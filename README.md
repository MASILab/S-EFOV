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
Kim L. Sandler, Fabien Maldonado, Bennett A. Landman. (2022). Body Composition Assessment with Limited Field-of-view Computed Tomography: A Semantic Image Extension Perspective. https://arxiv.org/abs/2207.06551. (Submitted to Medical Image Analysis)

[2] Kaiwen Xu, Riqiang Gao, Yucheng Tang, Steve A. Deppen, Kim L. Sandler, Michael N. Kammer, Sanja L. Antic, Fabien Maldonado, Yuankai Huo, Mirza S. Khan, Bennett A. Landman, "Extending the value of routine lung screening CT with quantitative body composition assessment," Proc. SPIE 12032, Medical Imaging 2022: Image Processing, 120321L (4 April 2022); https://doi.org/10.1117/12.2611784

## Quick Start
#### Get the docker image
```
docker pull masidocker/public:lung_body_composition_v1.0.1
```
#### Prepare input data
User need to prepare an input folder which contains the CT images in NIfTI format (.nii.gz) and
(optionally) a csv file for metadata to provide the height of the patient.

Step.1 Specify the input directory 
```
export INPUT_DIR=/home/input_dir
```

Step.2 Put NIfTI images under folder $input_dir/NIFTI without sub-folder

Step.3 Create a metadata.csv under $INPUT_DIR, which contains the following two fields: 
```
- Filename
- HeightMeters
```
This metadata file is optional. However, the height information is needed to get the normalized BC indexes.

#### Example input dataset
An example input dataset can be obtained via [https://doi.org/10.5281/zenodo.6853516]

As we do not have to permission to share the thoracic CT data used in the published papers, 
the example dataset are compiled using four thoracic CT scans selected from the following public available (CC BY 4.0) dataset:
[The Caner Imaging Archive (TCIA), Chest Imaging with Clinical and Genomic Correlates Representing a Rural COVID-19 Positive Population 
(COVID-19-AR)](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=70226443#70226443171ba531fc374829b21d3647e95f532c).
These chest CT imaging studies were acquired for COVID-19 patients.

#### Run docker container
Specify output location
```
export OUTPUT_DIR=$INPUT_DIR/output
mkdir -p $OUTPUT_DIR
```

Run docker container with input/output location binding
```
sudo docker run -it --gpus all --rm -v $INPUT_DIR:/Input -v $OUTPUT_DIR:/Output masidocker/public:lung_body_composition_v1.0.1 /app/src/Scripts/docker.sh
```

#### Testing platform
- Ubuntu 20.04
- cuda 11.2/11.3
- Docker version 20.10.14
- Nvidia-docker version 2.10.0


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
