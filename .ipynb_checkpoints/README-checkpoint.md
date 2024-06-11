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
