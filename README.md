# Routine Lung CT-based Body Composition Analysis with Semantically Extended Field-of-view 
###  [[project page]](https://github.com/MASILab/S-EFOV/) [[Paper (arXiv, TODO)]](https://arxiv.org/ftp/arxiv/papers/1906/1906.09549.pdf) 

In this work, we evaluate the body composition profiles, including muscle and subcutaneous adipose tissue, using the cross-sectional areas measured on T5, T8, and T10 vertebral levels.
Field-of-view limitation-caused tissue truncation is common in routinely acquired lung screening CT. 
We introduce a semantic FOV extension procedure to correct the measurement offset caused by this limitation in FOV.

<img src="https://github.com/MASILab/S-EFOV/blob/master/materials/report_example.jpg" width="600px"/>

## Citation
If you find this study can help your work, please cite the following paper.
```
TODO
```

## License

The materials published in this repository, including code and models in the docker container, 
are free for noncommercial usage (CC BY-NC 4.0). For details of the license, please check the LICENSE.md file. 

## Quick Start
#### Get the docker image
```
sudo docker pull kwxu:sefov_bcomp:v0.0.3
```
#### Prepare input data
The user should specify an input folder which contains the CT image in NIfTI format and a csv file for metadata. 
The analysis result will be generated at the specified output location.

Step.1 Specify the input directory 
```
export input_dir=/home/input_dir
```

Step.2 Put nifti images under folder $input_dir/NIFTI without sub-folder

Step.3 Create a metadata.csv under $input_dir, which contains the following fields: 
```
- Patient ID
- Scan Date
- NIFTI Filename
- Sex
- Height (m)
```

#### Run docker container
Specify output location
```
export output_dir=$input_dir/output
mkdir -p $output_dir
```

Run docker container with input/output location binding
```
sudo docker run -it --gpus all --rm -v $input_dir:/Input -v $output_dir:/Output kwxu:sefov_bcomp:v0.0.3 /app/src/Scripts/docker.sh
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


