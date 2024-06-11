# NLPMedicalAnalyzer
Created by: Sasha Ilinskaya and Hamza Magsi

### Abstract:
The Medical Analyzer was created to help doctors diagnose their patients more accurately by providing a list of possible diseases or illnesses the patient may have based on patients’ own words. The Medical Analyzer accomplishes this by providing doctors with a percent likelihood of potential diseases in a list format along with the original user input, extracted symptoms, similar symptoms, and the final list of symptoms on which the model makes its predictions. The tool uses Named Entity Recognition (NER) to successfully find symptom-related phrases and words in the patient input. It then uses a Logistic Regression classifier model (the best model for this task compared to two others) to make predictions. Our model has a 90.61% accuracy and a 70.5% F1 score for predicting the correct disease based on a list of symptoms. When running the model against real user symptom description we were able to extract relevant symptoms and make accurate predictions, having as high as a 66.89% likelihood for a disease. These results are a show that the project is headed in the right direction. If this tool is further developed by adding subtle symptom recognition as well as a conversation-style dialogue between patient and the Medical Analyzer, this tool can become a useful addition to the consultation process between patients and doctors in the real world.

### Problem description
Describing symptoms can be difficult for patients, and patients may struggle to provide all the relevant information to their doctor when talking in-person. Meanwhile doctors may misdiagnose their patient if not enough information or not enough relevant information is provided by the patient. This can have big implications on patients’ health. 

The Covid-19 Pandemic has shown us that there are not enough doctors to treat all the patients, much less give every single one the care they deserve. So to mitigate problems such as misdiagnosis and relieve doctors from the pressure of having many patients, we’ve created the Medical Analyzer. In the real world this tool (with proper improvements) can help doctors be more efficient in their consultations while the patients can feel more comfortable providing the right information.    

The Medical Analyzer addresses important needs, including: 
1) Provide doctors with useful information about the patient that they may not have otherwise learned about. 
2) Acts as an at-home consultation that can help the patient think deeper about their symptoms and have an idea how to describe them. It will also provide a smoother experience for the patients as they will be more prepared to explain their symptoms or be reminded of symptoms they may have forgotten about by the time their visit comes around. 
3) Having an at-home assistant available to patients that has no judgment over the patient would help mitigate possible wrong diagnoses or mis-prescribed medications. 
With this tool there is an ethical consideration to uphold is to ensure that users may only use this tool as part of their primary care and should not use it to self-diagnose.


### Related Work: Relevant literature for your project.
BioBERT: PyTorch-based BioBERT implementation; BERN: Web-Based biomedical NER + normalization using BioBERT; BERN2: Advanced version of BERN with NER from BioLM + NEN from PubMedBERT; covidAsk: BioBERT based real-time question answering model for COVID-19 [BioBERT GitHub]

### Approach

The Medical Analyzer works as 2 parts: Data-Preprocessing and Classification. 
Data-Preprocessing:
The Medical Analyzer intakes user symptoms as a natural conversation. In NER task, the Medical Analyzer cleans the input by: removing capitalization, rewriting contractions, removing punctuation, and correcting spelling. 
An example input could be: 
“I have been having migraines and headaches. I can't sleep. My whole body is shaking and shivering. I feel dizzy sometimes.”
After normalization: 
'i have been having migraines and headaches i cannot sleep my whole body is shaking and shivering i feel dizzy sometimes'
The Medical Analyzer is able to successfully extract symptoms:  
['migraine', 'headache', 'cannot', 'sleep', 'shaking', 'shivering', 'dizzy', 'fever']. 
After extracting these symptoms, the Medical Analyzer creates a list of synonyms of relevant symptoms - the total symptoms list is as follows: 
['throw_off', 'migraine', 'kip', 'shake_off', 'judder', 'lightheaded', 'headache', 'hemicrania', 'sleep', 'sick_headache', 'sway', 'head_ache', 'light-headed', 'megrim', 'didder', 'quivering', 'dizzy', 'quiver', 'fever', 'cephalalgia', 'escape_from', 'silly', 'throb', 'woozy', 'palpitation', 'vexation', "catch_some_Z's", 'trembling', 'rest', 'eternal_rest', 'shake_up', 'febrility', 'chill', 'empty-headed', 'stimulate', 'vertiginous', 'eternal_sleep', 'shakiness', 'nap', 'featherbrained', 'pyrexia', 'shaking', 'giddy', 'shudder', 'rock', 'quietus', 'shivering', 'airheaded', 'stir', 'thrill', 'sopor', 'febricity', 'concern', 'agitate', 'shaky', 'excite', 'shiver', "log_Z's", 'worry', 'vibration', 'slumber', 'shake', 'feverishness']
Lastly, the total symptoms list is compared against the symptoms found in the dataset. If a symptom from the total symptoms is found within the dataset of symptoms, then it is added to the final symptoms list. This list of symptoms can be used in the classification task to output a list of diseases for the given symptoms. 
Final symptoms list: 
['chill', 'shivering', 'shaking', 'headache', 'fever', 'shakiness']
Classification:
	In order to find the best approach for our classification task we tried three models: Multinomial Naive Bayes, Support Vector Machine, and Logistic Regression. The actual model creation and fit were easy since we used the sklearn library to import MultinomialNB, SVC, and LogisticRegression which made the train, fit, and prediction very straight-forward. From there we used Accuracy and F1-score metrics to see which model is best for this task and found Logistic Regression to be best. 
	We then applied the Logistic Regression model in the Medical Analyzer by converting the final symptoms list into a binary vector. This vector is used in predicting the most likely diseases from the given symptoms. We then output a list of potential diseases with percentage likelihood of each disease based on the given symptoms. 
