doc01
=====

https://afni.nimh.nih.gov/

https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/

https://afni.nimh.nih.gov/afni/doc/program_help/index.html

https://www.opensourceimaging.org/project/afni/

https://en.wikipedia.org/wiki/Analysis_of_Functional_NeuroImages

A package of computer programs for analysis and visualization of three-dimensional human brain functional magnetic 
resonance imaging (FMRI) results is described. The software can color overlay neural activation maps onto higher 
resolution anatomical scans. Slices in each cardinal plane can be viewed simultaneously. Manual placement of markers 
on anatomical landmarks allows transformation of anatomical and functional scans into stereotaxic (Talairach–Tournoux) 
coordinates. The techniques for automatically generating transformed functional data sets from manually labeled 
anatomical data sets are described. Facilities are provided for several types of statistical analyses of multiple 3D 
functional data sets. The programs are written in ANSI C and Motif 1.2 to run on Unix workstations.

https://andysbrainbook.readthedocs.io/en/latest/AFNI/AFNI_Short_Course/AFNI_fMRI_Intro.html

.. _AFNI_fMRI_Intro57:

==============
Introduction to AFNI
==============

------------

This course will show you how to analyze an fMRI dataset from start to finish. We will begin by **downloading a sample 
dataset** and inspecting the anatomical and functional images for each subject. We will then preprocess the data, 
which 
removes noise and enhances the signal in the images. Once the images have been preprocessed, we will create a model 
representing what we think the BOLD signal, a measure of neural activity, should look like in our images. During model 
fitting we compare this model with the signal in different areas of the image. This model fit is a measure of the 
strength of the signal under different conditions - for example, we can take the difference of the signal between 
conditions A and B of the experiment to see which condition leads to a larger BOLD response.

Once a model has been created for each subject and the BOLD response has been estimated for each condition, we can do 
any kind of group analysis we like: Paired t-tests, between-group t-tests, interactions, and so on. The goal of this 
course is to calculate a simple within-subjects contrast between two conditions, and test whether it is significant 
across subjects. You will also learn how to create figures showing whole-brain analyses, similar to what you see 
published in the neuroimaging journals, and how to do a region of interest (ROI) analysis.

.. figure:: Intro_Final_Map.png

   A figure showing group-level results from the data used in this course, represented as a z-statistic map. Brighter colors indicate larger z-scores. You will begin by preprocessing the raw data and end with creating a statistical map  
like this one.

This course is designed to build your confidence in working with fMRI data, increase your fluency with the basic terms 
of fMRI analysis, and help you make educated choices during each step. Some chapters have exercises to help you 
practice what you’ve learned and to prepare you for the next chapter. Once you have mastered the fundamentals of this 
course, you will be able to apply them to other datasets of your choosing.

Downloading and Installing AFNI
*******************************

Depending on your computer’s operating system, you have several different options for downloading and installing the 
AFNI package. The `AFNI website 
<https://afni.nimh.nih.gov/pub/dist/doc/htmldoc/background_install/install_instructs/index.html>`__  
has instructions on how to install the software on Windows, Macintosh, and different 
versions of Linux; a tutorial video on how to install AFNI on a Macintosh operating system can be found here.

For Macintosh users, an installation app has been created that will streamline the process. Once you have downloaded 
it here, simply unpack the app and follow the instructions; I recommend keeping the default boxes checked.

If you do not have any of the required packages, such as R or Homebrew, the entire installation can take 1-2 hours. 
When it has completed, open a Terminal, type ``afni``, and then press return. You should see the graphical user 
interface 
open. Close it, and from the same terminal type uber_subject.py and press return. You should see this window, which 
indicates that any Python-based programs should run without errors.

../../_images/Intro_AFNI_GUI.png

When you have successfully installed AFNI and typed afni from the command line, you should see something like this.

.. note::

This course will not be covering MRI physics in depth. For a review of that topic, I recommend chapters 1-5 of the 
book Functional Magnetic Resonance Imaging, by Huettel, Song, & McCarthy (3rd Edition). Also see Allen Elster’s 
excellent MRI Questions website for useful illustrations of MRI concepts.

