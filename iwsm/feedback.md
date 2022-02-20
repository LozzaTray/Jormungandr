Summary

Decision: Reject
Comment:
This paper proposes a generative model (FFBM) for generating networks with community structure and node features or labels. The authors present a Bayesian framework for inference under this model and show its performance on a number of real-world networks. The reviewers have asked several questions ranging from the optimization landscape and presence of local optima to the time complexity of the proposed model. The authors need to address these. The reviewers have also pointed out that there is a large literature on Stochastic Blockmodels and related models. The authors should use these suggestions to strengthen the experimental section further.



##########################################################################################################################################################
LAFd
##########################################################################################################################################################

Summary:
In this work the authors contribute towards understanding the interplay between features and graph structure. Nowadays many datasets consist of a graph topology where each node is associated with D features. The key question the authors ask in this work, is which features, if any, determine the edges. To achieve this goal, the authors propose a new generative model that first draws block memberships given the input, and given the block membership generates the edges according to the micro-canonical stochastic block-model of [13]. The approach is Bayesian, with choices of probability distributions and priors that maintain the simplicity while offering useful insights into the importance of features. The proposed inference method is applied on three real-world datasets.

Main Review:
The definition 2.1 should state that e=(e_rs) is BxB instead of NxN symmetric matrix.
In lines 41-44 references are needed in the description of the intuitive procedure that first one partitions the graph according to the feature meta-data and checks how the connectivity correlates with the actual topology.
Can you discuss in a real-world setting with discrete (e.g., Zip code) and continuous features (e.g., salary) how you would apply your method efficiently? 1-hot encoding may increase D a lot and at the same time the complexity of the inference steps is likely to not allow the method to scale to large graphs. Can you discuss the scalability of your method, and discuss its computational complexity?
Can you discuss further the case of local optima for the vectors w_b and how they affect the interpretability? Have you run into such cases in the experiments?
Can you report runtimes in the experimental section?
Can you clarify the visualizations in Figure 4? For example, in Figure 4b, it looks 5B is given a float block index (around x=0.5). Also changing the marker type instead of only the color can be helpful for readability.
Limitations And Societal Impact:
There is no obvious potential negative societal impact of the authors' work .

Ethical Concerns:
None

Needs Ethics Review: No
Ethics Review Area: I don’t know
Time Spent Reviewing: 5
Rating: 5: Marginally below the acceptance threshold
Confidence: 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.
Code Of Conduct: While performing my duties as a reviewer (including writing reviews and participating in discussions), I have and will continue to abide by the NeurIPS code of conduct.


##########################################################################################################################################################
iZbe
##########################################################################################################################################################


Summary:
This paper presents a technique to model both the community structure and vertex-label distribution in undirected graphs, namely the feature-first block model (FFBM). This is done through a Bayesian estimation and based on the micro-canonical degree-corrected stochastic block-model with a posterior of block memberships  given D dimensional features  (and weights ) and a posterior given  and the adjacency matrix . Estimation is done through a two-level Markov chain and sampling is done to assign block membership. A formulation to reduce the variance of the negative log-posterior  and its gradient is also proposed using expectations as well as a dimensionality reduction to discard less important features. The experiments evaluate the proposed technique on three real datasets.

Main Review:
Creating models that provide a good representation of community structure and vertex-labels is an important problem. A Bayesian formulation could offer many benefits for modeling uncertainty and to provide probabilistic measurements on block membership in graph models where labels are considered. Using a splitter Markov chain to capture block memberships and weights is a reasonable choice.

The experiments show a set of alternative configurations of the proposed FFBM with and without dimensionality reduction and assessments about the structure of the blocks and how a particular feature could identify a set of nodes. However, it is hard to asses the quality of the models since no alternative baseline models are provided. For instance, what happens if the block structure is first estimated and then the attribute model is fit on a per-block basis? This is a fundamental question in order to asses how well the FFBM works. Also, while the paper does mention some related work, SBM is a highly explored area of research where comparison with alternatives is possible. Other alternatives could be to compare it agains degree-based network models such as the Chung-Lu model or the Feastpack model for degree information or the multiplicative attribute graph model (MAG) adapted to include vertex-label covariates.

