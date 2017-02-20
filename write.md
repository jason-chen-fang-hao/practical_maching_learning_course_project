<html>

<head>
<meta http-equiv="Content-Language" content="en-us">
<meta name="GENERATOR" content="Microsoft FrontPage 5.0">
<meta name="ProgId" content="FrontPage.Editor.Document">
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
<title>Classifying quality of exercise using monitor data</title>
<style>
<!--
code{white-space: pre;}-->
</style>
</head>

<body>



<h1>Coursera Practical Machine Learning Course Project</h1>
<p><b>Name: Jason</b></p>
<p class="MsoListParagraph" style="text-indent: -18.0pt; line-height: normal; margin-left: 18.0pt">
<b>
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
Course Project Background:</span></b></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible 
to collect a large amount of data about personal activity relatively 
inexpensively. These type of devices are part of the quantified self movement – 
a group of enthusiasts who take measurements about themselves regularly to 
improve their health, to find patterns in their behavior, or because they are 
tech geeks. One thing that people regularly do is quantify how much of a 
particular activity they do, but they rarely quantify how well they do it. In 
this project, your goal will be to use data from accelerometers on the belt, 
forearm, arm, and dumbell of 6 participants. They were asked to perform barbell 
lifts correctly and incorrectly in 5 different ways. More information is 
available from the website here: http://groupware.les.inf.puc-rio.br/har (see 
the section on the Weight Lifting Exercise Dataset).</span></p>
<p class="MsoNormal" style="line-height: normal"><b>
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
&nbsp;</span></b></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
The training data for this project are available here:</span></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv</span></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
&nbsp;</span></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
The test data are available here:</span></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv</span></p>
<p class="MsoNormal" style="line-height: normal">
<span lang="EN-SG" style="font-size: 12.0pt; font-family: 'Times New Roman',serif">
&nbsp;</span></p>
<p>
<span lang="EN-SG" style="font-size: 12.0pt; line-height: 115%; font-family: 'Times New Roman',serif">
The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har. 
If you use the document you create for this class for any purpose please cite 
them as they have been very generous in allowing their data to be used for this 
kind of assignment</span></p>
<p>&nbsp;</p>
<p><b>Project Goal</b></p>
<p>The goal of this project is to create a machine-learning algorithm that can correctly identify the quality of barbell bicep curls by using data from belt, forearm, arm, and dumbbell monitors. There are five classifications of this exercise, one method is the correct form of the exercise while the other four are common mistakes: exactly according to the specification (Class A), throwing the elbows to the front (Class B), lifting the dumbbell only halfway (Class C), lowering the dumbbell only halfway (Class D) and throwing the hips to the front (Class E).</p>
<p><a href="http://groupware.les.inf.puc-rio.br/har#ixzz3PO5pnm1R">http://groupware.les.inf.puc-rio.br/har#ixzz3PO5pnm1R</a></p>
<div id="loading-the-data" class="section level2">
<h2><font size="3">Loading the Data</font></h2>
<p>The first step is to load the data.</p>
<pre class="r"><code class="r"> <span class="identifier">setwd</span><span class="paren">(</span><span class="string">"~/Desktop"</span><span class="paren">)</span>
<span class="identifier">training</span><span class="operator">&lt;-</span><span class="identifier">read.table</span><span class="paren">(</span><span class="string">"./pml-training.csv"</span>, <span class="identifier">header</span><span class="operator">=</span><span class="literal">TRUE</span>, <span class="identifier">sep</span><span class="operator">=</span><span class="string">","</span><span class="paren">)</span>
<span class="identifier">testing</span><span class="operator">&lt;-</span><span class="identifier">read.table</span><span class="paren">(</span><span class="string">"./pml-testing.csv"</span>,<span class="identifier">header</span><span class="operator">=</span><span class="literal">TRUE</span>, <span class="identifier">sep</span><span class="operator">=</span><span class="string">","</span><span class="paren">)</span></code></pre>
<p>And to install the packages that will be used: caret &amp; rattle.</p>
<pre class="r"><code class="r"><span class="keyword">library</span><span class="paren">(</span><span class="identifier">caret</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Warning: package 'caret' was built under R version 3.1.2</code></pre>
<pre><code style="white-space: pre">## Loading required package: lattice
## Loading required package: ggplot2</code></pre>
<pre class="r"><code class="r"><span class="keyword">library</span><span class="paren">(</span><span class="identifier">rattle</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Warning: package 'rattle' was built under R version 3.1.2</code></pre>
<pre><code style="white-space: pre">## Rattle: A free graphical interface for data mining with R.
## Version 3.4.1 Copyright (c) 2006-2014 Togaware Pty Ltd.
## Type 'rattle()' to shake, rattle, and roll your data.</code></pre>
<pre class="r"><code class="r"><span class="keyword">library</span><span class="paren">(</span><span class="identifier">gridExtra</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Loading required package: grid</code></pre>
</div>
<div id="cleaning-the-data-sets" class="section level2">
<h2><font size="3">Cleaning the data sets</font></h2>
<p>The data provided has many variables with missing data as well as information that is not relevant to the question being analyzed. Relevant variables are extracted using pattern recognition for relevant strings, leaving 52 variables.</p>
<div id="cleaning-up-training-data-set" class="section level3">
<h3><font size="3">Cleaning up Training Data Set</font></h3>
<pre class="r"><code class="r"><span class="identifier">trainingaccel</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^accel"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">trainingtotal</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^total"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">roll</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^roll"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">pitch</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^pitch"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">yaw</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^yaw"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">magnet</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^magnet"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">gyro</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^gyro"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">training</span><span class="paren">))</span>
<span class="identifier">acceldata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span> ,<span class="identifier">trainingaccel</span><span class="paren">]</span>
<span class="identifier">rolldata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span> ,<span class="identifier">roll</span><span class="paren">]</span>
<span class="identifier">pitchdata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span> ,<span class="identifier">pitch</span><span class="paren">]</span>
<span class="identifier">yawdata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span>,<span class="identifier">yaw</span><span class="paren">]</span>
<span class="identifier">magnetdata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span>,<span class="identifier">magnet</span><span class="paren">]</span>
<span class="identifier">gyrodata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span>,<span class="identifier">gyro</span><span class="paren">]</span>
<span class="identifier">totaldata</span><span class="operator">&lt;-</span><span class="identifier">training</span><span class="paren">[</span>,<span class="identifier">trainingtotal</span><span class="paren">]</span>
<span class="identifier">trainClasse</span><span class="operator">&lt;-</span><span class="identifier">cbind</span><span class="paren">(</span><span class="identifier">acceldata</span>,<span class="identifier">rolldata</span>,<span class="identifier">pitchdata</span>,<span class="identifier">yawdata</span>,<span class="identifier">magnetdata</span>,<span class="identifier">gyrodata</span>,<span class="identifier">totaldata</span>,<span class="identifier">training</span><span class="paren">[</span> ,<span class="number">160</span><span class="paren">])</span>
<span class="identifier">colnames</span><span class="paren">(</span><span class="identifier">trainClasse</span><span class="paren">)[</span><span class="number">53</span><span class="paren">]</span><span class="operator">&lt;-</span><span class="string">'Classe'</span></code></pre>
</div>
<div id="cleaning-up-testing-data-set" class="section level3">
<h3><font size="3">Cleaning up Testing Data Set</font></h3>
<pre class="r"><code class="r"><span class="identifier">testingaccel</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^accel"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">testingtotal</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^total"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">troll</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^roll"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">tpitch</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^pitch"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">tyaw</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^yaw"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">tmagnet</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^magnet"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">tgyro</span><span class="operator">&lt;-</span><span class="identifier">grepl</span><span class="paren">(</span><span class="string">"^gyro"</span>,<span class="identifier">names</span><span class="paren">(</span><span class="identifier">testing</span><span class="paren">))</span>
<span class="identifier">tacceldata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span> ,<span class="identifier">testingaccel</span><span class="paren">]</span>
<span class="identifier">trolldata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span> ,<span class="identifier">troll</span><span class="paren">]</span>
<span class="identifier">tpitchdata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span>,<span class="identifier">tpitch</span><span class="paren">]</span>
<span class="identifier">tyawdata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span>,<span class="identifier">tyaw</span><span class="paren">]</span>
<span class="identifier">tmagnetdata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span>,<span class="identifier">tmagnet</span><span class="paren">]</span>
<span class="identifier">tgyrodata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span>,<span class="identifier">tgyro</span><span class="paren">]</span>
<span class="identifier">ttotaldata</span><span class="operator">&lt;-</span><span class="identifier">testing</span><span class="paren">[</span>,<span class="identifier">testingtotal</span><span class="paren">]</span>
<span class="identifier">testClasse</span><span class="operator">&lt;-</span><span class="identifier">cbind</span><span class="paren">(</span><span class="identifier">tacceldata</span>,<span class="identifier">trolldata</span>,<span class="identifier">tpitchdata</span>,<span class="identifier">tyawdata</span>,<span class="identifier">tmagnetdata</span>,<span class="identifier">tgyrodata</span>,<span class="identifier">ttotaldata</span>,<span class="identifier">testing</span><span class="paren">[</span> ,<span class="number">160</span><span class="paren">])</span>
<span class="identifier">colnames</span><span class="paren">(</span><span class="identifier">testClasse</span><span class="paren">)[</span><span class="number">53</span><span class="paren">]</span><span class="operator">&lt;-</span><span class="string">'problem.id'</span></code></pre>
</div>
</div>
<div id="making-training-testing-subset" class="section level2">
<h2><font size="3">Making Training &amp; Testing Subset</font></h2>
<p>There are 19,622 observations in the training set, so in order to reduce time and to be able to perform cross-validation, a training subset is created with 60% of the original training data set to be used for training and the remaining 40% to be used as the testing set (before final testing is performed).</p>
<pre class="r"><code class="r"><span class="identifier">set.seed</span><span class="paren">(</span><span class="number">400</span><span class="paren">)</span>
<span class="identifier">inTrain</span> <span class="operator">=</span> <span class="identifier">createDataPartition</span><span class="paren">(</span><span class="identifier">trainClasse</span><span class="operator">$</span><span class="identifier">Classe</span>, <span class="identifier">p</span> <span class="operator">=</span> <span class="number">.60</span><span class="paren">)[[</span><span class="number">1</span><span class="paren">]]</span>
<span class="identifier">trainingsubset</span> <span class="operator">=</span> <span class="identifier">trainClasse</span><span class="paren">[</span> <span class="identifier">inTrain</span>,<span class="paren">]</span>
<span class="identifier">testingsubset</span> <span class="operator">=</span> <span class="identifier">trainClasse</span><span class="paren">[</span><span class="operator">-</span><span class="identifier">inTrain</span>,<span class="paren">]</span></code></pre>
</div>
</div>
<div id="rpart-model" class="section level1">
<h1><font size="3">rpart Model</font></h1>
<p>As the outcomes are categorical (nominal), a decision tree was the first model tested using the method rpart.</p>
<pre class="r"><code class="r"><span class="identifier">set.seed</span><span class="paren">(</span><span class="number">400</span><span class="paren">)</span>
<span class="identifier">modFit</span><span class="operator">&lt;-</span><span class="identifier">train</span><span class="paren">(</span><span class="identifier">Classe</span><span class="operator">~</span>.,<span class="identifier">method</span><span class="operator">=</span><span class="string">"rpart"</span>, <span class="identifier">data</span><span class="operator">=</span><span class="identifier">trainingsubset</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Loading required package: rpart</code></pre>
<pre class="r"><code class="r"><span class="identifier">print</span><span class="paren">(</span><span class="identifier">modFit</span><span class="operator">$</span><span class="identifier">finalModel</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## n= 11776 
## 
## node), split, n, loss, yval, (yprob)
##       * denotes terminal node
## 
##   1) root 11776 8428 A (0.28 0.19 0.17 0.16 0.18)  
##     2) roll_belt&lt; 130.5 10776 7437 A (0.31 0.21 0.19 0.18 0.11)  
##       4) pitch_forearm&lt; -33.95 944    5 A (0.99 0.0053 0 0 0) *
##       5) pitch_forearm&gt;=-33.95 9832 7432 A (0.24 0.23 0.21 0.2 0.12)  
##        10) yaw_belt&gt;=169.5 507   55 A (0.89 0.049 0 0.053 0.0059) *
##        11) yaw_belt&lt; 169.5 9325 7076 B (0.21 0.24 0.22 0.2 0.13)  
##          22) magnet_dumbbell_z&lt; -88.5 1218  519 A (0.57 0.28 0.047 0.072 0.026) *
##          23) magnet_dumbbell_z&gt;=-88.5 8107 6110 C (0.15 0.24 0.25 0.22 0.14)  
##            46) pitch_belt&lt; -42.95 496   83 B (0.014 0.83 0.1 0.022 0.028) *
##            47) pitch_belt&gt;=-42.95 7611 5665 C (0.16 0.2 0.26 0.24 0.15)  
##              94) magnet_dumbbell_x&gt;=-446.5 3261 2281 B (0.17 0.3 0.094 0.24 0.2)  
##               188) roll_belt&lt; 117.5 2049 1175 B (0.17 0.43 0.024 0.13 0.25) *
##               189) roll_belt&gt;=117.5 1212  699 D (0.17 0.087 0.21 0.42 0.11) *
##              95) magnet_dumbbell_x&lt; -446.5 4350 2710 C (0.16 0.12 0.38 0.23 0.11) *
##     3) roll_belt&gt;=130.5 1000    9 E (0.009 0 0 0 0.99) *</code></pre>
<pre class="r"><code class="r"><span class="identifier">fancyRpartPlot</span><span class="paren">(</span><span class="identifier">modFit</span><span class="operator">$</span><span class="identifier">finalModel</span>,<span class="identifier">cex</span><span class="operator">=</span><span class="number">.5</span>,<span class="identifier">under.cex</span><span class="operator">=</span><span class="number">1</span>,<span class="identifier">shadow.offset</span><span class="operator">=</span><span class="number">0</span><span class="paren">)</span></code></pre>
<p><img border="0" src="fig%201.png" width="634" height="451"></p>
<pre class="r"><code class="r"><span class="identifier">classepredict</span><span class="operator">=</span><span class="identifier">predict</span><span class="paren">(</span><span class="identifier">modFit</span>,<span class="identifier">testingsubset</span><span class="paren">)</span>
<span class="identifier">confusionMatrix</span><span class="paren">(</span><span class="identifier">testingsubset</span><span class="operator">$</span><span class="identifier">Classe</span>,<span class="identifier">classepredict</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1349  233  479  166    5
##          B  247  864  337   70    0
##          C   41   55 1078  194    0
##          D   72  183  679  352    0
##          E   19  355  360   68  640
## 
## Overall Statistics
##                                           
##                Accuracy : 0.5459          
##                  95% CI : (0.5348, 0.5569)
##     No Information Rate : 0.3738          
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16       
##                                           
##                   Kappa : 0.4307          
##  Mcnemar's Test P-Value : &lt; 2.2e-16       
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.7807   0.5112   0.3675  0.41412  0.99225
## Specificity            0.8557   0.8938   0.9410  0.86650  0.88863
## Pos Pred Value         0.6044   0.5692   0.7880  0.27372  0.44383
## Neg Pred Value         0.9325   0.8695   0.7136  0.92409  0.99922
## Prevalence             0.2202   0.2154   0.3738  0.10834  0.08221
## Detection Rate         0.1719   0.1101   0.1374  0.04486  0.08157
## Detection Prevalence   0.2845   0.1935   0.1744  0.16391  0.18379
## Balanced Accuracy      0.8182   0.7025   0.6543  0.64031  0.94044</code></pre>
<p>The outcomes are not as definitive as one would hope in viewing the plot. In testing this model on the testing subset, it is revealed to have a 54.6% accuracy, which is only slightly better than chance. The variables used in the algorithm include roll_belt, pitch_forearm, yaw_belt,magnet_dumbbell_Z,pitch_belt, and magnet_dumbell_x. The model is the least accurate for outcome D.</p>
<div id="random-forest-model" class="section level2">
<h2><font size="3">Random Forest Model</font></h2>
<p>As the rpart model was largely inaccurate and the outcome variable appears to have more nuances in variable selection as demonstrated in the rpart tree, a random forest model was tested to see if that method fit the data more appropriately.</p>
<pre class="r"><code class="r"><span class="identifier">set.seed</span><span class="paren">(</span><span class="number">400</span><span class="paren">)</span>
<span class="identifier">modFit2</span> <span class="operator">&lt;-</span> <span class="identifier">train</span><span class="paren">(</span><span class="identifier">Classe</span> <span class="operator">~</span> ., <span class="identifier">method</span><span class="operator">=</span><span class="string">"rf"</span>,<span class="identifier">trControl</span><span class="operator">=</span><span class="identifier">trainControl</span><span class="paren">(</span><span class="identifier">method</span> <span class="operator">=</span> <span class="string">"cv"</span>, <span class="identifier">number</span> <span class="operator">=</span> <span class="number">4</span><span class="paren">)</span>, <span class="identifier">data</span><span class="operator">=</span><span class="identifier">trainingsubset</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Loading required package: randomForest
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.</code></pre>
<pre class="r"><code class="r"><span class="identifier">print</span><span class="paren">(</span><span class="identifier">modFit2</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Random Forest 
## 
## 11776 samples
##    52 predictor
##     5 classes: 'A', 'B', 'C', 'D', 'E' 
## 
## No pre-processing
## Resampling: Cross-Validated (4 fold) 
## 
## Summary of sample sizes: 8832, 8830, 8833, 8833 
## 
## Resampling results across tuning parameters:
## 
##   mtry  Accuracy   Kappa      Accuracy SD  Kappa SD   
##    2    0.9869232  0.9834568  0.004227321  0.005347632
##   27    0.9879419  0.9847469  0.005465882  0.006913808
##   52    0.9830171  0.9785171  0.004211651  0.005324614
## 
## Accuracy was used to select the optimal model using  the largest value.
## The final value used for the model was mtry = 27.</code></pre>
<pre class="r"><code class="r"><span class="identifier">varImp</span><span class="paren">(</span><span class="identifier">modFit2</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## rf variable importance
## 
##   only 20 most important variables shown (out of 52)
## 
##                      Overall
## roll_belt             100.00
## pitch_forearm          57.84
## yaw_belt               56.46
## pitch_belt             44.42
## magnet_dumbbell_z      44.09
## magnet_dumbbell_y      41.96
## roll_forearm           38.77
## accel_dumbbell_y       20.08
## magnet_dumbbell_x      17.97
## accel_forearm_x        17.93
## roll_dumbbell          17.47
## magnet_belt_z          16.24
## accel_belt_z           13.62
## magnet_forearm_z       13.04
## accel_dumbbell_z       12.84
## total_accel_dumbbell   12.49
## magnet_belt_y          11.31
## yaw_arm                11.23
## gyros_belt_z           10.71
## magnet_belt_x          10.63</code></pre>
<pre class="r"><code class="r"><span class="identifier">classepredict2</span><span class="operator">=</span><span class="identifier">predict</span><span class="paren">(</span><span class="identifier">modFit2</span>,<span class="identifier">testingsubset</span><span class="paren">)</span>
<span class="identifier">confusionMatrix</span><span class="paren">(</span><span class="identifier">testingsubset</span><span class="operator">$</span><span class="identifier">Classe</span>,<span class="identifier">classepredict2</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2231    0    0    0    1
##          B    8 1500   10    0    0
##          C    0   15 1349    4    0
##          D    0    1   17 1268    0
##          E    0    0    4    1 1437
## 
## Overall Statistics
##                                        
##                Accuracy : 0.9922       
##                  95% CI : (0.99, 0.994)
##     No Information Rate : 0.2854       
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16    
##                                        
##                   Kappa : 0.9902       
##  Mcnemar's Test P-Value : NA           
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9964   0.9894   0.9775   0.9961   0.9993
## Specificity            0.9998   0.9972   0.9971   0.9973   0.9992
## Pos Pred Value         0.9996   0.9881   0.9861   0.9860   0.9965
## Neg Pred Value         0.9986   0.9975   0.9952   0.9992   0.9998
## Prevalence             0.2854   0.1932   0.1759   0.1622   0.1833
## Detection Rate         0.2843   0.1912   0.1719   0.1616   0.1832
## Detection Prevalence   0.2845   0.1935   0.1744   0.1639   0.1838
## Balanced Accuracy      0.9981   0.9933   0.9873   0.9967   0.9993</code></pre>
<p>The random forest model has a 99.2% accuracy, far superior to the rpart method. The specificity and sensitivity is in the high 90s for all variables. The top five variables of importance included the roll_belt, yaw_belt,magnet_dumbbell_z,magnet_dumbbell_y, and the pitch_forearm. For outcome C, the model is the least accurate.Preprocessing was considered, but at the risk of overfitting the model was not tested due to the accuracy already being over 99%.</p>
<p>Below are a few examples of how the data is more intricate than a discrete rpart model allow for, as it would require many yes/no statements to find all the different variations of each outcome.</p>
<pre class="r"><code class="r"><span class="identifier">p1</span><span class="operator">&lt;-</span><span class="identifier">qplot</span><span class="paren">(</span><span class="identifier">roll_belt</span>,<span class="identifier">yaw_belt</span>,<span class="identifier">colour</span><span class="operator">=</span><span class="identifier">Classe</span>,<span class="identifier">data</span><span class="operator">=</span><span class="identifier">trainingsubset</span><span class="paren">)</span>
<span class="identifier">p2</span><span class="operator">&lt;-</span><span class="identifier">qplot</span><span class="paren">(</span><span class="identifier">roll_belt</span>,<span class="identifier">pitch_forearm</span>,<span class="identifier">colour</span><span class="operator">=</span><span class="identifier">Classe</span>,<span class="identifier">data</span><span class="operator">=</span><span class="identifier">trainingsubset</span><span class="paren">)</span>
<span class="identifier">grid.arrange</span><span class="paren">(</span><span class="identifier">p1</span>,<span class="identifier">p2</span>,<span class="identifier">ncol</span><span class="operator">=</span><span class="number">2</span><span class="paren">)</span></code></pre>
<p><img border="0" src="fig%202.png" width="822" height="590"></p>
<pre class="r"><code class="r"><span class="identifier">dev.off</span><span class="paren">()</span></code></pre>
<pre><code style="white-space: pre">## null device 
##           1</code></pre>
</div>
<div id="in-sample-out-of-sample-error" class="section level2">
<h2><font size="3">In Sample &amp; Out of Sample Error</font></h2>
<p>The in sample error is error rate when the model is used to predict the training set it is based off. This error is going to be much less than the model predicting another dataset (out of sample error). For the random forest model used as the final algorithm, the in sample error rate is 0; the model is 100% accurate. This could be a sign of overfitting.</p>
<pre class="r"><code class="r"><span class="identifier">insamplepredict</span><span class="operator">=</span><span class="identifier">predict</span><span class="paren">(</span><span class="identifier">modFit2</span>,<span class="identifier">trainingsubset</span><span class="paren">)</span>
<span class="identifier">confusionMatrix</span><span class="paren">(</span><span class="identifier">trainingsubset</span><span class="operator">$</span><span class="identifier">Classe</span>,<span class="identifier">insamplepredict</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 3348    0    0    0    0
##          B    0 2279    0    0    0
##          C    0    0 2054    0    0
##          D    0    0    0 1930    0
##          E    0    0    0    0 2165
## 
## Overall Statistics
##                                      
##                Accuracy : 1          
##                  95% CI : (0.9997, 1)
##     No Information Rate : 0.2843     
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16  
##                                      
##                   Kappa : 1          
##  Mcnemar's Test P-Value : NA         
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            1.0000   1.0000   1.0000   1.0000   1.0000
## Specificity            1.0000   1.0000   1.0000   1.0000   1.0000
## Pos Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000
## Neg Pred Value         1.0000   1.0000   1.0000   1.0000   1.0000
## Prevalence             0.2843   0.1935   0.1744   0.1639   0.1838
## Detection Rate         0.2843   0.1935   0.1744   0.1639   0.1838
## Detection Prevalence   0.2843   0.1935   0.1744   0.1639   0.1838
## Balanced Accuracy      1.0000   1.0000   1.0000   1.0000   1.0000</code></pre>
<p>However, as shown previously, when the model is used on a separate data set the accuracy is still at 99.2%.</p>
<pre class="r"><code class="r"><span class="identifier">classepredict2</span><span class="operator">=</span><span class="identifier">predict</span><span class="paren">(</span><span class="identifier">modFit2</span>,<span class="identifier">testingsubset</span><span class="paren">)</span>
<span class="identifier">confusionMatrix</span><span class="paren">(</span><span class="identifier">testingsubset</span><span class="operator">$</span><span class="identifier">Classe</span>,<span class="identifier">classepredict2</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 2231    0    0    0    1
##          B    8 1500   10    0    0
##          C    0   15 1349    4    0
##          D    0    1   17 1268    0
##          E    0    0    4    1 1437
## 
## Overall Statistics
##                                        
##                Accuracy : 0.9922       
##                  95% CI : (0.99, 0.994)
##     No Information Rate : 0.2854       
##     P-Value [Acc &gt; NIR] : &lt; 2.2e-16    
##                                        
##                   Kappa : 0.9902       
##  Mcnemar's Test P-Value : NA           
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9964   0.9894   0.9775   0.9961   0.9993
## Specificity            0.9998   0.9972   0.9971   0.9973   0.9992
## Pos Pred Value         0.9996   0.9881   0.9861   0.9860   0.9965
## Neg Pred Value         0.9986   0.9975   0.9952   0.9992   0.9998
## Prevalence             0.2854   0.1932   0.1759   0.1622   0.1833
## Detection Rate         0.2843   0.1912   0.1719   0.1616   0.1832
## Detection Prevalence   0.2845   0.1935   0.1744   0.1639   0.1838
## Balanced Accuracy      0.9981   0.9933   0.9873   0.9967   0.9993</code></pre>
<p>And when used on the original testing data set, the submitted answer resulted in 100% “You are correct!” I am hesitant to say this is equivalent to 100% accuracy as some problems may have had several solutions marked as correct to account for various students’ algorithms.For the purposes of this course, this testing on a new set of data gives more credence that the model accounts for the signal and not just the noise.</p>
<pre class="r"><code class="r"><span class="identifier">testinganswers</span><span class="operator">=</span><span class="identifier">predict</span><span class="paren">(</span><span class="identifier">modFit2</span>, <span class="identifier">newdata</span><span class="operator">=</span><span class="identifier">testing</span><span class="paren">)</span>
<span class="identifier">print</span><span class="paren">(</span><span class="identifier">testinganswers</span><span class="paren">)</span></code></pre>
<pre><code style="white-space: pre">##  [1] B A B A A E D B A A B C B A E E A B B B
## Levels: A B C D E</code></pre>
<p>It is also important to consider that the samples are all taken from one larger sample and that if the data were to be collected again during a different time period or with different participants the out of sample error could be higher and the algorithm may not be as accurate. This is especially true when considering that though there are many observations, the data comes for 6 participants which may not be representative of the population as a whole.</p>
</div>
<div id="conclusion" class="section level2">
<h2><font size="3">Conclusion</font></h2>
<p>Random Forest was a superior model for prediction of exercise quality compared to rpart. The nominal categories were dependent on various variables and the interaction between them. The RF model had over 99% accuracy and fitted well to other subsamples of the data. However, the algorithm may not have as high of accuracy on other samples, particularly ones with different subjects.</p>
<p>In the first model D was the most difficult to predict and in the second C was the most difficult to predict. This makes theoretical sense as Class C is lifting the dumbbell only halfway and Class D is lowering the dumbbell only halfway. These movements may be hard to distinguish by the data collected and could be a topic for future research regarding how to detect this difference-if deemed important.</p>
<p>Overall, it is interesting to consider how monitors are affected by the quality of an exercise and are able to predict the error made. This is an important indicator for health and fitness as it is not just the quantity of exercise that can be collected and analyzed but also the quality.</p>
</div>
</div>


</body>

</html>
