# twitter-sentiment-analysis



<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a name="readme-top"></a>

[![Contributors][contributors-shield]][contributors-url]
[![LinkedIn][linkedin-shield]][linkedin-url]


<br />
<div align="center">
  <h3 align="center">Twitter sentiment analysis</h3>
  <p align="center">
    Twitter sentiment analysis using BERT contextual embeddings
    <br />
    <a href="https://github.com/MajdAlkawaas/twitter-sentiment-analysis/issues">Report Bug</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->

## Table of Contents
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#Installation-instructions ">Installation instructions </a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage-instructions">Usage instructions</a></li>
    <li><a href="#Method-and-approach">Method and approach</a></li>
    <li><a href="#Data">Data</a></li>
    <li><a href="#Results">Results</a></li>
    <li><a href="#Future-work">Future work</a></li>
    <li><a href="#Contributing">Contributing</a></li>
  </ol>



<!-- ABOUT THE PROJECT -->
## About The Project

This is a sentiment analysis project for tweets using BERT model and a custom classification layer.
In the following readme file we are going to explain our approach, data and results.
We are also providing a discussion regarding our results.
If you have any ideas or comments you would like to share with us our contact information are in our profiles.



### Built With

<a href="#"> <img width ='32px' src ='https://raw.githubusercontent.com/rahulbanerjee26/githubAboutMeGenerator/main/icons/python.svg'> </a>
<a href="#"> <img width ='32px' src ='https://raw.githubusercontent.com/rahulbanerjee26/githubAboutMeGenerator/main/icons/pytorch.svg'> </a>
<a href="#"> <img width ='32px' src ='https://raw.githubusercontent.com/rahulbanerjee26/githubAboutMeGenerator/main/icons/scikit.svg'> </a>
<a href="#"> <img width ='32px' src ='https://huggingface.co/front/assets/huggingface_logo-noborder.svg'> </a>

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- Installation instructions  -->
## Installation instructions 

### Prerequisites
Create a python 3.8 virtual environment: ``` python3 -m venv <name_of_virtualenv>``` <br>
Activate the virtual environment: ```source new_env/bin/activate``` <br>
Install the standard Data science tools for python. <br>

Install manually
```
pip install nltk==3.7
pip install torch==1.12.1
pip install transformers==4.22.2
```

### Installation
Please follow the below provided steps

1. Clone the repo
   ```sh
   git clone https://github.com/MajdAlkawaas/twitter-sentiment-analysis.git
   ```
2. Download the datasets
    1. Use the following <a href="https://drive.google.com/file/d/19XcCpR67zhhojsPXDaW4jXDOfL2RIcYj/view?usp=sharing">link</a>. 
    2. Extract the file in the same directory as the data_preprocessing.ipynb notebook
3. Open the data_preprocessing.ipynb notebook 
    1. For downloading the jupyter notebook follow the instruction 
   <a herf="https://docs.jupyter.org/en/latest/install/notebook-classic.html">here</a>


<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- Usage instructions -->
## Usage instructions

After following the download instructions you can start with the following:
1. Adjust the `BASE_PATH` variable in the data_preprocessing.ipynb notebook to the path of the home directory where you cloned the repo.
2. Run the code in the data_preprocessing.ipynb notebook.
3. After running all the code cells in the mentioned notebook you will have a new .csv file with the name `preprocessed_dataset.csv` stored in the `BASE_PATH` you have chosen earlier.
4. Close the data_preprocessing.ipynb notebook
5. open the training.ipynb notebook.
6. Change the value of `BASE_PATH` to the same value used earlier
7. Choose a value for the `EPOCHS` 
8. Choose a value for the `portion` which controls that percentage you are using from the total size of the dataset.
9. Optional: pass a value for the parameter `frozen` when initiating the `BertClassifier` object this, parameters controls whether the BERT's parameters are frozen or not. (`frozen = True` BERT parameters are not being trained only the classification layer) Default value `frozen = False`
10. Optional: pass a value for the parameter `dropout` for the dropout layer, default value `dropout = 0.5`



