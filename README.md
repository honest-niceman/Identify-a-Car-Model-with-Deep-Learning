# Identify-a-Car-Model-with-Deep-Learning
Explore how to practice real world Data Science by collecting data, curating it and apply advanced Deep Learning techniques to
create high quality models which can be deployed in production. Use Keras and Pytorch libraries in python for applying advanced techniques like data augmentation, drop out, batch normalization and transfer learning

# Index
- [Data](#data)
- [Big Picture](#big-picture)
- [Pre-Requisites, Resources and Acknowledgments](#pre-requisites--resources-and-acknowledgments)
- [A note on the motivation and challenges](#a-note-on-the-motivation-and-challenges)
- [Training a base CNN model](#training-a-base-cnn-model)
  * [Training in Keras](#training-in-keras)
  * [Keras: Training and Validations Results](#keras--training-and-validations-results)
  * [Keras source code](#keras-source-code)
  * [Training in Pytorch](#training-in-pytorch)
  * [Pytorch: Training and Validations Results](#pytorch--training-and-validations-results)
  * [Pytorch source code](#pytorch-source-code)
- [Using Data Augmentation](#using-data-augmentation)
  * [Data Augmentation in Keras](#data-augmentation-in-keras)
  * [Data Augmentation Results in Keras](#data-augmentation-results-in-keras)
  * [Data Augmentation source code in Keras](#data-augmentation-source-code-in-keras)
  * [Data Augmentation in Pytorch](#data-augmentation-in-pytorch)
  * [Data Augmentation Results in Pytorch](#data-augmentation-results-in-pytorch)
  * [Data Augmentation source code in Pytorch](#data-augmentation-source-code-in-pytorch)
- [Train with Dropout](#train-with-dropout)
  * [Dropout in Keras](#dropout-in-keras)
  * [Dropout Results with Keras](#dropout-results-with-keras)
  * [Dropout Source code in Keras](#dropout-source-code-in-keras)
  * [Dropout in Pytorch](#dropout-in-pytorch)
  * [Dropout Results with Pytorch](#dropout-results-with-pytorch)
  * [Dropout Source code in Pytorch](#dropout-source-code-in-pytorch)
- [Batch normalization](#batch-normalization)
  * [Batch Normalization in Keras](#batch-normalization-in-keras)
  * [Batch Normalization results in Keras](#batch-normalization-results-in-keras)
  * [Batch Normalization source code in Keras](#batch-normalization-source-code-in-keras)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



# Data
- There are 4000 images of two of the popular cars (Swift and Wagonr) in India of make Maruti Suzuki with 2000 pictures belonging to
each model class. The data is divided into training set with 2400 images, validation set with 800 images and test set with 800 images. 
- The data was randomized before splitting into training, test and validation set.
- Data was collected using a web scraper written in python. Selenium library was used to load the full HTML page and 
Beautifulsoup library was used to extract and download images from HTML tags
- The data is hosted at [Kaggle](https://www.kaggle.com/ajaykgp12/cars-wagonr-swift) which can be downloaded or you can 
also create a notebook directly on Kaggle and access data in your notebook.
![data_image](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/cover_image.PNG)

# Big Picture
- Data science beginners often start with curated set of data, but it's a well-known fact that in a real Data Science Projects, major time is spent on collecting, cleaning and organizing data.  Also, domain expertise is considered as an important aspect of creating good ML models. 
- Being an automobile enthusiast, I took up this challenge to collect images of two of the popular car models from a used car website, where users upload the images of the car they want to sell and then train a Deep Neural Network to identify model of a car from car images. 
- In my search for images I found that approximately 10 percent of the car pictures did not represent the intended car correctly and those pictures must be deleted from final data.

- We will explore all Major Deep Learning framework starting from Keras, and moving on to Pytorch and TensorFlow 2.0. 
- Keras is a high-level deep learning framework and is very well suited for fast prototyping and is also recommended for beginners. 
- Pytorch is gaining popularity due to its use in Research and is considered pythonic, it provides flexibility to experiment with different Neural Network Architectures. 
- TensorFlow 2.0 is a revamp of old TensorFlow 1.xxx version which was not very user friendly with steep learning curve, static graphs and lots of boilerplate code to get things done. TensorFlow 2.0 solves this problem by adopting ideas from Keras and Pytorch. TensorFlow is most suited to deployment in production with support to multiple production environments.

As we are dealing with Images, we will be focusing on Convolutional Neural Networks (CNNs / ConvNets), and start with simple CNN Model and train it from scratch.
We will then move on to advance techniques like Data Augmentation, Dropout, Batch Normalization and  Transfer Learning using Pre-Trained networks like VGG16 trained on ImageNet.
<br> As we progress in our journey we will also explore some key aspects below, which are important for any Data Science Project.
1.	How much data is enough to get reliable results from Deep Neural Networks and is more data is always good?
2.	How do I deal with Bias and Variance tradeoff, and how to select best model which can generalize better without sacrificing too much of performance?
3.	For image recognition tasks what works best, custom CNN model or a Pre-Trained network?
4.	What strategy to choose to validate the model performance. Hint: Trust your validation score and do not touch test set till end.
5. What Deep Learning Framework to choose and which one is best suitable for the task.

<br>There are no straightforward answers to some of the questions and it depends on context and the type of problem or data that we are dealing with, and it will be my endeavor to answers some of the difficult questions with experimentation and analysis.

# Pre-Requisites, Resources and Acknowledgments
- It is assumed that you have some experience in Python and basic deep learning concepts.

- Many of the ideas presented here are based on book [Deep Learning with Python]( https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438), written by Francois Chollet who also happens to be author of Keras library

- Another excellent resource to learn theoretical concepts around deep learning is [Deep Learning Specialization]( https://www.coursera.org/specializations/deep-learning) taught by Andrew Ng on Coursera. Even though this is paid course the videos are freely available on YouTube. Initially it was not easy to grasp the convolutions, but thanks to Andrew Ng, Convolution Neural Networks no longer appear to be convoluted

- The book  focuses on the practical application while  Andrew Ng gravitates towards theoretical concepts with easy to understand mathematics. I have personally benefit from both, and even though both differ in their approaches they complement each other well just like ensemble machine learning models.

- Its recommended that that code be run on machine with GPU, there are many ways to achieve it without owning a high-end machine.  You can use Kaggle Notebook with GPU which is what I would be doing throughout the project. Links to Kaggle Notebooks will be shared and it can be forked, modified and results can be easily reproduced without any extra set up. Another way is to use Google Colab or Google Cloud Platform with free credits of 300 USD.

# A note on the motivation and challenges
- When I started with this project my goal was to achieve a reasonable performance with my image classification model with more focus on building a robust model which can generalize well on unseen data.
- The data is user generated and images are taken by many users from all over India, from different angles, under different lighting conditions and  using mobile devices with varying image quality. 
- This presents an interesting challenge and I am very much curious as well as anxious as to how things would turn out. 
  There are multiple stories on how some models which performed well during training, failed  to perform on new data that came from different distribution.
- To make sure that our model is robust here are some techniques that we will apply.
  1.	When gathering data, I made sure that I downloaded car images from multiple regions like Delhi, UP, Maharashtra etc.
  2.	Delete images which are not car, incorrect model, closeup images, interior images. Anything which a human cannot recognize most likely ML model probably can???t recognize. Interestingly one of the class of data is WagonR, the model I owned for 7 years so it was easy for me to curate the data manually. This is where domain experience comes in handy.
  3.	The data is randomized and split into Training Set with 2400 images, Validation Set with 800 images and test set with 800 images. The model will be trained on training set, fine-tuned with feedback from validation data and test data will not evaluated until end when we have found our best model based on validation set. This is important because the test set acts as unseen data which model will encounter in future and if it performs well on this data, our model  will perform well on unseen data. To further challenge our model, I have created another set of test data which was taken from different used car website and  different city (Hyderabad).

Hold on with your seat belts, grab some popcorn, and be ready for an exciting as well as thrilling ride, this sure will be a long and interesting one.

# Training a base CNN model

The CNN model architecture is shown below.
![model_arch](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/cnn_arch.PNG)

- The images are converted to 150 X 150 X 3 shape and fed to CNN model. The first CONVD layer performs convolution on the input image with 32 filters with filter size of 3, resulting in layer of dimension 148 X 148 X 32,  which is then down sampled by a Max Pool layer of filter size 2 and stride of 2 resulting in layer of dimensions 74X74X32.  We are using four CONVD layers each with filter size of 3, followed by a Max Pooling layer of filter size 2 and stride of 2. The output from last MAX pool layer is flattened and converted to dense layer of shape 512 X 1. The final output layer consists of a single layer with sigmoid activation function. The other layers use Relu Activation Function

- You can notice that convolution operation increases the depth of the layer while keeping height and width almost same, while max pool operation halves the height and width while keeping depth same. There is very simple math behind it which is not in scope of this tutorial. Andrew Ng explains this very well in his course.

## Training in Keras
The keras code for buliding model is shown below
```python

def build_cnn(display_summary =False):
    model = models.Sequential()
    model.add( layers.Conv2D(32, (3,3),  activation= 'relu', input_shape = (150, 150, 3)) )
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(64, (3,3),  activation= 'relu') )
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(128, (3,3),  activation= 'relu') )
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(128, (3,3),  activation= 'relu') )
    model.add(layers.MaxPooling2D((2,2)))

    model.add(layers.Flatten())
    model.add(layers.Dense(512, activation= 'relu'))
    model.add(layers.Dense(1, activation= 'sigmoid'))

    model.compile(loss = 'binary_crossentropy',
                  optimizer = optimizers.RMSprop(lr = 1e-4),
                  metrics = ['acc']
                  )
    if display_summary:
       model.summary()
    return model
```
Model summary 
```
______________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
conv2d_1 (Conv2D)            (None, 148, 148, 32)      896       
_________________________________________________________________
max_pooling2d_1 (MaxPooling2 (None, 74, 74, 32)        0         
_________________________________________________________________
conv2d_2 (Conv2D)            (None, 72, 72, 64)        18496     
_________________________________________________________________
max_pooling2d_2 (MaxPooling2 (None, 36, 36, 64)        0         
_________________________________________________________________
conv2d_3 (Conv2D)            (None, 34, 34, 128)       73856     
_________________________________________________________________
max_pooling2d_3 (MaxPooling2 (None, 17, 17, 128)       0         
_________________________________________________________________
conv2d_4 (Conv2D)            (None, 15, 15, 128)       147584    
_________________________________________________________________
max_pooling2d_4 (MaxPooling2 (None, 7, 7, 128)         0         
_________________________________________________________________
flatten_1 (Flatten)          (None, 6272)              0         
_________________________________________________________________
dense_1 (Dense)              (None, 512)               3211776   
_________________________________________________________________
dense_2 (Dense)              (None, 1)                 513       
=================================================================
Total params: 3,453,121
Trainable params: 3,453,121
Non-trainable params: 0
_________________________________________________________________
```

## Keras: Training and Validations Results
The model was trained for 50 epochs on a Kaggle notebook with GPU and achived accuracy of 88.125 percent on validation set.
![results](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/results_keras_cnn_base.PNG)
- I had set a conservative target of 80% accuracy, but it seems our baseline CNN model performed better than expected with 88% accuracy.  If you ask me if this a good accuracy, and I might say it???s pretty good considering that a random classifier will be 50% accurate as there are  equal number of samples of each class.  But how is the performance compared to a human, and I will agree that humans will typically perform with 96% accuracy. The benchmark is raised, and our goal will be to achieve near human performance. At this point we have no idea if we can achieve the target accuracy.
- Ok, the model is pretty good for a baseline model, what about its robustness, can it perform well on unseen data?. As we can see from the screenshot that the training accuracy is much higher than the validation accuracy throughout all epochs, and now we are talking the bias and variance tradeoff. The model is clearly showing classic symptoms of low bias (since training accuracy is near 100%), and high variance(since validation accuracy is much lower at 88) or overfitting.  I would be not willing to put this model into production even if you think the accuracy is good enough. As we will see there many ways to deal with this and we will explore it in next sections.

## Keras source code 
 - [github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/keras/cars_keras_cnn_baseline.ipynb)
 - [kaggle](https://www.kaggle.com/ajaykgp12/cars-keras-cnn?scriptVersionId=20357823)
 ## Training in Pytorch
    The same model in pytorch can be written as shown below.
    
```python
 class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(in_channels= 3, out_channels=32, kernel_size= 3)
        self.pool = nn.MaxPool2d(kernel_size=2, stride= 2)
        
        self.conv2 =  nn.Conv2d(in_channels= 32, out_channels= 64, kernel_size= 3)
        self.conv3 =  nn.Conv2d(in_channels= 64, out_channels= 128, kernel_size= 3)
        self.conv4 =  nn.Conv2d(in_channels= 128, out_channels= 128, kernel_size= 3)
    
       #128 * 128 * 7 is the output of the last max pool layer
        self.fc1 = nn.Linear(128 * 7 * 7, 512)
        self.fc2 = nn.Linear(512, 2)       

    def forward(self, x):
        x = self.pool(F.relu(self.conv1(x)))
        x = self.pool(F.relu(self.conv2(x)))
        x = self.pool(F.relu(self.conv3(x)))
        x = self.pool(F.relu(self.conv4(x)))
        
        #this is similar to flatten in keras but keras is smart to figure out dimensions by iteself.
        x = x.view(-1, 128 * 7 * 7)
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
 ```
The Pytorch model differs slightly as we have two output neurons instead of one in Keras.  This is fine because in this case we can imagine each neuron outputting result corresponding to each class. The loss function [CrossEntropyLoss]( https://pytorch.org/docs/stable/nn.html#crossentropyloss) used in Pytorch includes the SoftMax activation and there no need to activate the output neurons. In Keras a single output neuron is activated by sigmoid activation function which is outputting probability of positive class.  
## Pytorch: Training and Validations Results
![keras_cnn_base_results](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/results_pytorch_cnn_base.PNG)
- It was little time consuming to re-write the same model in Pytorch as it does not have Keras like fit method to train as well as evaluate your model on validation set. In Pytorch you will have to write your own training and test methods and run each method for every epoch. The advantage is you have more control at every epoch and can write custom metrics or loss functions easily and implement callbacks like early stopping and saving/keeping best model. We can see that the Pytorch model displays same pattern as Keras with low bias and high variance.
- The Pytorch performs  better with validation accuracy of 0.896 compared to 0.88125 in Keras. This can be due to normalization, in Pytorch the image pixel values are scaled to [-1,1] while in Keras the same are scaled to [0,1].  When I used the scaling of [0,1] in Pytorch the results were even worse than Keras. So, its would be worthwhile to revisit and check how Keras performs under same normalization

## Pytorch source code
- [kaggle](https://www.kaggle.com/ajaykgp12/cars-pytorch-cnn?scriptVersionId=20577825)
- [github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/pytorch/cars_pytorch_cnn.ipynb)
# Using Data Augmentation 
We saw that vanilla CNN model overfitted on our data, this is because the number of training samples of 2400 images is less for a deep neural network.  We can collect more data if it is easy to do so, but in some cases that may not be feasible. One of widely used technique to solve this is by data augmentation. We can apply a few images transforms like, rotating it, horizontal flip, random cropping, shearing etc.
## Data Augmentation in Keras
The code below achieves this in Keras and the corresponding output is also shown when the transformations are applied to a single image.
```python
datagen = ImageDataGenerator( rotation_range= 40,
                              width_shift_range = 0.2,
                             height_shift_range = 0.2,
                             shear_range = 0.2,
                             zoom_range = 0.2,
                             horizontal_flip = True,                           
                            )
 ```
![keras_aug](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/keras_aug.PNG)

It is a common misconception that data augmentation increases the training data size.  The data augmentation does not result in more images and the trasnformations are applied on the fly during training and a random transformation is applied every epoch. This way model sees a different variation of the same image on every epoch which explains why it works so well. Another thing to keep in mind is data augmentation is only applied on training set and not on validation or test set.
## Data Augmentation Results in Keras
![kera_sug_results](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/keras_aug_results.PNG)

- The results are impressive, and our validation accuracy jumped from 0.88 to 0.95, we are very close to our target accuracy of 0.96 or more to beat human performance. Our model is not overfitting, as the training and validation curves are close to each other
- Please note that the benchmark accuracy of 0.96 for humans for based on what I have read on research papers and for our data it may or may not be correct. But having a benchmark is always good to know how our model performed. 
 

## Data Augmentation source code in Keras
- [kaggle](https://www.kaggle.com/ajaykgp12/cars-keras-data-aug-dropout-bn?scriptVersionId=20565354)
- [github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/keras/cars_keras_data_aug.ipynp.ipynb)

## Data Augmentation in Pytorch
The data augmentation works in similar way in Pytorch and we have also defined some additional transformations like color saturations. The below code achieves this in Pytorch with a sample output on images. It???s important to verify how images appear after transformation as it may negatively impact model performance. For example too much cropping can lead to loss of information or vertical flip may not be required as in our case.
```python 
transform_train = transforms.Compose( [                                  
                                 transforms.Resize((150,150)), 
#                                  transforms.RandomCrop(size =100),
                                 transforms.RandomHorizontalFlip(),
                                 transforms.RandomRotation(20, resample=PIL.Image.BILINEAR),
                                 transforms.ColorJitter(hue= 0.01, saturation=.01),
                                 transforms.RandomAffine(degrees = 0, translate=(0.2,0.2),  shear= 0.2),
                                  ])
 ```                                
![pytorch_aug](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/pytorch_aug.PNG)
## Data Augmentation Results in Pytorch
![pytorch_aug_results](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/pytorch_aug_results.PNG)
Our score jumped from 0.896 to 0.965 inline with what we saw with Keras. The Pytorch score of 0.965 is still ahead of Keras score of 0.95, but I believe with some fine tuning we can achieve similar score in Keras too. What???s more important is that our strategy seems to be working fine and we have broken the benchmark human performance of 0.96 in this step. But we will not celebrate yet, as we have not tested on the test set against which we will compare the benchmark performance. We will continue to fine tune with other strategies like Dropout, Batch Normalization and Transfer Learning. Stay tuned.

## Data Augmentation source code in Pytorch
- [kaggle](https://www.kaggle.com/ajaykgp12/cars-pytorch-aug?scriptVersionId=20578393)
- [github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/pytorch/cars_pytorch_data_aug.ipynp.ipynb)

# Train with Dropout
Dropout was proposed by Nitish Srivastava, Geoffrey Hinton, Alex Krizhevsky, Ilya Sutskever, Ruslan Salakhutdinov in their [paper](http://jmlr.org/papers/v15/srivastava14a.html) as way to prevent neural networks from overfitting.
It works by randomly turning off activations of some neurons. The number of neurons to drop are defined by  probability defined in dropout layer.
The image below explains this process.
![dropout](https://www.researchgate.net/profile/Amine_Ben_khalifa/publication/309206911/figure/fig3/AS:418379505651712@1476760855735/Dropout-neural-network-model-a-is-a-standard-neural-network-b-is-the-same-network.png)
- You can imagine this process by taking an example of classroom where in a class only a few students would generally participate in discussions and the rest of class usually  remains silent. The quality of teaching suffers because teacher is changing the content  based on the feedback of select few students. If teacher randomly asks a group of students to participate in discussion, the silent ones would start paying more attention and teacher can fine tune the content with more feedback.

- We will apply dropout immediately after flattening the layer. If you see the network architecture, the number of neurons on flattened layer is 6272 and we will apply dropout on this layer by randomly dropping 50% neurons. There is no need to apply dropout on ConvD layers as the output neurons are few. We will also need to train for more epochs(200) as dropout takes more iteration to converge 

## Dropout in Keras
Its very simple to add dropout layer in keras as shown in code example below
```python
  model.add(layers.Flatten())
  model.add(layers.Dropout(0.5))
```
## Dropout Results with Keras
![keras_drop_res](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/keras_drop.PNG)
The effect of dropout was marginal, and our validation score improved from 0.95 to 0.956. We will however continue with dropout and fine tune our model again with batch normalization in next section
## Dropout Source code in Keras
-	[kaggle]( https://www.kaggle.com/ajaykgp12/cars-keras-data-aug-dropout-bn?scriptVersionId=20566728)
-	[github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/keras/cars_keras_data_aug_drop.ipynb)

## Dropout in Pytorch
The relevant portion of code to set dropout is shown below
```python
class Net(nn.Module):
    def __init__(self):
        .....
        self.dropout = nn.Dropout(p=0.5)
       

    def forward(self, x):  
        ........
        x = x.view(-1, 128 * 7 * 7)   
        x = self.dropout(x)
        x = F.relu(self.fc1(x))
        x = self.fc2(x)
 ```

## Dropout Results with Pytorch
![drop_pytorch](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/pytorch_drop.PNG)
Unlike in Keras the validation accuracy decreased with dropout from 0.966 to 0.963 and the validation and training curves are also similar.  Since there is no noticeable improvement in either accuracy or variance, we will drop the dropout for Pytorch and continue with Batch Normalization in next section without dropout

## Dropout Source code in Pytorch
- [Kaggle](https://www.kaggle.com/ajaykgp12/cars-pytorch-aug-bn?scriptVersionId=20582538)
- [github](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/pytorch/cars_pytorch_data_aug_drop.ipynb)

# Batch normalization
- [Batch normalization](https://en.wikipedia.org/wiki/Batch_normalization) is a technique for improving the speed, performance, and stability of artificial neural networks. Batch normalization was introduced in a 2015 [paper](https://arxiv.org/pdf/1502.03167.pdf). It is used to normalize the input layer by adjusting and scaling the activations.

- In a typical neural network, we pre-process the data by normalizing so that the input values in well-defined range. For example, the Image pixel values are between [0, 255] and we have converted it to [-1,1] as in case of our Pytorch model or [0,1] for our Keras Model. 

- Input ???> first layer -> activation  function  ->  output -> second layer
- The normalized input feature is then passed to first layer of neural network which then passes through activation function. As the neural network becomes deeper and deeper, the subsequent input and outputs are no longer of same distribution which results in internal covariate shift.
 
- [The basic idea]( https://mlexplained.com/2018/01/10/an-intuitive-explanation-of-why-batch-normalization-really-works-normalization-in-deep-learning-part-1/)  behind batch normalization is to limit covariate shift by normalizing the activations of each layer (transforming the inputs to be mean 0 and unit variance). This, supposedly, allows each layer to learn on a more stable distribution of inputs, and would thus accelerate the training of the network.

- The original paper recommended that batch normalization be done before activation function.
    Input ???> first layer -> batch normalization-> activation function  ->  output -> second layer
    
    But as we will see it may not always lead to better results and sometimes batch normalization works  better after activation function

## Batch Normalization in Keras
The implementation of Keras for batch implementation is shown below. Here batch normalization is done after activation function as this resulted in better performance than doing it before activation function.  
```python

def build_cnn(display_summary =False):
    model = models.Sequential()
    model.add( layers.Conv2D(32, (3,3),  input_shape = (150, 150, 3)) )    
    model.add(Activation("relu"))
    model.add(layers.BatchNormalization())
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(64, (3,3)) )    
    model.add(Activation("relu"))
    model.add(layers.BatchNormalization())
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(128, (3,3)) )   
    model.add(Activation("relu"))
    model.add(layers.BatchNormalization())
    model.add(layers.MaxPooling2D((2,2)))

    model.add( layers.Conv2D(128, (3,3)) )    
    model.add(Activation("relu"))
    model.add(layers.BatchNormalization())
    model.add(layers.MaxPooling2D((2,2)))

    model.add(layers.Flatten())
    model.add(layers.Dropout(0.5))
    
    model.add(layers.Dense(512))    
    model.add(Activation("relu"))
    model.add(layers.BatchNormalization())
   
    model.add(layers.Dense(1, activation= 'sigmoid'))

    model.compile(loss = 'binary_crossentropy',
                  optimizer = optimizers.RMSprop(lr = 1e-4),
                  metrics = ['acc']
                  )
    if display_summary:
       model.summary()
    return model
```

## Batch Normalization results in Keras
The accuracy on validation boosted from 0.956 to 0.961 and training validation curves are also smoother.
![bn_keras]( https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/keras_bn_results.PNG)
This concludes the custom CNN training and we will move to transfer learning in next section where we will use a pre-trained network VGG16 and find out if we can further improve our model performance.

## Batch Normalization source code in Keras
- [kaggle]( https://www.kaggle.com/ajaykgp12/cars-keras-data-aug-dropout-bn?scriptVersionId=20617761)
- [guthub]( https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/keras/cars_keras_data_aug_drop_bn.ipynb)

## Batch Normalization in Pytorch
The implementation of Pytorch for batch implementation is shown below, only partial code is shown, refer to source code link to see full implemenation.
```python
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(in_channels= 3, out_channels=32, kernel_size= 3)
        self.bn1   = nn.BatchNorm2d(32)
        self.pool = nn.MaxPool2d(kernel_size=2, stride= 2)        
        
        self.conv2 =  nn.Conv2d(in_channels= 32, out_channels= 64, kernel_size= 3)
        self.bn2   = nn.BatchNorm2d(64)
        
       ..........................
       
       

    def forward(self, x):
        x = self.conv1(x) 
        x = F.relu(x)
        x = self.bn1(x)
        x = self.pool(x)
        
        x = self.conv2(x) 
        x = F.relu(x)
        x = self.bn2(x)
        x = self.pool(x)
        
        ................      
 
   ```
## Batch Normalization results in Pytorch
![pytorch](https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/resources/pytorch_bn_results.PNG)
- The accuracy is 0.965 which is marginal improvement over previous best score 0.965, which was trained with data augmentation only. Note that the training and validation curves are close to each other which means  our model will is not overfitting . This is kind of model I would want to put in production but our task is not finsihed yet, we will explore transfer learning in next section. 
- Note: We created model with batch normalization after relu activation function in our example. Training model with batch normalization before relu activation function resulted in better accuracy score of 0.97, but the training and validation curves were not smooth with unstable results, hence the model was discarded. You can check the [Kaggle](https://www.kaggle.com/ajaykgp12/cars-pytorch-aug-bn?scriptVersionId=20606746) script  for this model.
## Batch Normalization source code in Pytorch
- [Kaggle]( https://www.kaggle.com/ajaykgp12/cars-pytorch-aug-bn?scriptVersionId=20606325)
- [guthub]( https://github.com/ajayrawatsap/Identify-a-Car-Model-with-Deep-Learning/blob/master/pytorch/cars_pytorch_data_aug_bn.ipynb )

