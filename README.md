# Customer Question Answering Based on User Aggrements
This project was built for a my **Deep Learning for Natural Language Processing** course in **Istanbul Sehir University**


Under supervision of **ASST. PROF. MEHMET SERKAN APAYDIN**

  This project is an example of question answering based on BERT transformer. 
  
  Before starting to explain what has been done in this project concept of transfer learning is needed to mention. Transfer learning focuses on storing knowledge gained while solving one problem and applying it to a different but related problem. As my professor frequently says "No need to invent the wheel again", this project uses already built transformers. In order to acces transformers that I used and others you need to install [Huggingface Transformers](https://github.com/huggingface/transformers). Installing them is fairly simple. Also for this project I have used Google Colab since it is free to use easy access to high performing machines. (In this project I mostly benefited from their high speed internet connection :) )
  
```markdown
!git clone https://github.com/huggingface/transformers
%cd transformers
!pip install .
```
After installing transformers I am going to import the modules I need.

```markdown

from transformers import  pipeline, AutoTokenizer, AutoModelForQuestionAnswering 
```

Model and Tokenizer that I used. I downloaded a specific model that fits my purpose which are finetuned model in Stanford Question Answering Dataset (SQuAD). Which is a pretty similar task that I try to implement so no further implementation for the model is needed. Also in transformers pipelines are implemented basically you pass first paramater as the task that you want to perform that if you want you can specify the model and tokenizer, in this case that's what I am doing.
```markdown
tokenizer = AutoTokenizer.from_pretrained("bert-large-uncased-whole-word-masking-finetuned-squad")
model = AutoModelForQuestionAnswering.from_pretrained("bert-large-uncased-whole-word-masking-finetuned-squad")

nlp_qa = pipeline('question-answering', model=model, tokenizer=tokenizer)
```
For further reading;
[BERT Model Paper](https://arxiv.org/pdf/1810.04805.pdf)
  
At this point in order evaluate the performance of the model. I used Microsoft's Machine comprehension dataset.
[MCTEST Paper](https://www.aclweb.org/anthology/D13-1020.pdf)
[MCTEST Dataset](https://github.com/mcobzarenco/mctest/tree/master/data/MCTest)

After doing some preprocessing the dataset to perform evaluation. Dataset looks like as following;

```markdown
# Context
Alyssa got to the beach after a long trip. She's from Charlotte. She traveled from Atlanta. She's now in Miami. She went to Miami to visit some friends. But she wanted some time to herself at the beach, so she went there first. After going swimming and laying out, she went to her friend Ellen's house. Ellen greeted Alyssa and they both had some lemonade to drink. Alyssa called her friends Kristin and Rachel to meet at Ellen's house. The girls traded stories and caught up on their lives. It was a happy time for everyone. The girls went to a restaurant for dinner. The restaurant had a special on catfish. Alyssa enjoyed the restaurant's special. Ellen ordered a salad. Kristin had soup. Rachel had a steak. After eating, the ladies went back to Ellen's house to have fun. They had lots of fun. They stayed the night because they were tired. Alyssa was happy to spend time with her friends again.
# Questions
Question1: What city is Alyssa in?
Correct Answer: Miami
['trip', 'Miami', 'Atlanta', 'beach']

Question2: Why did Alyssa go to Miami?
Correct Answer: visit friends
['swim', 'travel', 'visit friends', 'laying out']

Question3: How many friends does Alyssa have?
Correct Answer: 3
['1', '2', '3', '4']

Question4: What did Alyssa eat at the restaurant?
Correct Answer: catfish
['steak', 'soup', 'salad', 'catfish']
```
In brackets we can see multiple choices that dataset provides to us. The model doesn't depend on these choices however I am going to make use of them.

Pipeline is being run as following, first you provide a context in our case these are short stories then you ask a question.
```markdown
nlp_qa(context='Hugging Face is a French company based in New-York.', question='Where is based Hugging Face ?')
# Output 
{'answer': 'New-York.', 'end': 50, 'score': 0.985049841952879, 'start': 42}
```
This is a fairly simple and obvious example but let's see how is the model performing with the MCTEST dataset.
```markdown
Question1: What city is Alyssa in?
Correct Answer: Miami
['trip', 'Miami', 'Atlanta', 'beach']
Model Answer: Miami.
Question2: Why did Alyssa go to Miami?
Correct Answer: visit friends
['swim', 'travel', 'visit friends', 'laying out']
Model Answer: to visit some friends.
Question3: How many friends does Alyssa have?
Correct Answer: 3
['1', '2', '3', '4']
Model Answer: Kristin and Rachel
Question4: What did Alyssa eat at the restaurant?
Correct Answer: catfish
['steak', 'soup', 'salad', 'catfish']
Model Answer: catfish.
```
As it can be observed, first and last questions are exact answer third one is wrong and second one is a similar answer. Since I wanted to accept this similar answers to correct and evulate the performance better. I got model output and used Gestalt Pattern Matching algorithm to find most similar choice to choose. Then I evaluated models performance. At this point I realized that it's not likely my model to solve questions like the third one. However I still evaluated them as mistakes eventhough they were even hard for humans. 
Further reading;
[Gestalt Pattern Matching](https://www.sciencedirect.com/science/article/pii/S0019995861800491)

In the end I got **70%** accuracy with this and I was satified with the result.

Then I got [Netflix Terms of Use] (https://help.netflix.com/en/legal/termsofuse) then passed it as a context to the model and asked some questions.

```markdown
# Question: 
When can I cancel Netflix?
# Answer: At any time

# Question: 
When should I cancel Netflix?
# Answer: Before your billing date
```
These questions are amazingly accurate since I can cancel my Netflix account my any time but I would like to cancel it before my billing date to not pay another month's bill.

I wanted to try another case
```markdown

# Question:
How old should I be to use Netflix?
# Answer: 18

# Question:
What happens if I am not older than 18 ?
# Answer: Minors may only use the service under the supervision of an adult

```
First question is to learn what is the age restriction for Netlix since not all content are fitting for everyone but some are. Then I asked another question since I may want niece to have an account and watch some cartoons. Model accurately answers my second question as well. It tells me that minors can use it but they need the supervision of adult. 

Up untill this point I am satisfied with the performance of the model. I want to do another quick test for age restriction for Twitter.
[Twitter Terms of Service] (https://twitter.com/en/tos)

```markdown
# Question: 
How old should I be to use Twitter?
# Answer: 13 years old

# Question: 
Can I use Twitter if I am not older than 13?
# Answer: You must be at least 13 years old
```
Model succesfully answers these questions as well. As a person needs to be 13 years old to use twitter, if they are younger than this they can't use it. This model's further usage can be studied and it may be applicable for some contract related questions or may be customer question answering tool.


