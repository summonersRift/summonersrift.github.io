---
layout: post
title: Machine Learning Part-1
published: true
---

<p class="intro">Machine Learning is cool because for the first time we can create something that can mimic human activities, intelligence. Just like children learn anything really fast, similarly machine learning is advancing at a rapid pace now-a-days. Commercial interest is machine learning is driving the aggresively driving the innovation on many frontier now-a-days that are solving real-world problems. This is just the beginning of artificial intelligence in general. The future in 50 years will be exciting where humans will be enhanced with artificially intelligent chips and machines will start to co-exist. Long way for us to get there. But we must start somewhere.</p>
Steps of using machine learning:
1. Retrieve data.
1. Clean data.
1. Identify features or label data.
1. Split dataset into Test/Cross-Validation/ Test dataset
1. Train your algorithm using supervised or unsupervised learning on <i>test</i> data.
1. Adjust model parameters using </i>cross-validation</i>
1. Test on the test data.
1. Deploy.
1. Measure the accuracy or error.

#### Useful References
1. [41 Essential Machine Learning Interview Questions &amp; answers by Roger Huang](https://www.springboard.com/blog/machine-learning-interview-questions/).
1. [A great Glossary from AnalyticsVidya](https://www.analyticsvidhya.com/glossary-of-common-statistics-and-machine-learning-terms/). They have so many great intro posts.
1. [A Good intro writeup](https://github.com/ShuaiW/ml-interview).
1. [Slides on ML from a course taught on edx by Columbia](https://courses.edx.org/asset-v1:ColumbiaX+CSMM.101x+1T2017+type@asset+block@AI_edx_ml_5.1intro.pdf).


<table>
  <tr>
    <th rowspan="10"><span style="font-weight:bold">Supervised Learning</span></th>
  </tr>
  <tr>
    <td>Linear Regression</td>
  </tr>
  <tr>
    <td>Nonlinear multivariate Regression</td>
  </tr>
  <tr>
    <td>Logictic Regression</td>
  </tr>
  <tr>
    <td>Decision Trees</td>
  </tr>
  <tr>
    <td>Support Vector Machines(SVMs) </td>
  </tr>
  <tr>
    <td>Random Forests</td>
  </tr>
  <tr>
    <td>KNN, K-Nearest Neighbors</td>
  </tr>
  <tr>
    <td>Neural Networks</td>
  </tr>
  <tr>
    <td></td>
  </tr>
  <tr>
    <td></td>
  </tr>
  <tr>
    <td rowspan="4"><span style="font-weight:bold">Unsupervised Learning</span></td>
    <td>Clustering: K-means</td>
  </tr>
  <tr>
    <td>Hierarchical Cluster Analysis (HCA)</td>
  </tr>
  <tr>
    <td>Visualization and dimensionality reduction: PCA Principle COmponent analysis, Kernel PCA, Locality,-Linear Embedding(LLE), t-distributed Stochastic Neighbor Embedding(t-SNE)</td>
  </tr>
  <tr>
    <td></td>
  </tr>
</table>
<!--Table generated using https://www.tablesgenerator.com/html_tables#  -->

<br><b>Ensemble Learning</b> Combining multiple learning or models for better performance.
Ensemble methods outperform(superior to) individual models by averaging out biases, reducing variance, and are less likely to overfit.
<br> There's a common line in machine learning which is: "ensemble and get 2%.". This implies that you can build your models as usual and typically expect a small performance boost from ensembling. More on this <a href="https://elitedatascience.com/machine-learning-interview-questions-answers">here</a>.

<br><br>
<b>Explain Bagging </b><br>
(1) Bagging, or Bootstrap Aggregating, is an ensemble method in which the dataset is first divided into multiple subsets through resampling.<br>
(2)Then, each subset is used to train a model, and the final predictions are made through voting or averaging the component models.  <br>
(+) Bagging is performed in <b>parallel</b>.<br>
(+) Bagging reduces the variance of the meta learning algorithm. Bagging can be applied to decision tree or other algorithms.<br>


<p>
Application specific learning algorithms:
<ul>
  <li>Text, NLP</li>
  <li>Image Processing</li>
  <li>Audio Processing</li>
  <li>Audio Transcription</li>
  <li>Video</li>
  <li>Recommendations</li>
</ul>

<b>Here Polyfit/ SVR example</b>.<br>
{% highlight python %}
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

#https://docs.scipy.org/doc/numpy/reference/generated/numpy.polyfit.html

#install sklearn 
#https://stackoverflow.com/questions/13212987/cannot-import-scikits-learn-even-though-it-seems-to-be-installed
#http://scikit-learn.org/stable/install.html
#https://stackoverflow.com/questions/11114225/installing-scipy-and-numpy-using-pip
#sudo pip install -t . numpy scipy scikit-learn
from sklearn.svm import SVR


data = pd.read_csv('laplace2d_tol.dat', delim_whitespace=True, header=None, skiprows=14, comment="#")
#sep = "\t"  #separator if not using delim_whitespace. delim_whitespace is faster than regex
#names=["a", "b", "c"] # can be ised in read_csv instead of 'colums' used in this code
data.columns = ["iterations", "error", "runtime"]


print "rows:",len(data)
#iteration of dataframe
for index, row in data.iterrows():
    if index ==0:
        print "first entry  iterations:",row['iterations'], "---runtime:",row['runtime']
col =  data['iterations'].values.flatten()
print "data flattened:\n  ", col

#option-1: split the data in {train, test} w/ sklearn
#from sklearn.model_selection import train_test_split
#train, test = train_test_split(df, test_size=0.2)

#option-2 split the data in {train, test} with pandas
#train_rand = data.sample(frac=0.5,random_state=200)
#random_state for randomization

#split without randomization
split_ratio = 0.5
train_size = int(len(data)*split_ratio)
train = data[:train_size]
col =  train['iterations'].values.flatten()
print "trains samples:\n", col

test  = data.drop (train.index)
data['iterations'].values.flatten()

print "total train samples =", len(train)
print "total test samples =", len(test)

turns = 8
x_train = train['error'].values.flatten()
y_train = train['iterations'].values.flatten()
z = np.polyfit(x_train, y_train, turns)
p = np.poly1d(z)

test_value = 0.000073 #error here
print "  p(",test_value,"): predicted=%.3f" %(p(test_value))

x_test = test['error'].values.flatten()
y_test = test['iterations'].values.flatten()
y_predicted =  p(x_test)


####################################################
##http://scikit-learn.org/stable/auto_examples/svm/plot_svm_regression.html
#ALTERNATIVE learning SVM SR=VR

#fit regression model
svr_rbf = SVR(kernel='rbf', C=1e3, gamma=0.1)

#predict
y_rbf = svr_rbf.fit(x_train, y_train).predict(x_test)
lw=2


plt.plot(x_train, y_rbf, color='navy', lw=lw, label='RBF model')

plt.plot(x_train, y_train, 'x', label='train', color = "black",lw=lw)
plt.plot(x_test, y_test, '-', label='test-real', color="red",lw=lw )
plt.plot(x_test, y_predicted, '--', label='test-predicted', color="green", lw=lw)
plt.ylabel("iterations")
plt.xlabel("error")
plt.ylim(0,4000)
plt.grid(True)
plt.xlim(max(x_train), min(x_test))
plt.legend(loc="upper center")
plt.show()


"""
#numpy polyfit
#https://docs.scipy.org/doc/numpy/reference/generated/numpy.polyfit.html

#PROBLEM 1: predicting
x = np.array([0.000302, 0.000254, 0.000201, 0.000151])
y = np.array([58.283531, 70.682653, 91.963334, 127.307981])
z = np.polyfit(x, y, turns)
p = np.poly1d(z)
print "  p(0.000101):runtime actual=202.007 predicted=%.3f" %(p(0.000101))


#PROBLEM 2: predicting number of loops
x = np.array([0.000302, 0.000254, 0.000201, 0.000151])
y = np.array([800, 950, 1200, 1600])
z = np.polyfit(x, y, turns)
p = np.poly1d(z)
print "  p(0.000101) loops actual=2400 predicted=",p(0.000101)

#Plotting
#xp = np.linspace(0.000050, 0.000302, 20)
xp = np.linspace(0.000050, 0.000302, 20)
_ = plt.plot(x, y, 'o', xp, p(xp), '-', xp, p(xp), '--')
plt.ylim(800, 3000) # iterations limit on y-axis
plt.show()
"""



{% endhighlight %}



THis<br>

<code>PREEMPT=y</code>
and:
<code>CONFIG_1000_HZ=y</code>
To add some powersaving check this one:
<code>CONFIG_NO_HZ=y</code>

Additionally, preemt, lowlatency or the rt kernel wont make your system faster (for general tasks?). They are slightly slower than generic kernel.


<h4>Good references</h4>
* 
* Google Cloud Speech to Text API Python: https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/speech/cloud-client

Data:
#Jacobi relaxation Calculation: 2048 x 2048 mesh
<pre>
    0     0.250000  0.080103
    50    0.004751  3.792906
   100    0.002397  7.241144
   150    0.001601  10.689349
   200    0.001204  14.137605
   250    0.000964  17.586176
   300    0.000804  21.034838
   350    0.000689  24.483031
   400    0.000603  27.931385
   450    0.000537  31.379790
   500    0.000483  34.828075
   550    0.000439  38.461205
   600    0.000403  42.302963
   650    0.000372  46.218797
   700    0.000345  50.199788
   750    0.000322  54.226861
   800    0.000302  58.283531
   850    0.000284  62.380708
   900    0.000269  66.513298
   950    0.000254  70.682653
  1000    0.000242  74.883781
  1050    0.000230  79.113273
  1100    0.000220  83.370917
  1150    0.000210  87.651342
  1200    0.000201  91.963334
  1250    0.000193  96.300035
  1300    0.000186  100.660911
  1350    0.000179  105.045946
  1400    0.000173  109.454092
  1450    0.000167  113.884365
  1500    0.000161  118.337551
  1550    0.000156  122.811895
  1600    0.000151  127.307981
  1650    0.000147  131.824508
  1700    0.000142  136.359728
  1750    0.000138  140.916791
  1800    0.000134  145.489694
  1850    0.000131  150.082835
  1900    0.000127  154.705981
  1950    0.000124  159.384847
  2000    0.000121  164.102636
  2050    0.000118  168.786614
  2100    0.000115  173.480620
  2150    0.000112  178.193624
  2200    0.000110  182.925185
  2250    0.000107  187.672891
  2300    0.000105  192.434249
  2350    0.000103  197.213852
  2400    0.000101  202.007015
  2450    0.000099  206.817127
  2500    0.000097  211.642450
  2550    0.000095  216.483002
  2600    0.000093  221.339118
  2650    0.000091  226.206693
  2700    0.000090  231.086084
  2750    0.000088  235.979211
  2800    0.000086  240.890114
  2850    0.000085  245.819259
  2900    0.000083  250.761324
  2950    0.000082  255.716833
  3000    0.000081  260.684375
  3050    0.000079  265.667891
  3100    0.000078  270.660522
  3150    0.000077  275.670072
  3200    0.000076  280.697543
  3250    0.000074  285.746627
  3300    0.000073  290.809013
</pre>
