# HOML PyTorch #1 versus HOML Keras & TensorFlow #3

> **About this checkout:** this file describes the book-level changes from the upstream project. The current repository branch is the community-maintained [Polars port](https://github.com/PGupta-Git/handson-mlp/tree/polars_integration) of [ageron/handson-mlp](https://github.com/ageron/handson-mlp). It tracks upstream while modernizing dataframe workflows for Polars 2.x and compatible versions of scikit-learn and the visualization stack. The upstream README link for the fork is proposed in [PR #41](https://github.com/ageron/handson-mlp/pull/41).

## Polars fork maintenance changes

The `polars_integration` branch keeps the upstream material intact while updating integration points that changed in the surrounding ecosystem:

* dataframe workflows use Polars 2.x where the fork replaces pandas, while `tools_pandas.ipynb` remains as the upstream pandas reference;
* scikit-learn is kept at 1.9.1 or newer so Polars input/output uses the current Narwhals interoperability path;
* Matplotlib and vector-oriented Seaborn calls consume Polars `Series` directly instead of converting plotting data through pandas;
* direct runtime dependencies used by the notebooks, including PyArrow and Stable-Baselines3's Rich progress output, are declared explicitly;
* the notebook index links to the Polars tutorial and the current Chapter 18 and Chapter 19 notebook filenames;
* the index introduces Polars in its prerequisites, removes a broken Kaggle launch link, and the retained tutorial badge and book cover have descriptive alternative text;
* the Polars tutorial distinguishes transformations that return new DataFrames from explicit in-place methods;
* Chapter 19's LunarLander exercise and installation instructions use the supported `LunarLander-v3` environment;
* local and Docker CUDA installation commands retain the project's PyTorch compatibility bounds and resolve before replacing the installed family;
* Docker uses the checkout's Python version and frozen lockfile, with Box2D enabled only through its declared optional extra rather than metadata-mutating build steps;
* the PyTorch family is currently capped below 2.10 because Appendix B still demonstrates eager-mode `torch.ao.quantization`, which PyTorch plans to remove in 2.10; and
* external-service examples are kept executable with current public model/API identifiers, while local credentials remain outside version control.

The committed notebook outputs have been regenerated sequentially from the locked Python 3.12 environment and checked for execution errors.

I wrote three TensorFlow (TF) editions of this book, published by O'Reilly in 2017, 2019, and 2022. If you have already read the latest TensorFlow edition of this book, here are the main changes you will find in this book (for changes between editions 1 and 2, see https://homl.info/changes2, and for changes between editions 2 and 3, see https://homl.info/changes3):

* All the code in the book was updated to recent library versions.
* All the code in Part 2 was migrated from TensorFlow and Keras to PyTorch. There were significant changes in all of these chapters.
* TensorFlow-specific content was removed, including former Chapter 12 (Custom Models and Training with TensorFlow) and Chapter 13 (Loading and Preprocessing Data with TensorFlow), and former Appendices C (Special Data Structures) and D (TensorFlow Graphs).
* The new Chapter 10 now introduces PyTorch, as well as TorchVision, TorchMetrics, and hyperparameter tuning using Optuna.
* I also added three big chapters on transformers (this is by far what required the most work, along with the SSM appendix):
    * Chapter 15 covers transformers for natural language processing. We build a Transformer from scratch, and use it to create a translation system from English to Spanish. Then we learn about encoder-only models like BERT, great for text classification tasks, multiple choice question answering, and more. Then we learn about decoder-only models like GPT, which are excellent at text generation, zero-shot learning, and much more. We then proceed to create a Transformer-based chatbot using pretrained models from the Hugging Face Hub. We learn how to fine-tune a pretrained model using SFT, RLHF, and DPO. Finally, we learn about RAG, vector databases, and how to connect the chatbot to tools, including using MCP.
    * Chapter 16 presents vision transformers (including ViT, DEiT, PVT, Swin, DINO, and more) and multimodal transformers (including VideoBERT, ViLBERT, CLIP, DALL·E, Perceiver, Flamingo, BLIP, and more).
    * Chapter 17, available online at https://homl.info/, discusses several advanced techniques to speed up and scale up transformers. This includes ways to speed up decoding, such as Key/Value caching and speculative decoding. We also look at how to accelerate attention, including sparse attention techniques (such as the Sparse Transformer, LongFormer, BigBird, and the Routing Transformer) and approximate attention techniques (such as the Reformer, Linformer, Performer), and projection-sharing techniques such as multi-query attention, grouped-query attention, and multi-head latent attention, and optimized implementations such as FlashAttention. We then scale up using mixture of experts (MoE), and speed up training using parameter-efficient fine-tuning (PEFT) such as low-rank adaptation (LoRA), activation checkpointing, sequence packing and bucketing, gradient accumulation, and various parallelism techniques.
* Chapter 18 goes into more depth on diffusion models, and also shows how to use pretrained diffusion models.
* Chapter 19 on reinforcement learning now covers Actor-Critic algorithms, including PPO, and it shows how to train your own agent to beat an Atari game, using the Stable-Baselines3 library.
* There are also three new appendices:
    * Appendix B (Mixed Precision and Quantization) explains how to shrink models so they can run faster and fit on smaller devices.
    * Appendix D (Relative Positional Encoding) discusses advanced positional encoding techniques for transformers.
    * Appendix E presents state-space models (SSMs).
* I also added Appendix F on older popular artificial neural network architectures, [available online](https://homl.info).
* To make room for the newer content, the chapter on support vector machines (SVMs) was [moved online](https://homl.info) and renamed to Appendix C; the last two appendices are also online at the same URL, and the deployment chapter was partially merged into Chapter 10.

The three editions of the TensorFlow/Keras version of this book are nicknamed homl1, homl2, and homl3. This book, which is the first edition of the PyTorch version, is nicknamed homlp. Try saying that three times in a row.

Note that most of the changes compared to the latest TensorFlow edition are in the second part of the book. If you have read homl3, then don't expect big changes in the first part of the book, other than up-to-date libraries: the fundamental concepts of machine learning haven't changed since 2022.
