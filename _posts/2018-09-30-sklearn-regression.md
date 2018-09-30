---
published: true
---
### Regression in Python sklearn
Lets get started with installing the required python packages. I am on a ubuntu 18.04 x86_64 system. If you dont have the libraries installed yet, here are the commands--

#### Installation of packages
{% highlight bash %}
sudo apt-get install python-pip
sudo pip install numpy
sudo pip install pandas
sudo pip install sklearn
sudo pip install matplotlib

# you can also install these packagees with apt-get but pip is better.
#sudo apt-get install python-pip python-numy 
#sudo apt-get install python-pandas python-sklearn
{% endhighlight %}

#### To install jupyter IPython-Notebook ipynb
{% highlight bash %}
apt-get -y install ipython ipython-notebook
sudo apt-get -y install ipython ipython-notebook
sudo pip install jupyter

#lets start a notebook session on a directory
cd /practice/python
#opens --  http://localhost:8888/tree
jupyter notebook
{% endhighlight %}