When the model compares the binary vectors it calculates the percentage likelihood based on their similarity. So it becomes 0% if there’s no overlap and more than 0% otherwise.

### Experiments

For our implementation we used a dataset that was already preprocessed from the following github repository by Anand Sharma, Nikunj Agarwal and Rahul Maheshwari. This dataset contains a list of symptoms as features and diseases as labels. The data itself is represented as binary vectors where 1’s represent that the symptom is present for that disease. 
https://github.com/rahul15197/Disease-Detection-based-on-Symptoms?source=post_page-----54e6be60a3d1--------------------------------
This dataset was adapted from:  https://people.dbmi.columbia.edu/~friedma/Projects/DiseaseSymptomKB/index.html
Our main preprocessing was done on user input which was the primary NLP problem. The user input is normalized by lower casing, expanding contractions, and fixing spelling if there are any errors. This normalized input is then put through the NER model to find relevant terms to use in the classification model.

We used a couple of different inputs from the following dataset and generated by ChatGPT to extract the relevant symptoms and run them through the classification model. Our main concern was that the right words were extracted to base the diagnosis on. 
https://www.kaggle.com/datasets/dsxavier/diagnoise-me?select=en_medical_dialog.json 

#### Metrics
For the classification models we used Accuracy and F-1 score to see how well a disease was predicted based on relevant symptoms provided. We found that the best model for us to use was Logistic Regression with an average of 90.61% accuracy and F1 score of 70.5%. We also tried Multinomial Naive Bayes and Select Vector Machine, both had a lower F1 score and slightly lower accuracy.

### Conclusion

One of the main ideas of this project is to create a  natural dialogue between a patient and the Medical Analyzer. So a future development would be to add a response feature where the Medical Analyzer can ask further clarification on symptoms as well as provide some remedies the patient can do at-home to relieve some symptoms (holistic medicine style rather than prescription-based). We considered this dataset to help with that: https://www.kaggle.com/datasets/tusharkhete/dataset-for-medicalrelated-chatbots
To further improve the symptom deduction we would also want to add recognition of subtle symptoms or vague descriptions. Currently our model is able to identify single-word symptoms such as “headache” or “weakness.” Ideally we want to extract phrases such as “my stomach hurts” and use the two-word symptom “stomach hurts” as part of the final list of symptoms. Along with that, we had an idea to use the Levenshtein Distance in data-preprocessing to find the most closely matching symptoms in our database. This however backfired as we had more matches than expected which overfit the final vector causing there to be more false positives and much lower percentages for disease prediction. For future this can be improved by raising the threshold for the Levenshtein Distance as well as filtering out irrelevant symptoms beforehand. 
Another idea we had was related to word forms. Some words such as “sleep” and “sleeping” and “sleepy” have the same root and refer to a similar idea. We wanted to implement the opposite of a stemmer to generate variations of a word to better grasp all symptoms related to root “sleep.”  We found only one available library – reversestem – but it failed to do a proper job, generating word variations such as “sleepful” and “sleepe.”

### References

The bulk of our classification model comes from the research project done by Anand Sharma, Nikunj Agarwal and Rahul Maheshwari. Their project already contained the necessary classification we wanted from symptoms to diseases as well as the dataset we ended up using.  
https://rahul-maheshmaheshwari.medium.com/disease-detection-based-on-symptoms-with-treatment-recommendation-with-scrapped-data-set-54e6be60a3d1
https://github.com/rahul15197/Disease-Detection-based-on-Symptoms?source=post_page-----54e6be60a3d1--------------------------------
We got the idea of using NER and considering the use of BERT through the following article on Deep Language Model for Symptom Extraction from Clinical Text. We found that there are already models and structures out there that can determine symptom phrases or symptom words for us after the necessary preprocessing. This was key in our model creation because we want the patients to write about their symptoms however they feel most comfortable while we do the work of matching their description to an actual symptom. 
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9074854/
While we didn’t use the BERN2 model, we did find it helpful in our vision of the project and what we wanted to aim for as well as give us an idea of what already exists out there for medicine-related NLP projects and how we could add to the pool. 
http://bern2.korea.ac.kr/
This github provided insight into industry standard applications such as BioBERT, BERN2, and CovidAsk. This applications gave insight into incorporating or simply learning about such methodologies as NER, BERT, and Levenshtein Distance.
https://github.com/dmis-lab/biobert 
The original dataset used by Anand Sharma et.al. to create the vector-based dataset.
https://people.dbmi.columbia.edu/~friedma/Projects/DiseaseSymptomKB/index.html


