## INTRODUCTION
This python package is designed to compute, analyze, and visualize the results of miRNA TSSs based on the signal peaks.
## SUPPORTED PLATFORMS
Linux ,MacOS
## PREREQUISITES
python 2.7.x  
Python packages dependencies:  
--numpy  
--h5py  
--pandas  
--tensorflow  
The pip install -r requirement.txt will try to install these packages automatically. However, please install them manually if, by any reason, the automatic installation fails.  
External packages dependencies:  
--bedtools  
Install instructions:[http://bedtools.readthedocs.io/en/latest/](http://bedtools.readthedocs.io/en/latest/), make sure the bedtools has already exported to PATH.  
## INSTALLATION
First, create a python virtual environment.

```
virtualenv DeepMiRTss_env
```
Second, active the virtualenv and cd into it.

```
source ./DeepMiRTss_env/bin/activate
cd ./DeepMiRTss_env
```
Third, download the software package.  

```
git clone https://github.com/jwanglabwangzhi/DeepMiRTss

```
Fouth, cd to the package directory,run requirements and setup.py script to install.

```
cd ./DeepMiRTss
pip install -r requirements.txt
python setup.py install
```
Fifth, download the genomic data and DanQ model data and put it into the pre_load folder.

hg19:[http://mcube.nju.edu.cn/jwang/member/wangzhi/hg19.7z](http://mcube.nju.edu.cn/jwang/member/wangzhi/hg19.7z) unzip it as hg19.fa  
DanQ model:[http://mcube.nju.edu.cn/jwang/member/wangzhi/DanQ_bestmodel.hdf5](http://mcube.nju.edu.cn/jwang/member/wangzhi/DanQ_bestmodel.hdf5)

## USAGE
The following 3 commands were provided by the DeepMiRTss package:  
1 DeepMiRTss.tssfinder, 2 DeepMiRTss.analysis, 3 DeepMiRTss.visulization.
### 1. DeepMiRTss.tssfinder  
This command is used to compute the miRNA TSSs according to ChIP-seq data or 5' end of cap-protected RNA-seq data.  
All data should be generated by peak-calling to generate peak files and the peak file must use the first six columns of  
the BED format as input. The BED format can be found at [UCSC geonome browser website](http://genome.ucsc.edu/FAQ/FAQformat#format1)
#### Parameters:  
<-h --help>  
<-p --peak_filename> [The signal  peak file]    
A signal file is a necessity for the whole calculation process. You can use different kinds of  peak calling tools to obtain the signal file.  
<-k --judgement_filename> [The signal file for judgement]  
Another signal file can help to improve the accuracy of prediction , but it not a necessity.  
<-e --expressed_filename> [miRNA expression file]  
We suggest providing no more than 50 miRNAs at a time which reduces the waiting time.  
<-n --number_alternative_tss>[number of alternative TSSs]    
The predicted number of alternative TSSs, and the default value is 3, you can set any positive number to obtain multiple results for the same miRNA.
#### Examples：
```
# If you only have a single signal file, you can run the programme like this in example to find TSSs.
DeepMiRTss.tssfinder -p ./example/pol2.bed 

# When you have two signal files, you can use one of them as judgement. The judgement file can help improve the accuracy like the follow example.
DeepMiRTss.tssfinder -p ./example/pol2.bed -k ./example/h3k4me3.bed

# When you have two signal files and express file, code like this help you find TSSs based on expression file.
DeepMiRTss.tssfinder -p ./example/pol2.bed -k ./example/h3k4me3.bed -e ./example/express.txt

# You can identify only one TSS for a miRNA, or identify multiple TSSs, as long as you change the -n parameter.
DeepMiRTss.tssfinder -p ./example/pol2.bed -n 1
DeepMiRTss.tssfinder -p ./example/pol2.bed -n 5
```
### 2. DeepMiRTss.analysis  
Make full use of DanQ model to calculate and analyze sequence features of region nearby TSSs.The DanQ model can refer to [``DanQ: a hybrid convolutional and recurrent neural network for predicting the function of DNA sequences''](https://academic.oup.com/nar/article-lookup/doi/10.1093/nar/gkw226)
#### Parameters:  
<-h --help>  
<-u --upstream> [The distance value]  
The distance value between TSS and analysis region,and the default value is 0. You can choose the TSS region (TSS-500, TSS+500) as the analysis region which is the default or choose other region as you like.  
#### Examples：
```
# If you analysis TSSs region (TSS-500,  TSS+500), no parameter needs to be set.
DeepMiRTss.analysis
# If the promoter region(TSS-1000, TSS) is you need, set -u as 500.
DeepMiRTss.analysis -u 500
```
### 3. DeepMiRTss.visulization  
We use JavaScript and Ajax to dynamically visualize the features of miRNA alternative TSSs. So you can find the feature value and compare different alternative TSSs at the same time.  
#### No parameter here
#### Examples：
```
DeepMiRTss.visulization
```

## FAQ  
1 ChIP-seq siganl data including pol2, DHS, and the transcription related histone such as h3k4me3, h3k4me2, etc.  
2 The basic idea of peak calling of 5' end of cap-protected RNAs data including CAGE, TSS-Seq, START-seq, PRO-Cap, GRO-cap, CAP-seq, 5'RNA-Seq, 5'GRO-Seq etc is to identify regions with a high density of RNA sequencing reads, which on the surface sounds really similar to finding peaks in ChIP-Seq data (and it is!).  
3 Any file generated by different peak-calling tools can be used as input, as long as the first six columns of the bed format are satisfied.  
4 Of course, you can use any peak-calling software, we recommend using MACS2 to call peaks for ChIP-seq data and peak-calling for 5' end of cap-protected RNAs data with HOMER.  
5 If you don't provide -e parameter as a express file in 'DeepMiRTss.tssfinder' command, the program will compute all miRNA which might be last for a long time. So we suggest providing no more than 20 miRNAs at a time which reduces the waiting time.  
6 The command 'DeepMiRTss.analysis' must base on the result of command 'DeepMiRTss.tssfinder' which generates a 'miRNA_alternative_tss.bed' file.  
7 The command 'DeepMiRTss.visulization' must base on the result of command 'DeepMiRTss.analysis' which generates a 'y.csv' file.  


## CREDITS
This software was developed by mcube bioinformatics group @ NanJing University  
Implemented by Wang Zhi.
## LICENSE
This software is under MIT license.  
See the LICENSE.txt file for details.  
## CONTACT
szxszx@foxmail.com

















