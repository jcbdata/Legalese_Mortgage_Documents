
---

# Deconstructing Legalese: understanding the complex language of legal agreements


## Introduction & Problem Statement:

In this project, a Natural Language model is developed to interpret the intent of Cash Trap trigger clauses within SASB CMBS agreements (aka: Single Asset Single Borrower Commercial Mortgage Backed Securities). These documents are lengthy and complex - typically spanning 300-600 pages. Embedded in these agreements are Cash Trap Trigger events that have significant financial implications for the parties that enter into them. The core goal of this work is to automate the interpretation and classification of the sentences containing trigger information.


---


## Data Set

The underlying dataset consists of both trigger-related and nontrigger sentences that have been hand labeled by a linguist. The dataset represents 36 SASB documents, with sentences ranging from 5 to 1300+ words. 15 distinct trigger categories are contained within the dataset, and each sentence can contain more than one trigger category.

As received, the raw data set had the following structure:

| Document | Sentence                        | Trigger             | Multiclass |
|----------|---------------------------------|---------------------|------------|
| Doc_1    | The Borrower has established... | Loan Default        | 1          |
| Doc_1    | The Borrower has established... | Aggregate DSCR Fall | 1          |
| Doc_1    | "Trigger Period" means any...   | DSCR Fall           | 0          |

Where the Sentence is the original text extracted from the document, the Trigger column is the label assigned by a linguist, and Multiclass represents where a given sentence had more than one label assigned. 

---

## Executive Summary

Some of the key characteristics of the data set are: structured text patterns, varied sentence lengths and nonstandard language. Several models were evaluated (including Logistic Regression, Random Forest and BERT, a pretrained neural network). Logistic Regression was selected as our final model, for several reasons. The logistic regression model was quite successful in predicting the most frequent trigger categories (0.950+ ROC AUC) , while also being simple to implement, fast/inexpensive to run, and providing intuitive features (words and phrases contained in the sentences). 

Due to the fact that individual sentences could contain more than one trigger type, it is important to note that a multiclass classification approach would not be appropriate. The implemented approach is instead to create a series of binary classification models, where each model predicts whether a given sentence contains a specific trigger type.

The relatively small data size and class balance differences created challenges. Some triggers were represented only a few times within the data set, and the standard logistic regression model did not perform well in these cases due to poor representation of the trigger types within the data set. To address this issue, a secondary classification approach was developed, where the aim was to predict whether a sentence contained a Trigger clause (i.e.,  Trigger Sentence vs. Nontrigger Sentence). For this approach, a custom Training set was created, containing balanced classes of the larged trigger categories. The Test set contained the small classes that had not been exposed to the training set. This approach was successful, and was able to identify 90% of the sentence containing trigger types that were not contained in the training set.

Furthermore, in this project, an emphasis was placed on review of any miscategorized sentences as well as the features that lead to prediction of each trigger type. 

