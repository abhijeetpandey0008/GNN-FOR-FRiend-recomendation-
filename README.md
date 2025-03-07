# GNN-FOR-FRiend-recomendation-

OVERVIEW - 
The paper presents a novel approach that leverages Graph Neural Networks (GNNs) to enhance friend ranking and recommendation on large-scale social platforms. The key idea is to learn rich user representations by combining multi-modal features (such as user profiles, content interests, activity logs, etc.) and communication signals (link features) into a unified framework. The resulting model, called GraFRank (Graph Attentional Friend Ranker), is designed to overcome common challenges in social networks, including heavy-tailed degree distributions and activity sparsity.

Traditional friend recommendation systems typically rely on simple heuristics (like common neighbors) or feature-based models. 
However, these methods can fall short when dealing with:

Sparse connectivity: Many users, especially those less active, have limited interaction data.
Multi-faceted interactions: Users engage with the platform in various ways (e.g., direct messages, likes, posts), which are hard to capture with a single feature space.

Dynamic networks: Social graphs evolve over time, requiring models that can handle temporal changes.

GraFRank addresses these issues by:

Modeling multi-modal user features: Each user is represented by features from different modalities (e.g., static profile attributes, dynamic engagement activities).

Incorporating communication signals: Pairwise link features (e.g., frequency of chats or interactions) are used to better capture the quality of relationships.

Leveraging a temporal graph: The evolving nature of the friendship graph is explicitly modeled by using time-aware neighbor aggregation.

Model Architecture

The architecture of GraFRank is built upon two major components:

1. Modality-Specific Neighbor Aggregation
   
Separate Aggregators: Instead of merging all features into a single vector, the model uses separate aggregators for each modality. This design allows it to capture the unique patterns and homophily (the tendency of users to connect with similar others) present in each feature space.

Communication-Aware Aggregation: For each modality, the model incorporates both the user features and the corresponding pairwise link (communication) features. This helps the model prioritize actively engaged relationships.

Message Passing Framework: Inspired by typical GNN operations, the model uses a message-passing mechanism where each node (user) aggregates messages from its neighbors. The messages are computed by transforming both user and link features through learnable weight matrices.

Self-Connections: To preserve the original user features, self-connections are included. This ensures that the aggregated representation retains important individual characteristics.

3. Cross-Modality Attention
   
Attention Mechanism: After obtaining modality-specific representations, a cross-modality attention layer computes attention weights for each modality. This layer learns non-linear correlations between the different modalities and combines them into a final user embedding.

Weighted Fusion: The final representation is a weighted sum of the modality-specific embeddings, where the weights reflect the importance of each modality for friend ranking.

Training Strategy

GraFRank is trained using a temporal pairwise ranking loss that ensures the inner product (similarity) of positive pairs (actual friendships) is higher than that of negative pairs (non-friend pairs) by a predefined margin. Key aspects include:

Temporal Sampling: The evolving friendship graph is divided into daily snapshots, allowing the model to capture time-sensitive behaviors.

Negative Sampling: Two strategies are used:

Random Negatives for coarse candidate retrieval.
Hard Negatives (drawn from the local neighborhood but not actual friends) for fine-grained candidate ranking.

Two-Phase Training: Often, the model is pre-trained with random negatives and then fine-tuned on hard negatives. This approach helps in learning both broad and fine-grained distinctions among users.

Experimental Evaluation
The model was evaluated on large-scale datasets from a major social platform (Snapchat). The experiments demonstrated:

Significant Gains: GraFRank outperforms both traditional feature-based models (e.g., logistic regression, MLP) and other GNN variants (e.g., GCN, GAT) by considerable margins in terms of Mean Reciprocal Rank (MRR) and Hit-Rate metrics.
Robustness Across User Segments: Detailed analysis shows that the model provides notable improvements especially for less active and low-degree users.
Scalability: The model efficiently handles millions of users and billions of connections through effective neighborhood sampling and parallel training strategies.
