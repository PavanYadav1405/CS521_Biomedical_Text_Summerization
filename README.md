# CS521_Biomedical_Text_Summerization

We put forward transformer-based approaches leveraging structured knowledge graphs to advance the state-of-the-art in biomedical text summarization.

The SumPubMed medical summarization corpus [Gupta et al., 2021] served as the foundation for our model training and evaluation. This dataset contains rich annotations across over 32,000 biomedical publications, enabling robust analysis. Each included document is paired with two types of gold reference summaries: a full abstract from the original paper, as well as a condensed summary focusing solely on key findings and contributions.

Cloning the dataset -!git clone https://github.com/vgupta123/sumpubmed

## Knowledge Graph Creation

Our overall goal was to identify and represent connections between medical concepts extracted from text, in order to enrich transformer-based language models with a graph-based contextual backing of medical relationships. As our corpus for analysis, we utilized 5000 medical document summaries from the SumPubMed dataset. We chose this dataset because of its reliable annotations and quality summaries covering a breadth of medical topics - enabling robust semantic connection mapping across a range of domains.

Our pipeline began by parsing the SumPubMed documents using SciSpacy, a SpaCy model optimized for scientific and medical text. SciSpacy identified critical medical entities and relationships within each document. We then constructed medical knowledge graphs capturing connections between extracted concepts using the Textacy and NetworkX Python libraries for graph analysis. In these graphs, nodes represented medical concepts while edges linked related concepts based on co-occurrence within documents or explicit connections made in the source text.

Originally, we had planned to further enhance our knowledge graphs using the Unified Medical Language System (UMLS) Metathesaurus API. UMLS integrates over 100 medical terminologies and would have allowed us to integrate additional standard concept relationships into our graphs. However, we faced challenges fully implementing this tool at scale. As an alternative approach, we focused solely on building semantically-rich graphs directly from our SumPubMed corpus parsing.

Our baseline set consisted of transformer-based neural models previously demonstrating strong performance on in-domain summarization tasks: GPT, BART and T5 sequence-to-sequence and Longformer Encoder-Decoder models. We tried to train versions of these baseline systems using only the raw SumPubMed reference document and summary pairs.

Issues with GPT-2 and BART -
Very less context limit of 1k tokens

Issues with T5 -
Model too large for our hardware constraints

Model chosen to move forward with - Longformer Encoder-Decoder - allenai/led-base-16384
Advantages -

- Able to finetune it on M3 Max Macbook
- has high token limit of 16k tokens

Next, to integrate external knowledge, we additionally incorporated our textual knowledge graphs encoding semantic connections between medical concepts extracted from the SumPubMed corpus. With graph-enriched and baseline versions of each model architecture, our experiments aimed to isolate the degree of performance gains coming solely from the integration of connected domain knowledge. Through comparative ablation studies between baseline and graph-enriched systems, we quantified summarized medical content quality improvements attributable solely to medical knowledge graph integration across a variety of model architectures.

## Implementation

We carried out rigorous experiments, implemented in PyTorch using the Transformers library which provides state-of-the-art models optimized for sequence tasks. PyTorch enabled rapid and modular prototyping of graph incorporation approaches. The TensorBoard toolkit was leveraged for tracking experimental results across metrics like ROUGE and loss convergence over multiple runs.

For knowledge graph creation - Our computing infrastructure consisted of Google Colab notebooks accessed through the cloud, which provide free GPU access. This enabled executing jobs in a fast, parallelized manner critical for neural approaches. All models were trained using T4 GPUs.

For model finetuning - We used M3 Max Macbook pro with 36 GB RAM.

We ran each experiment 3 times with different random seeds and parameter initializations. We report performance as averaged over these 3 runs along with the variance. This provides much more reliable estimates compared to single run results, as models can demonstrate instability even when keeping hyperparameters fixed. The variance also quantifies fluctuation across different model parameterizations.

## Evaluation -

We used ROGUE metrics for evalluating the 2 models with 100 randomply picked text-summary paires from the sumpubnet dataset-

1. Finetuned with sumpubnet data
2. Finetuned with Sumpubnet data along with Knowledge graph embeddings

## Results -

We noticed an increase in precision and f1 scores for the model with knowledge graph embeddings.

## Future Work -

We would like to continue working o the project and explare other techniques of creating and embedding knowledge graphs to improvwe the results further.