<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Method and approach -->
## Method and approach
We were trying to figure out what kind of results we will get by training BERT with a custom classification layer on multiple different small amounts of data and see how much the performance is going to improve with the extra data.

1. We started with four dataset that we will go further about in the data section
2. Performed data preprocessing which included:
   1. Labels encoding
   2. Dropping duplicates and Nulls
   3. Removing stopwords (NLTK English stopwords list)
   4. Removing links, symbols ...etc (Using Regex expressions)
   5. Stemming (Using porter stemmer from NLTK)
3. Fine tuned a pre trained BERT model from huggingface's transformers library (bert-base-cased)
4. Added a custom classification layer which consisted of:
   1. Dropout layer
   2. linear layer
   3. Sigmoid

<br>

<figure>
<img src='model_diagram.jpg' alt="model diagram" style="width:100%">
<figcaption align = "center"><b>Diagram of our model architecture</b></figcaption>
</figure>

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- Data -->
## Data

We have combined multiple datasets to create a dataset of 1.76 Million tweets.
We have used the following datasets:
1. <a href="https://www.kaggle.com/datasets/kazanova/sentiment140">Sentiment140 dataset</a> (1.6 million tweets)
2. <a href="https://www.kaggle.com/datasets/crowdflower/twitter-airline-sentiment">US Airline Sentiment</a> (14.6K tweets)
3. Professors dataset (465 tweets)
4. <a href="https://www.kaggle.com/datasets/saurabhshahane/twitter-sentiment-dataset">Twitter Sentiment Dataset</a> (163K tweets)

we are using a train 75%, val 12.5%, test 12.5% split

### Issues with the datasets:

1. Each dataset has a different label encoding for example the Sentiment140 encodes the positive, neutral, negative classes as 4, 2, 0 respectively. This issue required unifying the label encoding through out the four datasets.
2. Lack of tweets within the neutral class, the sentiment140 dataset documentation claims that it includes a neutral class tweets but no such tweets were found upon inspection. The US Airlines Sentiment dataset contained some tweets with the neutral class.
3. Removal of the neutral class from the merged dataset, given the relatively small number of neutral tweets (14 times less than either of the other two classes) the neutral class has been removed given that the model will most like sees those tweets as noise.


The total number of tweets after removing the neutral class and nulls, Nan is
1.7 Million tweets.
<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Results -->
## Results

We have trained the model multiple times using different amounts of data and hyperparameters the following table provides a summery.

| Epochs | Train data size | Train accuracy | Validation accuracy | Test Accuracy | F1 Score |
| ------ | --------------- |--------------- | ------------------- |-------------- | -------- |
| 4      |0.1              |0.850           | 0.779               | 0.770         | 0.77     |
| 4      |0.01             |0.895           | 0.730               |0.732          | 0.758    |
| 4      |0.05             |0.901           | 0.751               | 0.748         | 0.758    |

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Discussion -->
## Discussion

Our results and approach suggests some questions that require a discussion

### How much of this extra training or data made a difference?
As we can see from the first row of the results that the model started to overfit during the forth epoch nonetheless, it achieved the highest F1 score with 1% percent of the training data. The second and third training instances achieved similar results with 10 times or 2 times less data. These results suggests that for this specific model more data may not result in better results.

### Is the architecture of classification layer effective?


### Is BERT base the right way to do it?

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- Feature work -->
## Future work
- Test different classification layers (such as BiLSTM).
- Detach the classification layer of the RoBERTa-sentiment-base (from huggingface) and try other classification layers with fine tuning.
- Conduct further analysis of the performance metrics to make an informed decision regarding the current model
<p align="right">(<a href="#readme-top">back to top</a>)</p>




<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b <branch-name>`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin <branch-name>`)
5. Open a Pull Request

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/MajdAlkawaas/twitter-sentiment-analysis.svg?style=for-the-badge
[contributors-url]: https://github.com/MajdAlkawaas/twitter-sentiment-analysis/graphs/contributors
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/in/majdalkawaas/
[product-screenshot]: images/screenshot.png
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com