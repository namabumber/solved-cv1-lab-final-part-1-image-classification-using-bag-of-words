Download Link: https://assignmentchef.com/product/solved-cv1-lab-final-part-1-image-classification-using-bag-of-words
<br>
The goal of the assignment is to implement a system for image classification. In other words, this system should tell if there is an object of given class in an image. You will perform 5-class ( {1 : <em>airplanes,</em>2 : <em>birds,</em>3 : <em>ships,</em>4 : <em>horses,</em>5 : <em>cars</em>}) image classification based on bag-of-words approach<sup>1 </sup>using SIFT features, respectively. STL-10 dataset<sup>2 </sup>will be used for the task. For each class, test sub-directories contain 800 images, and training sub-directories contain 500 images. Images are represented as (RGB) 96×96 pixels.

<strong>Hint</strong>

In a real scenario, the public data you use often deviates from your task. You need to figure it out and re-arrange the label as required using <em>stl10 input.py </em>as a reference.

Download the dataset from <a href="http://ai.stanford.edu/~acoates/stl10/stl10_binary.tar.gz">http://ai.stanford.edu/</a><a href="http://ai.stanford.edu/~acoates/stl10/stl10_binary.tar.gz">~</a><a href="http://ai.stanford.edu/~acoates/stl10/stl10_binary.tar.gz">acoates/stl10/stl10_binary.tar.gz</a><a href="http://ai.stanford.edu/~acoates/stl10/stl10_binary.tar.gz">. </a>There are five files: <em>test X.bin</em>, <em>test y.bin</em>, <em>train X.bin</em>,<em>train y.bin </em>and <em>unlabeled X.bin</em>. For the project, you will just use the train and test partitions. Download the dataset and make yourself familiar with it by figuring out which images and labels you need for the aforementioned 5 classes. Note that you do not need <em>fold </em><em>indices </em>variable.

<h2>1.1         Training Phase</h2>

Training must be conducted over the training set. Keep in mind that using more samples in training will likely result in better performance. However, if your computational resources are limited and/or your system is slow, it’s OK to use less number of training data to save time.

<a href="http://www.robots.ox.ac.uk/~az/icvss08_az_bow.pdf">http://www.robots.ox.ac.uk/</a><a href="http://www.robots.ox.ac.uk/~az/icvss08_az_bow.pdf">~</a><a href="http://www.robots.ox.ac.uk/~az/icvss08_az_bow.pdf">az/icvss08_az_bow.pdf</a>

<a href="https://cs.stanford.edu/~acoates/stl10/">2 </a><a href="https://cs.stanford.edu/~acoates/stl10/">https://cs.stanford.edu/</a><a href="https://cs.stanford.edu/~acoates/stl10/">~</a><a href="https://cs.stanford.edu/~acoates/stl10/">acoates/stl10/</a>

<h2>1.2         Testing Phase</h2>

You have to test your system using the specified subset of test images. All 800 test images should be used at once for testing to observe the full performance. Again, exclude them from training for fair comparison.

<h1>2           Bag-of-Words based Image Classification</h1>

Bag-of-Words based Image Classification system contains the following steps:

<ol>

 <li>Feature extraction and description</li>

 <li>Building a visual vocabulary</li>

 <li>Quantify features using visual dictionary (encoding)</li>

 <li>Representing images by frequencies of visual words</li>

 <li>Train the classifier</li>

</ol>

We will consider each step in detail.

<h2>2.1         Feature Extraction and Description</h2>

SIFT descriptors can be extracted from either (1) densely sampled regions or (2) key points. You can use SIFT related functions in <em>OpenCV </em>for feature extraction.

<h2>2.2         Building Visual Vocabulary</h2>

Here, we will obtain visual words by clustering feature descriptors, so each cluster center is a visual word, as shown in Figure 1. Take a subset (maximum half) of all training images (this subset should contain images from ALL categories), extract SIFT descriptors from all of these images, and run k-means clustering (you can use your favourite k-means implementation) on these SIFT descriptors to build visual vocabulary. Then, take the rest of the training images to calculate visual dictionary. Nonetheless, you can also use less images, say 100 from each class (exclusive from the previous subset) if your computational resources are limited. Pre-defined cluster numbers will be the size of your vocabulary. Set it to different sizes (500, 1000 and 2000).

Figure 1: An illustration on learning visual dictionary. Note: (1) Code-words is another term for visual words. (2) The figure is from <em>Hosef Slvic</em>, with SIFT space used.

<h2>2.3         Encoding Features Using Visual Vocabulary</h2>

Once we have a visual vocabulary, we can represent each image as a collection of visual words. For this purpose, we need to extract feature descriptors (SIFT) and then assign each descriptor to the closest visual word from the vocabulary.

<h2>2.4         Representing images by frequencies of visual words</h2>

The next step is the quantization. The idea is to represent each image by a histogram of its visual words, see Figure 2 for overview. Check out <em>matplotlib</em>’s hist function. Since different images can have different numbers of features, histograms should be normalized.

Figure 2: Schematic representation of Bag-Of-Words system.

<h2>2.5         Classification</h2>

We will train a classifier per each object class. Now, we take the Support Vector Machine (SVM) as an example. As a result, we will have 5 binary classifiers. Take images from the training set of the related class (should be the ones which you did not use for dictionary calculation). Represent them with histograms of visual words as discussed in the previous section. Use at least 50 training images per class or more, but remember to debug your code first! If you use the default setting, you should have 50 histograms of size 500. These will be your positive examples. Then, you will obtain histograms of visual words for images from other classes, again about 50 images per class, as negative examples. Therefore, you will have 200 negative examples. Now, you are ready to train a classifier. You should repeat it for each class. To classify a new image, you should calculate its visual words histogram as described in Section 2.4 and use the trained SVM classifier to assign it to the most probable object class. (Note that for proper SVM scores you need to use cross-validation to get a proper estimate of the SVM parameters. In this assignment, you do not have to experiment with this cross-validation step).

<h2>2.6         Evaluation</h2>

To evaluate your system, you should take all the test images from all classes and rank them based on each binary classifier. In other words, you should classify each test image with each classifier and then sort them based on the classification score. As a result, you will have five lists of test images. Ideally, you would have images with airplanes on the top of your list which is created based on your airplane classifier, and images with cars on the top of your list which is created based on your car classifier, and so on.

In addition to the qualitative analysis, you should measure the performance of the system quantitatively with the Mean Average Precision over all classes. The Average Precision for a single class c is defines as

,                                                                              (1)

where <em>n </em>is the number of images (<em>n </em>= 50 × 5 = 250), <em>m </em>is the number of images of class <em>c </em>(<em>m<sub>c </sub></em>= 50), <em>x<sub>i </sub></em>is the <em>i<sup>th </sup></em>image in the ranked list <em>X </em>= {<em>x</em><sub>1</sub><em>,x</em><sub>2</sub><em>,…,x<sub>n</sub></em>}, and finally, <em>f<sub>c </sub></em>is a function which returns the number of images of class <em>c </em>in the first <em>i </em>images if <em>x<sub>i </sub></em>is of class <em>c</em>, and 0 otherwise. To illustrate, if we want to retrieve <em>R </em>and we get the following sequence: [<em>R,R,T,R,T,T,R,T</em>], then <em>n </em>= 8, <em>m </em>= 4, and.