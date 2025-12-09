# Detecting-Misinformation-and-Fake-News-Titles-Online

Abstract: 

We addressed the problem of the spread of misinformation by developing an AI agent to classify news titles as true or fake. Our approach employed a retrieval-augmented reasoning agent utilizing a TF-IDF search backend and a small Large Language Model for generating interpretable predictions. Out of 100 predictions, the model made 62 valid predictions, achieving an overall accuracy of 46.77% on this subset. While these do indicate some challenges in the model's performance, overall, there were several important conclusions from this accuracy experiment and the model overall. 

Overview: Summarize your project report in several paragraphs.
What is the problem? For example, what are you trying to solve? Describe the motivation.
Why is this problem interesting? For example, is this problem helping us solve a bigger task in some way for society? Where would we find use cases for this problem in the community?
What is the approach you propose to tackle the problem? What approaches make sense for this problem? Would they work well or not?
What is the rationale behind the proposed approach? Did you find any reference for solving this problem previously? If there are, how does your approach differ from theirs (if any)?
What are the key components of the approach and results? Also, include any specific limitations.


The key problem is the difficulty in combating the sheer volume and speed with which misinformation spreads quickly online. Many users, particularly older adults and those with lower media literacy overall, struggle to tell real vs. fake stories. This is often exploited by fake headlines that use emotional or exaggerated language to attract clicks, likes, and viewership. The extremely negative effects of disinformation are relatively uncontroversial, including erosion of trust, increased polarization, and even political and social violence. Our approach here seeks to get at solving a bigger issue in society, which is the fact that the truth has become so objective to one's beliefs, and we often struggle to arrive at mainstream truths. As a society, when we are unable to agree on an objective truth—a foundation—it makes it very tough to agree on anything else. Furthermore, AI is often discussed through the lenses of its economic and technological benefits as well as its social detriments, but few understand or believe that it can have a genuinely positive effect on society. Our project seeks to highlight this possibility.

We are proposing using a retrieval-augmented classification system built around the ReAct (Thought -> Action -> Observation) loops to try to classify news headlines as either fake or true. This architecture uses a retrieval-augmented reasoning agent with a TF-IDF search method paired with a small LLM. The goal is to classify news titles automatically, while also providing some sort of reasoning and/or logic to the model's response to help the user understand the context to our agent's answer. The rationale was to leverage the interpretable step-by-step reasoning capabilities of an LLM while grounding its decisions with evidence retrieved via TF-IDF search. While the TF-IDF approach to classifying misinformation actually seems to be relatively common from research, our unique contribution was integrating this common approach into an LLM-based agent pipeline, which allows for complex decision logic via the ReAct schema that a simple TF-IDF algorithm may not be able to achieve. We also decided to allow the model to abstain from prediction in order to improve reliability by avoiding low-confidence guesses. While this decision may impact the usefulness and final performance of the model, we felt it was important to not "box the model in" to making a decision, and give it the breathing room to abstain from a choice. The key technical components of the model the small instruction-tuned LLM (Qwen 0.5B/1.5B) and the TF-IDF backend. Our experimental results show an accuracy of 46.77% on valid predictions (i.e., the ones the model doesn't abstain from). We found the project was hindered by a few specific limitations. A major limitation encountered was the sensitivity to prompt format and the general limited model capacity of the small LLM used.

Approach: 

The overall prediction process is governed by a Retrieval-Augmented classification algorithm, which is executed via a ReAct loop that's run by the small instruction-tuned Qwen 0.5B/1.5B LLM. When presented with a new news title that needs classification (fake/true), the LLM first generates a Thought (its reasoning) and then an Action, which is always a call to our custom search tool. This search tool is the TF-IDF backend; it then converts the input title into a vector using TF-IDF vectorization. From here, the search tool queries the corpus to retrieve the top k=5 most similar, already-labeled headlines using cosine-similarity ranking. These retrieved results are returned as the Observation and fed back into the LLM's context. The LLM then integrates this external evidence to generate its next Thought and decides to either continue the process for up to six steps or output the final classification (True, Fake, or Abstain). We make a few key assumptions and design choices in this approach. First, a key tenet of our project is the assumption that news titles actually contain enough information to make a determination as to whether the article is fake or true. This assumption is central to the project, as if this were not the case, more information (say the first few sentences of the article, for example) would be required for each input to generate a valid output. We also decided to allow the model to abstain from choosing fake or true when classifying a news model. This allows the model to ignore the situations where it is the most unsure (often likely the toughest calls), meaning the model will only make a choice when it is relatively sure of it's answer. This will help to improve the accuracy and trustworthiness of the agent, but also means that its utility will be lower given the many headlines it will not classify at all. Furthermore, for many missed headlines, the agent may have actually been correct to go with its intuition, but with an option to abstain, it may "chicken out" of answering a more difficult situation. Additional limitations include the model's capacity and our own compute power. Because we are using a relatively small LLM there is a limit to how much deep learning can happen in using the model in our agent pipeline. Furthermore, given we are working on home computers, many of us do not have on aggregate the computation for expansive testing or more intense learning & training. Lastly, our approach comes with a real sensitivity to the prompt format for the model; given the ReAct format we are using, the LLM must adhere to a strict syntax when parsing thoughts—resulting in possibly invalid or unparseable outputs.  

Experiments: Set up the stage for your experimental results.
Describe the dataset, including its basic statistics.
Describe the implementation, including what models you run, what parameters you use, and what computing environment you execute on.
Describe the model architecture (e.g., for neural networks, describe the network structure that you use in the experiments).

Results: Describe the results from your experiments.

Main results: Describe the main experimental results you have; this is where you highlight the most interesting findings.

Supplementary results: Describe the parameter choices you have made while running the experiments. This part explains why those choices were made.

Discussion: Discuss the results obtained above. 
If your results are strong, see if you can compare them with existing approaches you find online.
If your results are not as strong as you had hoped for, make a good-faith diagnosis about what the problem is.
Speculate on what else could be done with the direction you are taking, e.g., how can you make the work more impactful? Suggest future directions or improvements.

Conclusion: In several sentences, summarize what you have achieved.


References: 

Shahane, Saurabh. “Fake News Classification.” Kaggle, 8 Oct. 2023, www.kaggle.com/datasets/saurabhshahane/fake-news-classification. 

