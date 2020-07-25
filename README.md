Dataset is not shared due to license issues.

Data Description for reference:
There are 258 classes. Of these 258 classes 64 classes have less than 4 samples and 13 classes have more than 4 samples or maximum 10 samples. 

Division of dataset into train, test, validation set:
Since there are many classes with only 1 sample, it is not possible to have not augmented or uncommon samples in train, test and validation sets.
Hence, I divided classes with samples greater than 10 in the ratio of:
train: 0.8
test: 0.1
validation: 0.1 
(For this ratio the minimum number of samples in class should be 10).

For classes with number of samples between 4 to 10, I divided classes in the ratio of:
train: 0.4
test: 0.3
validation: 0.3
(I wanted to divide in equal ratio i.e 0.33 each. Hence, approximating. Minimum number of samples in each class should be 4 for this.)

For remaining classes, with samples less than 4, I kept all samples in all three sets.

I have used the split_folders library for splitting data to train, test and validation set.

Finally, I have used distort image augmentation technique using Augmentor library.
I felt this would give a crumpled effect to packets. Very light settings have been used for this purpose. Finally each class is having 204 samples  for training.

Review:
While going through different trained models on Imagenet, I decided to use InceptionResnet2 as it had the highest accuracy. But after using the validation loss range was very high compared to training loss. 
Hence, I inferred this network is very big for this. Next I tried InceptionV2. I faced the same problem with it with little less difference. Finally I traded off accuracy a bit and went with mobilenetV2. 
I added a dense layer of 1024 with 0.3 dropout followed by a dense layer of 258 with softmax activation. I used Categorical Cross entropy loss. This gave me training accuracy of 99.7% and validation accuracy of 96.48%.

The input for this was in shape (224, 224,3).

But the confusion matrix precision and recall were extremely bad. Hence, I decided to train a triplet loss on this model. What triplet loss does is it separates the embeddings of each class in embedding space to form clusters. I started with AdrianUng implementation. 
In this implementation its loading the entire data. My system hardware is not supporting it.I tried with google colab. It takes me entire day aprrox. 18 hours to upload the data and soon after they restrict me from using the GPU. 
Hence, I thought with training with 1/4th the data and small batch size. But I am getting Nan loss as of now with this. I will try fixing this in the coming days.

Things planning to try next:
Write a custom generator to train on triplet loss.

References:

https://github.com/abhinavsagar/Grocery-Product-Classification/blob/master/grocery2.ipynb

https://augmentor.readthedocs.io/en/master/code.html

https://towardsdatascience.com/deep-learning-unbalanced-training-data-solve-it-like-this-6c528e9efea6

http://openaccess.thecvf.com/content_cvpr_2017/papers/Karlinsky_Fine-Grained_Recognition_of_CVPR_2017_paper.pdf

https://arxiv.org/pdf/1611.05799.pdf

https://github.com/AdrianUng/keras-triplet-loss-mnist
