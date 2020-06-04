# Customer Question Answering Based on User Aggrements
  This project is an example of question answering based on BERT transformer. This was prepared for my "deep learning for natural language processing" course. 
  
  Before starting to explain what has been done in this project concept of transfer learning is needed to mention. Transfer learning focuses on storing knowledge gained while solving one problem and applying it to a different but related problem. As my professor frequently says "No need to invent the wheel again", this project uses already built transformers. In order to acces transformers that I used and others you need to install [Huggingface Transformers](https://github.com/huggingface/transformers). Installing them is fairly simple. Also for this project I have used Google Colab since it is free to use easy access to high performing machines. (In this project I mostly benefited from their high speed internet connection :) )
  
```markdown
!git clone https://github.com/huggingface/transformers
%cd transformers
!pip install .
```
After installing transformers I am going to import the modules I need.

```markdown

`from transformers import  pipeline, QuestionAnsweringPipeline, AutoTokenizer, AutoModelForQuestionAnswering, AutoModel, AutoModelWithLMHead`

```



## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/FastXr/customer_helper/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/FastXr/customer_helper/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
