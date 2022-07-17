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

Step.3 Create a metadata.csv under $INPUT_DIR, which should contain the following two fields: 
```
- Filename
- HeightMeters
```

An example for input data structure can be obtained via

```
TODO
```

Note: as we do not have to permission to share data related to neither the in-house lung screening dataset (VLSP)
nor the National Lung Screening Trial (NLST), the example data are generated using (TODO) ### cases from ### dataset 
(link). (Need to mention if the data is lung screening related or not.)

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


#### install Docker
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt-get update
sudo apt-get install docker-ce
```

#### install Nvidia-Docker
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
```

## Additional examples

#### Lung screening chest CT

#### Routine diagnostic chest CT