Video
For a brief video introduction to AFNI and a walkthrough of how to download it, click here.


-------------------------------------

https://cbmm.mit.edu/afni

------------------------------------

A recent study posted on bioRxiv by Bowring, Maumet and Nichols aimed to compare results of FMRI data that had been 
processed with three commonly used software packages (AFNI, FSL and SPM). Their stated purpose was to use "default" 
settings of each software's pipeline for task-based FMRI, and then to quantify overlaps in final clustering results 
and to measure similarity/dissimilarity in the final outcomes of packages. While in theory the setup sounds simple 
(implement each package's defaults and compare results), practical realities make this difficult. For example, 
different softwares would recommend different spatial resolutions of the final data, but for the sake of comparisons, 
the same value must be used across all. Moreover, we would say that AFNI does not have an explicit default pipeline 
available: a wide diversity of datasets and study designs are acquired across the neuroimaging community, often 
requiring bespoke tailoring of basic processing rather than a "one-size-fits-all" pipeline. However, we do have strong 
recommendations for certain steps, and we are also aware that the choice of a given step might place requirements on 
other processing steps. Given the very clear reporting of the AFNI pipeline used in Bowring et al. paper, we take this 
opportunity to comment on some of these aspects of processing with AFNI here, clarifying a few mistakes therein and 
also offering recommendations. We provide point-by-point considerations of using AFNI's processing pipeline design 
tool at the individual level, afni_proc.py, along with supplementary programs; while specifically discussed in the 
context of the present usage, many of these choices may serve as useful starting points for broader processing. It is 
our intention/hope that the user should examine data quality at every step, and we demonstrate how this is facilitated 
in AFNI, as well.

 ------------------------------------

https://web.conn-toolbox.org/

------------------------------------

https://cloud.wikis.utexas.edu/wiki/spaces/~mvf327/pages/70001716/Intro+to+AFNI+basics

------------------------------------

https://andysbrainbook.readthedocs.io/en/latest/AFNI/AFNI_Short_Course/AFNI_05_1stLevelAnalysis.html

https://andysbrainbook.readthedocs.io/en/latest/OpenScience/OS/fMRIPrep_Demo_5_1stLevelAnalysis.html

https://andysbrainblog.blogspot.com/2012/09/afni-part-1-introduction.html

https://canlab.github.io/_pages/tutorials/html/first_level_spm12.html

------------------------------------

Data sharing is becoming a priority in functional Magnetic Resonance Imaging (fMRI) research, but the lack of a 
standard format for shared data is an obstacle (Poline et al., 2012; Poldrack and Gorgolewski, 2014). This is 
especially true for information about data provenance, including auxiliary information such as participant 
characteristics and task descriptions. The three most commonly used analysis software packages [AFNI 1 (Cox, 1996), 
FSL 2 (Jenkinson et al., 2012), and SPM 3 (Penny et al., 2011)] broadly conduct the same analysis, but diﬀer in how 
fundamental concepts are described, and have a myriad of diﬀerences in the pre-processing and modeling steps. The 
practical consequence is that sharing analyzed data is further complicated by the idiosyncrasies of the particular 
software used.

The Neuroimaging Data Model [NIDM 4 (Keator et al., 2013; Maumet et al., 2016)] is an initiative from the 
International Neuroinformatics Coordinating Facility (INCF 5 ) that addresses these practical barriers through the 
development of a standard format for neuroimaging data. Ultimately, NIDM will provide a standard format that can 
handle data that has been processed in any of the common software packages. In order to achieve this, the development 
of NIDM requires publicly available derived data that covers all the major use cases in the main software programs.

The purpose of the current work was to produce a set of results of mass univariate fMRI analyses using the most common 
software packages: AFNI, FSL, and SPM [which between them cover 80% of published fMRI analyses (Carp, 2012)], 
utilizing publicly available data from OpenfMRI6  (Poldrack et al., 2013). The analyses (‘variants’) presented in this 
paper cover the most common options available in each software package at each analysis stage, from diﬀerent 
Hemodynamic response function (HRF) basis functions through to group-level tests. The tests are arranged so that 
readers can compare the closest equivalent variants across software packages. In particular, these tests will be 
useful for comparing the results from default test settings across software packages.

While this collection of analyses was chosen for their relevance to the NIDM project, it also addresses a gap in the 
literature where publicly available processed data is concerned. Speciﬁcally, while there are published comparisons 
of diﬀerent processing pipelines, the data are not publicly available (Carp, 2012) or are for resting state fMRI only 
(Bellec et al., 2016). Others have shared raw data but lack analysis results (Hanke et al., 2014) or do not include
comparisons across multiple software packages [e.g., analyses in the The Human Connectome Project 7 (Van Essen et al., 
2013) are performed with FSL only]. Shared raw data is a useful resource, but we argue that shared processed data is 
also important, both to provide a basis of cross-software comparisons and to create a benchmark for testing of 
automated provenance software. The dataset presented in this paper is a contribution toward this omission in the 
literature.


 ------------------------------------

Quality control (QC) assessment is a vital part of FMRI processing and analysis, and a typically underdiscussed aspect 
of reproducibility. This includes checking datasets at their very earliest stages (acquisition and conversion) through 
their processing steps (e.g., alignment and motion correction) to regression modeling (correct stimuli, no 
collinearity, valid fits, enough degrees of freedom, etc.) for each subject. There are a wide variety of features to 
verify throughout any singlesubject processing pipeline, both quantitatively and qualitatively. We present several 
FMRI preprocessing QC features available in the AFNI toolbox, many of which are automatically generated by the 
pipeline-creation tool, afni_proc.py. These items include a modular HTML document that covers full single-subject 
processing from the raw data through statistical modeling, several review scripts in the results directory of 
processed data, and command line tools for identifying subjects with one or more quantitative properties across a 
group (such as triaging warnings, making exclusion criteria, or creating informational tables). The HTML itself 
contains several buttons that efficiently facilitate interactive investigations into the data, when deeper checks are 
needed beyond the systematic images. The pages are linkable, so that users can evaluate individual items across a 
group, for increased sensitivity to differences (e.g., in alignment or regression modeling images). Finally, the QC 
document contains rating buttons for each “QC block,” as well as comment fields for each, to facilitate both saving 
and sharing the evaluations. This increases the specificity of QC, as well as its shareability, as these files can be 
shared with others and potentially uploaded into repositories, promoting transparency and open science. We describe 
the features and applications of these QC tools for FMRI.

 ------------------------------------

Functional magnetic resonance imaging (fMRI) has become the most popular method for imaging of brain functions. 
Currently, there is a large variety of software packages for the analysis of fMRI data, each providing many features 
for users. Since there is no single package that can provide all the necessary analyses for the fMRI data, it is 
helpful to know the features of each software package. In this paper, several software tools have been introduced and 
they have been evaluated for comparison of their functionality and their features. The description of each program has 
been discussed and summarized.


Processing, evaluating, and understanding FMRI data with afni_proc.py
------------------------------------


FMRI data are noisy, complicated to acquire, and typically go through many steps of processing before they are used in 
a study or clinical practice. Being able to visualize and understand the data from the start through the completion of 
processing, while being confident that each intermediate step was successful, is challenging. AFNI’s afni_proc.py is a 
tool to create and run a processing pipeline for FMRI data. With its flexible features, afni_proc.py allows users to 
both control and evaluate their processing at a detailed level. It has been designed to keep users informed about all 
processing steps: it does not just process the data, but also first outputs a fully commented processing script that 
the users can read, query, interpret, and refer back to. Having this full provenance is important for being able to 
understand each step of processing; it also promotes transparency and reproducibility by keeping the record of 
individual-level processing and modeling specifics in a single, shareable place. Additionally, afni_proc.py creates 
pipelines that contain several automatic self-checks for potential problems during runtime. The output directory 
contains a dictionary of relevant quantities that can be programmatically queried for potential issues and a 
systematic, interactive quality control (QC) HTML. All of these features help users evaluate and understand their data 
and processing in detail. We describe these and other aspects of afni_proc.py here using a set of task-based and 
resting-state FMRI example commands.

 ------------------------------------

------------------------------------

------------------------------------

------------------------------------

------------------------------------

------------------------------------


