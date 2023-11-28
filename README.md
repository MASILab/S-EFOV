# Thoracic CT-based Body Composition Analysis with Semantically Extended FOV

This repository provides instructions for the body composition assessment tool described in paper 
["AI Body Composition in Lung Cancer Screening: Added Value Beyond Lung Cancer Detection"](https://pubs.rsna.org/doi/10.1148/radiol.222937) 
published in *Radiology* (2023).

+ See [QUICK_START.md](https://github.com/MASILab/S-EFOV/blob/master/QUICK_START.md) for instructions of 
  using the Docker image.

#### The repository has been updated since the acceptance of the paper. The original repository can be access via [commit b2ab4db9cf17a9ebe4b13b195491341ac687636b](https://github.com/MASILab/S-EFOV/tree/b2ab4db9cf17a9ebe4b13b195491341ac687636b) (Oct 11, 2022, docker image version v1.0.2)
---

Body composition analysis, which captures the physical and
constitutional characteristics of the human body, can provide valuable
predictive information for various health
outcomes. In this work, we developed a fully automatic pipeline to derive body composition measurements from 
routine lung screening chest low-dose computed tomography (LDCT). To overcome the systematic
field-of-view (FOV) limitations that causing body tissue truncation, we proposed a two-stage method to 
extend the image border and generate anatomically consistent body tissues in the truncated regions. 
See [our manuscript](https://www.sciencedirect.com/science/article/abs/pii/S1361841523001123?via%3Dihub) 
published in *Medical Image Analysis* (2023) for more details.

An overview of the framework:

<img src="https://github.com/MASILab/S-EFOV/blob/master/materials/fig-method-overall-workflow.jpg" width="600px"/>

A typical result report obtained for a lung cancer screening CT:

<img src="https://github.com/MASILab/S-EFOV/blob/master/materials/report_example.png" width="600px"/>

## Citation

If you find this study can help your work, please consider to cite the following papers.

#### The body composition measurements improved the prediction for lung cancer death, CVD death, and all-cause mortality in the CT arm of the [National Lung Screening Trial (NLST)](https://www.cancer.gov/types/lung/research/nlst):

Kaiwen Xu, Mirza S. Khan, Thomas Li, Riqiang Gao, James G. Terry, Yuankai Huo, John Jeffrey Carr, Fabien Maldonado,
Bennett A. Landman, Kim L. Sandler (2023). AI Body Composition in Lung Cancer Screening: Added Value Beyond Lung 
Cancer Detection. *Radiology*, 308(1). https://doi.org/10.1148/radiol.222937

#### A two-stage FOV extension module to compensate for the systematic FOV restriction: 

Kaiwen Xu, Thomas Li, Mirza S. Khan, Riqiang Gao, Sanja L. Antic, Yuankai Huo, 
Kim L. Sandler, Fabien Maldonado, Bennett A. Landman. (2023). 
Body composition assessment with limited field-of-view computed tomography: A semantic image extension perspective.
*Medical Image Analysis*, 88, 102852. https://doi.org/10.1016/j.media.2023.102852
[[arxiv version]](https://arxiv.org/abs/2207.06551)

#### An early version of the pipeline, with automatic slice level selection and body composition segmentation:  

Kaiwen Xu, Riqiang Gao, Yucheng Tang, Steve A. Deppen, Kim L. Sandler, Michael N. Kammer, Sanja L. Antic, 
Fabien Maldonado, Yuankai Huo, Mirza S. Khan, Bennett A. Landman, "Extending the value of routine lung screening 
CT with quantitative body composition assessment," Proc. SPIE 12032, Medical Imaging 2022: Image Processing, 
120321L (4 April 2022); https://doi.org/10.1117/12.2611784 
[[PubMed version]](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9604426/)

## License

This project is under the CC BY-NC 4.0 license. See
[LICENSE.md](https://github.com/MASILab/S-EFOV/blob/master/LICENSE.md) for more details.

## Disclaimer

The code and data of this repository are provided to promote reproducible research. 
The software is provided "as is", without warranty of any kind,
express or implied, including but not limited to the warranties of merchantability, 
fitness for a particular purpose and noninfringement. 
In no event shall the authors or copyright holders be liable for any claim, damages or other liability, 
whether in an action of contract, tort or otherwise, arising from, 
out of or in connection with the software or the use or other dealings in the software.
