# CPC Classification Codes for AI/ML/Software Patent Searches

## Primary AI/ML Classifications

### G06N - Computing Arrangements Based on Specific Computational Models

| Code | Description | Use For |
|------|------------|---------|
| G06N 3/00 | Biological models (neural networks) | Any neural network invention |
| G06N 3/02 | Using neural network models | General NN applications |
| G06N 3/04 | Architecture (interconnection topology) | Novel network architectures |
| G06N 3/044 | Recurrent networks | RNN, LSTM, GRU inventions |
| G06N 3/045 | Combinations of networks | Ensemble methods, multi-model |
| G06N 3/0455 | Auto-encoder networks | Autoencoders, VAE |
| G06N 3/0464 | Convolutional networks | CNN inventions |
| G06N 3/0475 | Generative adversarial networks | GAN inventions |
| G06N 3/048 | Attention mechanisms | Transformer, attention inventions |
| G06N 3/08 | Learning methods | Training methodology |
| G06N 3/082 | Backpropagation | Training with backprop |
| G06N 3/084 | Supervised learning | Labeled data training |
| G06N 3/088 | Unsupervised/self-supervised | Contrastive learning, etc. |
| G06N 3/09 | Reinforcement learning | RL inventions |
| G06N 3/094 | Transfer learning | Fine-tuning, domain adaptation |
| G06N 3/096 | Optimization of neural networks | Architecture search, pruning |
| G06N 5/00 | Knowledge-based models | Expert systems, knowledge graphs |
| G06N 7/00 | Probabilistic models | Bayesian methods |
| G06N 20/00 | Machine learning (general) | General ML methods |
| G06N 20/10 | Ensemble methods | Random forest, boosting |
| G06N 20/20 | Support vector machines | SVM inventions |

### G06F - Electric Digital Data Processing

| Code | Description | Use For |
|------|------------|---------|
| G06F 18/00 | Pattern recognition | Classification, clustering |
| G06F 18/20 | Analysis of multidimensional data | Dimensionality reduction |
| G06F 18/21 | Feature extraction/selection | Feature engineering |
| G06F 18/24 | Classification | General classification methods |
| G06F 40/00 | Natural language processing | NLP inventions |
| G06F 40/20 | NLP - natural language analysis | Parsing, understanding |
| G06F 40/30 | NLP - semantic analysis | Meaning extraction |
| G06F 40/40 | NLP - text processing | Text manipulation |
| G06F 40/56 | NLP - text summarization | Summarization |
| G06F 40/58 | NLP - machine translation | Translation inventions |

### G06V - Image or Video Recognition

| Code | Description | Use For |
|------|------------|---------|
| G06V 10/00 | Image analysis arrangements | General image analysis |
| G06V 10/70 | Feature extraction | Image feature extraction |
| G06V 10/764 | Using neural networks | NN-based image analysis |
| G06V 10/82 | Object detection | Detection inventions |
| G06V 20/00 | Scenes; scene-specific elements | Scene understanding |
| G06V 20/40 | Augmented reality | AR-related |
| G06V 40/00 | Biometric recognition | Face, fingerprint, etc. |
| G06V 40/16 | Face recognition | Face ID inventions |

### G10L - Speech Analysis/Synthesis/Recognition

| Code | Description | Use For |
|------|------------|---------|
| G10L 15/00 | Speech recognition | ASR inventions |
| G10L 15/06 | HMM-based | Traditional ASR |
| G10L 15/16 | Using neural networks | NN-based ASR |
| G10L 15/22 | Adaptation procedures | Speaker adaptation |
| G10L 15/26 | Multi-lingual | Multilingual ASR |
| G10L 13/00 | Speech synthesis | TTS inventions |

## Domain-Specific Classifications

### Healthcare AI
| Code | Description |
|------|------------|
| G16H 30/00 | ICT for diagnosis |
| G16H 50/00 | ICT for medical prediction/prognosis |
| G16H 50/20 | Using machine learning |
| A61B 5/72 | Signal processing for diagnosis |

### Autonomous Driving
| Code | Description |
|------|------------|
| G05D 1/00 | Control of position/course |
| G06V 20/56 | Traffic monitoring |
| B60W 60/00 | Drive control for autonomous vehicles |

### Robotics
| Code | Description |
|------|------------|
| B25J 9/16 | Robot control |
| B25J 13/08 | Robot with sensors |

## Search Strategy by Technology

### For Transformer/Attention-Based Inventions
Primary: G06N 3/048, G06N 3/04
Secondary: G06F 40/00 (if NLP), G06V 10/764 (if vision)

### For Training Method Inventions
Primary: G06N 3/08, G06N 3/084/088/09
Secondary: G06N 3/096 (if optimization-related)

### For Multi-Modal AI
Primary: G06N 3/04, G06N 3/045
Secondary: G06V 10/00, G06F 40/00, G10L 15/00

### For Edge AI / Model Compression
Primary: G06N 3/096
Secondary: G06F 18/00, H04L (if communication-related)

## Tips for Using CPC Codes

1. **Start broad, then narrow**: Begin with G06N 3/00, review results, then focus on specific subcodes
2. **Combine with keywords**: CPC + keyword search is more precise than either alone
3. **Check multiple codes**: AI inventions often span 2-3 CPC areas
4. **Use "also classified" (A) codes**: Patents may have secondary classifications that reveal related art
5. **Cross-reference with IPC**: Some databases use IPC instead of CPC; the codes are similar but not identical
