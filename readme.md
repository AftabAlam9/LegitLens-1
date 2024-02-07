# Legit Lens :computer: :newspaper:
 <p align="center"> 
 <img src="https://github.com/HVbajoria/LegitLens/assets/72864242/6e59a051-435c-47c1-91e8-7e80685a9874" width="250" alt="Logo" > 
   </p> 
 </br> 
  
 ## Website Link: https://legitlens.streamlit.app/
  
 ## Details : 

 **Team Name** : Innovation Redefined

 **Mentor & Guide**: Dr. Pronaya Bhattacharya

 **Member Details**: 
 * **Member 1** : Arghdeep Banerjee (A91005220030)
 * **Member 2** : Harshavardhan Bajoria (A91005220073)
 * **Member 3** : Aftab Alam (A91005220092)
 * **Member 4** : Esita Budia (A91005220104)
  
 **College Name** : Amity University Kolkata 
  
 **Graduation Year**: 2024 
  
## Theme :  
The proliferation of fake news undermines the integrity of information, erodes public trust, and poses a significant threat to democratic processes. Developing a robust fake news detection tool is imperative to safeguard the accuracy of news sources, protect public discourse, and mitigate the societal consequences of misinformation in the digital age.

## Problem Statement:
Misinformation is everywhere, in social media, websites, and even face-to-face. It’s hard to know what to believe, and even harder to figure out what’s true.

Misinformation has severe consequences. It can lead to health scares, like the anti-vaccination movement. It can cause political instability, like the 2016 U.S. Presidential Election. It can even lead to murder, like the case of the Pizzagate conspiracy theory.

Existing solutions have failed due a number of reasons:
* Lack of accurate, up-to-date data: Most fact-checking organizations rely on manually collected data, which is often out-of-date or incomplete.
* Lack of centralized knowledge base: There is no central repository of fact-checked content, which makes it difficult to track the spread of misinformation and to identify new instances of misinformation.
* Lack of platform coverage: Most fact-checking organizations only operate on one platform (e.g. Facebook), making it difficult to reach users on other platforms (e.g. WhatsApp).
* Lack of scale: Fact-checking organizations are often small, with limited resources. They can only fact-check a limited number of claims.
* Lack of language coverage: Most fact-checking organizations only operate in one language, making it difficult to reach a global audience.

 ## Tech Stack: 
 The following tech stacks have been used to create the application and deploy it.   
* **Streamlit** to build the front end of the application. 
* **Streamlit Cloud** to deploy the application for global access. 
* **Microsoft Azure Blob Storage** to store the training and testing dataset along with hosting images. 
* **Microsoft Azure ML Studio** to build and test Machine Learning models for fake news detection.
* **NLP and ML Models** to extract the relevant features, keywords and build the prediction model. 
* **Python** to build the logic, and the backend of the application. 
* **GitHub** to host the source code, use the version control (collaboration history) to understand the changes, and go back and forth if required to complete the software.
* **GitHub Codespaces** For quick and easy building and deployment of the software using in-browser VS code.

## Methodology

### Training and Testing Model
<img src="https://github.com/HVbajoria/LegitLens/assets/72864242/ee92ea6b-0c67-4505-959b-3d4d1d8756ed" width="1050" alt="Training and Testing Model" > 

## The Application Architecture
<img src="https://github.com/HVbajoria/LegitLens/assets/72864242/d0d92675-3996-42cf-9bd5-064cfebddfa0" width="1050" alt="Application Architecture" > 

## Overview of the model: 
*  Basics
    *  For each news article or site, we extract **features** from the **URL** and **HTML**, such as, a vector describing the domain extension of the URL.
    *  We feed this into a logistic regression model, and out comes the result.
*  Data
    *  The data was taken from news sites around the world, containing `2002` examples in the training data, and `309` in the testing data.
    *  `52.2%` of the training data is fake, while `54.4%` of the testing data is fake.
    *  For testing, we are only provided the URL of a site and its HTML content.
*  Features
    *  At no point do we actually determine if the content is legitimate. We only ever employ statistical methods to determine if the site appears fake or not. In other words, if a particularly rude person looked at our code, they could easily write an article that is superficially real, to our model. Luckily, you, dear reader, would never do such a thing.
    *  Domain extension.
        * The domain extension is what comes after the domain name.
        *  The domain extension for `https://github.com/HVbajoria/LegitLens` is `.com`.
        *  The domain extension for `https://en.wikipedia.org` is `.org`.
        *  There are a total of 22 different domain extensions in our training data: `.com`, `.org`, `.edu`, `.net`, `.co`, `.nz`, `.media`, `.za`, `.fr`, `.is`, `.tv`, `.press`, `.news`, `.uk`, `.info`, `.ca`, `.agency`, `.us`, `.ru`, `.su`, `.biz`, and `.ir`
        *  Note that 3 domain extensions: `.il`, `.me`, and `.au` only appeared in the testing data and not the training data, so when they appear, we have nothing to base our judgement on, and we simply count them as incorrect when later testing with just these features.
        *  Also (for fun), we can do some analysis on the domain extensions. The domain extensions for every government (`.au` for Australia, `.ca` for Canada, `.fr` for France, `.il` for Israel, `.ir` for Iran, `.is` for Iceland, `.nz` for New Zealand, `.ru` for Russia, `.su` for the Soviet Union, `.us` for the United States, and `.za` for South Africa) except for `.uk` for the United Kingdom, have not a single real article. This might be some error, or maybe not.
        *  We create individual features for the most common extensions—`1` if the URL ends in an extension and `0` otherwise. This is similar to a one-hot encoding.
        *  Alone, this accurately predicts the validity of the testing data `53.1%` of the time.
*  Keywords.
    *  This feature is similar to a bag of words, where we count the number of certain keywords in the HTML body text.
    *  The keywords were determined by analyzing the training data. Specifically, we counted the words in real and fake news, weighted by if it was real or fake.
    * For example, if the word `apple` (this can't be controversial, right?) appears `10000` times total in some number of real sites, but only `100` times total in some number of fake sites—where we have the same number of real and fake sites, for simplicity—then our total count is `-9900`. This is very negative, so we add that as a keyword.
    *  We then normalized the count with the function 
    $\\log(1+\\#)$.
* Issues.
    *  Of course, this is limited by the fact that we can not count word pairs or n-grams in general.
    *  Furthermore, fake news is generally around twice as long as real news, so that is another issue.
    * For example, the letter `u` is strongly correlated with real news, while `president` is strongly correlated with fake news.
    * By itself, this had an accuracy of `89.6\%`.
*  URL word count.
    *  Some sites, such as `yahoo.com` or `cnn.com` have few (or no) words in them.
    *  Some sites, such as `redflagnews.com` or `consortiumtimes.com` have several.
    *  We hypothesize that real sites have fewer words while fake sites have less.
    *  We count the number of words in the following manner.
    *  Start from the second letter. Keep going to the next letter until you find that all the letters up to that letter form a valid word (according to nltk vocabulary).
    *  Once you find a valid word, increment the number of words by `1`, and start again, this time two letters after the end of the word. Repeat.
*  Bag of Words.
    *  We count the number of each of a set of the `300` most common words in the description of the website, and create `300` features.
*  GloVe.
    *  We use the average GloVe word embeddings of the description of the website as a list of several features.
    *  Lengths (of URL and HTML).
    *  We calculated the lengths of the URL and HTML.
