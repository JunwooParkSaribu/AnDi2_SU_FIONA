[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.13334951.svg)](https://doi.org/10.5281/zenodo.13334951)

## BI-ADD (Bottom-up Iterative Anomalous Diffusion Detector)

> [!IMPORTANT]  
> Requirements </br>
> - TensorFlow 2.14.1[^6]
> - Python 3.10 or higher
> - latest version of [scikit-learn](https://scikit-learn.org/stable/)[^3]
> - latest version of [scikit-image](https://scikit-image.org/docs/stable/user_guide/install.html)[^4]
> - Pre-trained [models](https://drive.google.com/file/d/1WF0eW8Co23-mKQiHNH-KHHK_lJiIW-WC/view?usp=sharing)

---------------------------------------------------------------------------------------------------- </br>
<b>*** This is a preset version of <b>BI-ADD</b> for [AnDi2 Challenge](http://andi-challenge.org/challenge-2024/#andi2seminar) final-phase datasets ***</b></br>
The general version of the <b>BI-ADD</b> is being updated [here](https://github.com/JunwooParkSaribu/BI_ADD?tab=readme-ov-file).</br>
The trajectory prediction from video is performed with <b>[FreeTrace](https://github.com/JunwooParkSaribu/FreeTrace)</b></br>
---------------------------------------------------------------------------------------------------- </br>

<b>BI-ADD</b> detects changepoints at single molecular trajectory level which follows fBm with two properties, Anomalous exponent(alpha) and Generalized diffusion coefficient(K), on different scenarios.</br>
For the details of used data and scenarios, please check Andi2 Challenge[^1][^2].

<h2>Visualized examples of changepoint detection from molecular trajectories</h2>
<table border="0"> 
        <tr> 
            <td>QTM</td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/qtm_0.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/qtm_1.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/qtm_2.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/qtm_3.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/qtm_4.gif" width="128" height="128"></td> 
        </tr> 
        <tr> 
            <td>TCM</td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/tcm_0.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/tcm_1.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/tcm_2.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/tcm_3.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/tcm_4.gif" width="128" height="128"></td> 
        </tr>
        <tr> 
            <td>DIM</td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/dim_0.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/dim_1.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/dim_2.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/dim_3.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/dim_4.gif" width="128" height="128"></td> 
        </tr>
        <tr> 
            <td>MSM</td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/msm_0.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/msm_1.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/msm_2.gif" width="128" height="128"></td> 
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/msm_3.gif" width="128" height="128"></td>
            <td><img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/msm_4.gif" width="128" height="128"></td> 
        </tr>  
</table>
<img src="https://github.com/JunwooParkSaribu/AnDi2_SU_FIONA/blob/main/tmps/imgs/foot.png" width="164" height="52"></br>


<h3> Brief description of the method </h3>

At first, <b>BI-ADD</b> builds prior distribution of alpha, K by splitting a trajectory into a subset of sub-trajectories using multiple sliding windows. These multiple sliding windows generate a signal suggesting potential changepoints. Each sub-trajectory contains two potential changepoints at the extremities, <b>BI-ADD</b> estimates alpha and K for each sub trajectory. Then, it decides iteratively the potential changepoints between two sub-trajectories whether they are true positives or false positives with a priori clustered distribution with GMM[^3]. The threshold of the signal generated by multiple sliding windows controls the number of false negative changepoints, and also determines the computational time of the algorithm. </br></br>
<b>BI-ADD</b> has three major parameters to tune the quality of result. </br>
<b>(1) *Threshold of the signal*</b> (Default: 0.15) </br>
<b>(2) *Sizes of sliding windows*</b> (Default: 20 to 40) </br>
<b>(3) *Number of states*</b> in the prior distribution. (Default: 2 or 3) </br></br>
(1) acts as an FN controller. low threshold decrease FN, but increase computational time. (2) is related to the transition probability between states of trajectories. When the transition events occur frequently, it needs to use small sizes of sliding windows instead of large windows. Unnecessary small sizes of sliding windows aren't helpful to increase the quality, it rather only increases the computational time. (3) is related to the hidden number of states of molecular trajectories. The default values of (1) and (2) are enough in general cases, however (3) need to be carefully chosen since it may produce many FP if we select too high number of states. To help choosing the number of sates for (3), we generate a plot of prior distribution after building it. then, we can select its number (2 or 3 in general cases). The estimations of alpha and K for each sub-trajectory are done with ConvLSTM[^5](3 features) and Dense[^6](1 feature) layers respectively.

</br>

<h3> To remake results on AnDi2 final-phase datasets </h3>

1. Clone the repository on your local device.</br>
2. Download pre-trained [*models*](https://drive.google.com/file/d/1WF0eW8Co23-mKQiHNH-KHHK_lJiIW-WC/view?usp=sharing), place the *models* folder inside of *AnDi2_SU_FIONA* folder.</br>
3. Run *run_process.py* script with python.</br>
4. Results will be made in the *result_final_0* folder.
* result_filnal_submission.zip contains the result of the AnDi2 final-phase.

<h3> Contacts </h3>
junwoo.park@sorbonne-universite.fr<br>

<h3> References </h3>

[^1]: [AnDi datasets](https://doi.org/10.5281/zenodo.10259556)
[^2]: Quantitative evaluation of methods to analyze motion changes in single-particle experiments, Gorka Muñoz-Gil et al. 2024 [https://arxiv.org/abs/2311.18100](https://arxiv.org/abs/2311.18100)
[^3]: Scikit-learn, Machine Learning in Python, Pedregosa et al., JMLR 12, pp. 2825-2830, 2011.
[^4]: Scikit-image, Image processing in Python. PeerJ 2:e453 (2014) [https://doi.org/10.7717/peerj.453](https://doi.org/10.7717/peerj.453)
[^5]: Convolutional LSTM Network, A Machine Learning Approach for Precipitation Nowcasting, Xingjian Shi et al. 2015 [https://arxiv.org/abs/1506.04214](https://arxiv.org/abs/1506.04214)
[^6]: TensorFlow, Large-scale machine learning on heterogeneous systems, 2015
