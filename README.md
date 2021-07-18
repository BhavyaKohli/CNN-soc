##Basic Timeline: <br/>
Learning phase : 24/03/2021 — 24/05/2021<br/>
Application phase : 25/05/2021 — 14/07/2021<br/>
Results compilation and report submission : 14/07/2021 — 17/07/2021<br/>

##24/03 — 20/04 <br/>
During the learning phase, we were first referred to the Machine Learning course on Coursera (https://www.coursera.org/learn/machine-learning) taught by Andrew N.G. for a basic understanding of machine learning, loss functions, logistic regression, etc.
As the assignments in the course were on MATLAB, our mentors made 4 python assignments covering the first 4 weeks of the course. The assignments covered the aforementioned basic concepts of ML and also reinforced our python basics. Additionally, the first assignment had a tutorial on setting up git and about github as a good way to archive the work we do in an online repository.

##20/04 — 13/05 Midsems

13/05 — 24/05<br/>
After our (mentors' and mentees') midsems were finished we were referred to the Convolutional Neural Networks course on Coursera (https://www.coursera.org/learn/convolutional-neural-networks?specialization=deep-learning), also taught by Andrew N.G., which is course 4 of 5 in the Deep Learning specialization. We were instructed to cover material upto week 2 in this course within 10 days before starting on the actual project.

24/05 — 25/05<br/>
Our mentors had introduced us to the three projects early on, but now they asked us for our preferences to allot us to our desired project. Four of us including me were alloted 
to the COVID-19 Detection project under mentors Adarsh Raj and Gudipaty Aniket.

25/05 — 07/07<br/>
The four mentees in the project further split into two teams, and I was paired with Akshat Kumar. Then we decided to independently work on our models and then compare results.

Using the research paper shared with us as a reference (https://journals.physiology.org/doi/pdf/10.1152/physiolgenomics.00084.2020), I first began working on a binary classifier, which would classify a given image between COVID-19 positive and Healthy. The dataset was obtained from kaggle (https://www.kaggle.com/tawsifurrahman/covid19-radiography-database) and contained 21,165 chest X-ray images spanning 4 classes: Normal, COVID-19, Lung Opacity and Viral Pneumonia.
Some problems which I was faced with were overclocking available RAM and running out of GPU usage time on google colab. As a workaround for the GPU issue, I used two google accounts and for the RAM issue, I split up the total work into different sections: (Full) Dataset extraction, Creating usable datasets (Subsets of the full dataset), Loading the created datasets and finally, creating and training the model.

1. Dataset extraction and creating usable datasets — The dataset was downloaded from kaggle onto the default google colab directory (/content/) and after unzipping, I created functions which created subsets of the main dataset with around 3000 images each, including COVID-positive and healthy images. These subsets were saved as .npy files on my drive which I mounted onto the colab notebook.

2. Loading the created datasets — (After restarting runtimes) I created a function which would load these .npy files from the drive directory specified. This function included StratifiedShuffleSplit so the output was the train set (shuffled), test set(shuffled). This function had to be called everytime training had to be done and it used a lot of the available RAM due to which maximum of two datasets could be imported if the model needed to be trained afterwords without crashing.

3. Creating and training the model — Using a similar model architechture as given in the research paper, with some modifications such as adding a Dense layer after the Flatten layer, and replacing the "grouped convolution" with two separate Conv2D layers with a normalization layer between them. Grouped convolutions required the dataset to be of a particular form and that is why I chose to work without them. After the fist few runs, I was getting val_accuracy >90%. To improve on the results I experimented with a few different layer arrangements, but none worked better than the first. After hitting this block I thought of using the GaussianBlur function to blur the images before training, and so I integrated it into the load dataset function, passing the blur index as a function parameter. Running the model with the modified dataset yielded val_accuracies >95%. After some research I discovered the EarlyStopping callback and keras backend with which you can change learning rates without running the compile function. Using these, I was able to achieve a val_accuracy of 99.2% in a run. I saved this model to be safe in case the session crashed and reran its evaluation after restarting the session, and it gave the same 99.2% accuracy on its test set. Then to have a better idea about its results, I imported two other datasets and ran the model evaluation on them. This resulted in an average accuracy of 98.11% as reported. <br/>
After this I moved onto creating multi classifier which would classify a given image between three classes, COVID-19 positive, Viral pneumonia and Healthy. The create_dataset function was modified to include the third class and was renamed to create_dataset_multi. The function for loading the dataset was also modified to include the function to_categorical, which converted the labels numbered 0, 1 and 2 into one-hot vectors. After restarting the runtimes and running the load dataset function, it was time to train the model. The architechture of this model was also similar to the one described in the research paper with the same few changes like replacing grouped convolutions with two Conv2D layers, and adding a few Dense layers after Flattening. After a few tweaks I managed to get an accuracy of 96% after running the model evaluation. To have a better idea, I again imported other datasets and obtained an aerage accuracy of 96.1% as reported.

A problem faced during the training of the models was overfitting. After some research, I found out that Dropout is essential to prevent overfitting and hence I introduced a few Dropout layers whenever it seemed like the model was overfitting. 

Comparing with the research paper, the attained accuracy in the paper was 98.92% for the binary classification task and 98.27% for the multi classification task. And as seen above, the results obtained with my work are pretty close to the results in the paper.

Another method which I discovered recently, around last week was using dataframes and the ImageDataGenerator and flow_from_dataframe functions instead of the .npy files. This method was better than the previous in the fact that the images could be passed through some augmentations which would improve the general performance of the model, and using this method RAM usage was never a problem even after using the COMPLETE dataset (3616 COVID-19 positive and 4500 Healthy (undersampled)) for the binary classification task. Since I discovered this method late I couldnt do as much work in this but with it I could reach a maximum accuracy of 95% after running model evaluation with just a few (50) epochs. I feel that this method is better than the first, and I think that with proper augmentations, batch size selection, and other parameters its possible to achieve even higher accuracies given proper hardware to run training runs at higher speeds.

14/07 — 17/07<br/>
We compared results and made a common repository for our SoC team with all the three projects and their respective work in that repository.

I have learnt a lot from this project and I am grateful to my mentors who accepted my application. AFter working on this project I have become greatly interested in deep learning and neural networks and I plan to work on similar in the future, or even mentor a project of my own in later years.