With respect to the model itself, in page 3 line 85 it is stated that the bias term is excluded to guarantee only leveraging feature information. This can be very problematic as there is a need to centering the data before applying the softmax function for both estimation and prediction. Otherwise, the results will be compromised, unless what it was meant was that the bias is not included for notational purposes only.

Finally, there is a minor typo in Definition 2.1. where it is stated that  is a matrix  of edge counts however  is the number of vertices in the adjacency matrix. Thus, the dimension of  should be .

Limitations And Societal Impact:
Section 6 did briefly recommend ethical and privacy preserving practices when applying their model.

Ethical Concerns:
NA

Needs Ethics Review: No
Time Spent Reviewing: 16
Rating: 5: Marginally below the acceptance threshold
Confidence: 5: You are absolutely certain about your assessment. You are very familiar with the related work and checked the math/other details carefully.
Code Of Conduct: While performing my duties as a reviewer (including writing reviews and participating in discussions), I have and will continue to abide by the NeurIPS code of conduct.


##########################################################################################################################################################
mjxX
##########################################################################################################################################################

Summary:
The paper proposes a new generative graph model including network attributes. The new model is based on stochastic block models. The main contribution is the theoretical development of the model. However, the evaluation of the model must be improved.

Main Review:
The originality of this work is important. Even though parts of the development are based on other ideas, the paper proposes a new theoretical model and theoretically justifies its process.

Unfortunately, the clarity must be improved. The paper is confusing. The title focuses on inferring community characteristics, but the abstract talks about a generative graph model. Then, it explains a generative graph model, and realizes an evaluation based on other characteristics. Besides, there are other flaws that can be improved. The description of microcanonical DC-SBM does not explain how the paper respects the constraint. I do not believe that it samples until all the constraints are satisfied. Otherwise, it could take a long time to achieve this purpose. Finally, the paper introduces multiple parameters over the model, without any further explanation. Later, they are explained and the reader has to connect the different parts of the paper with the corresponding parameters, making it confusing. For example, after reading the paper, I could not remember all the parameters that you had to use.

Another important flaw is the time complexity of the model. This is not analyzed at all, only from an empirical perspective, obtaining very bad results in comparison to other state-of-the-art models (some generative graph models can generate networks with millions of nodes and edges in seconds).

The experiment section must improve considerably. First, there is no other model to compare to, please read "A review of stochastic block models and extensions for graph clustering" for a list of other models with similar characteristics. Second, even though the network could replicate the label characteristics, it will not be worthy if it can not replicate the network characteristics. Please include evaluation over this topic, analyze the distribution of local and global characteristics. Finally, what are the effects of the hyperparameters in the final process?

While significance is clear, it does seem practical. It is always important to have new models able to replicate networks. However, given the lack of evaluation against other models, and the large amount of time for small networks, this model seems impractical in comparison to current state-of-the-art methods.

Limitations And Societal Impact:
No, the paper just mentions a simple phrase at the end of the conclusion, "So long as data collection techniques remain ethical and care is taken to respect personal privacy, such empowered decision-making can only help humankind.".

Please, talk about this topic from another point of view. I mean, what are the risk of generating biased network.

Needs Ethics Review: No
Time Spent Reviewing: 4 hours
Rating: 3: Clear rejection
Confidence: 4: You are confident in your assessment, but not absolutely certain. It is unlikely, but not impossible, that you did not understand some parts of the submission or that you are unfamiliar with some pieces of related work.
Code Of Conduct: While performing my duties as a reviewer (including writing reviews and participating in discussions), I have and will continue to abide by the NeurIPS code of conduct.


