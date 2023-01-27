# Alumni_Network_Visualization
## Dataset Introduction
The dataset is scraped from Linkedin, containing LPI alumni profiles by jean-marc.sevin@cri-paris.org. 
Every profile should contain the following information: 'Name', 'Position', 'Company', 'Location', 'Experience', 'Education', 'Skills', 'URL'.
There are multiple spreadsheets with these information for different subjects of master, Phd and licence. In that sense, we also subject and LPI degree information.
![Original_Dataset](/assets/raw_dataset.png)

Note:

- Due to the scrapped data, some people do not have the 'skills' information. We drop these people for further processing.
- All data have been anonymized.
- Some data cleaning is needed. 

## Network building
We try to form a skill network from alumni's data to see how the skills are interconnected and see if it can reflect the interdisciplinary feattures of LPI.
The dataset is cleaned to be have user id and a skillset.

![Cleaned_Dataset](/assets/skill_user_dataset.png)

We construct the co-occurance matrix by forming a connection between skills if they appear in the same person's skillset.
Then the co-occurance matrix is used to construct a network using Gephi.
We use modurality to assign cluster to each skill and visualize them. Node size is proportional to weighted degree.

![Preliminary_Results](/assets/preliminary.png)

## Problem

- Too many nodes. Hard to reason.
   - Too many nodes with similar names. For example, Physical and  Physical Science should mean the same skill.
   - One skill can be written in English and French. But they are represented in the network by two nodes.

## Solution
### French to English translation 
using Helsinki-NLP/opus-mt-fr-en pretrained modelf from huggingface.
### nlp encoder model to get sentence embedding of each skill
using sentence-transformers/all-MiniLM-L6-v2 from huggingface.
![Skill_nlp_embedding](/assets/embedding2.png)
We can see skills with similar meanings are clustered together.
Based on this, we can group similar skills to be one, in order to reduce redundancy in network.
Moreover, we use the emsi skill hierarchy dataset API to get a skill hierarchy. 
(An open-source library of 32,000+ skills gathered from hundreds of millions of online job postings, profiles, and resumes—updated every two weeks. https://lightcast.io/open-skills)
![emsi_dataset](/assets/emsi_skills.png)
We map each skill in our dataset to the skill hierarchy dataset.
In doing so, we can reduce have skills that are independent from each other.
We can also have the skillset hierarchy.

## Further work
- Visualize with different level of skills. For example, skill level, sub-category level and category level.
- Do clustering only based on skill embeddings. See how LPI's network clustering differs.
   - using un-supervised learning clustering methods. *************find more metrics
   - then use PCA, tSNE to visualize ****************UMAP
   - use skill hierarchy to form distance between subjects and see how it differs from LPI to see interdisciplinarity.
- Build user profile for the cluster with some skillset.

## Presentation
Please refer to the report
