Summary:

This notebook follows the code for fine-tuning a gpt-2 model on the provided medical dataset to create a question-answering model.
I am implementing a GPT model from scratch (including all the modules like CausalAttention, MultiHeadedAttention and FFN) and then binding it all together in LightningAI and training it with the help of it. 
I am using Lance to load our training data. It is a modern columnar data format for ML and LLMs implemented in Rust. Lance is being used here as it is easily referenceable at different stages in the model and can highly efficiently load data during the training process. This approach involved pre-tokenizing the dataset, converting it into a PyArrow table and saving it in Lance format. It is recommended that the notebook is run on google colab.

Data Preprocessing:
The assumptions made in the training data are that capitalisation, punctuation, and stop words hold significance in the answers as the absence of them can change the meaning of the answer. 
1. Missing values are eliminated
2. Some of the answers contained urls which are irrelevant to answering the questions from a textual perspective, hence they are removed along with extra spaces present in the data.
3. The question and answer columns of the data are combined such that the gpt-2 model can process it more efficiently and generate a better accuracy.
4. After applying the following modifications, a split into training and testing datasets is conducted.

Model Creation:
The different stages of a gpt model are described in different cells of the notebook such as causal attention heads, blocks, and the loading of the dataset. Usage of Lance during the dataset loading and Lightning during the training makes the process faster and more efficient. 

Model Training:
Model Training on the provided dataset takes about 2 hours to complete using the T4 GPU from the free tier of google colab. 

Model Evaluation:
The ROUGE score is chosen as an evaluation metric. To be more precise, the F1 score generated from the ROUGE-L for the following reasons:
1. Focuses on Semantic Similarity.
2. High degree of factual accuracy and importance for structure
3. Less Sensitive to Stopwords
While ROUGE-L has a limited assessment of fluency compared to other metrics, a medical question answering model should give more importance to factual accuracy. Although Human Evaluation would be the best metric to judge a question-answering model, the f1 score of the ROUGE-L is a strong contender.

Model Performance:
The model however did not perform satisfactorily as looked at by the rouge score but the prompts gave distinct believable answers as seen in the code. 

Potential Improvements:
Potential improvements that can be done to the model for better answers include:
1. Providing the model with context, during training and evaluation.
2. Static prompting: giving the model random prompts from the dataset before entering the evaluation prompt to set some context.
3. Dynamic prompting: giving the model prompts that are related to the evaluation prompt from the dataset before entering the evaluation prompt to set better context.