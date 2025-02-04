# Hangman ML Player – 65.6% Win Rate Solution

## Overview

This repository contains a machine learning-based solution for the Hangman Challenge, achieving **65.6% accuracy** on test runs and **67.4% in practice runs**. The approach is centered around a **multi-stacked BiLSTM model**, which processes the Hangman game state along with guessed letter flags to predict the next best letter.

The model effectively mimics logical guessing behavior by leveraging contextual learning from BiLSTM layers. It utilizes relevant logical data for training, generated via an agent-env setup, ensuring strong performance in word prediction.

## Solution Approach

### Data Generation Strategy

To train the model, a **data generation strategy** was implemented using an Agent and environment setup:

* **Extensive Data Collection:**
  * An agent-env setup simulated **5 million Hangman games**.
  * **32 million training examples** were collected from correct guesses.

* **Biased Sampling of Actions:**
  * The first guess was always a vowel.
  * Subsequent guesses followed a probability-weighted random strategy:
    * **60%**: Bias toward correct choices.
    * **40%**: Weighted random sampling based on letter frequency in words of similar length.

* **Logical Guessing Flow:**
  * Ensured the guessed letter flags represented realistic human-like guessing patterns.
  * Weights for letter selection were calculated dynamically based on word length-specific letter frequency.

The combination of **Agent-env data generation** and **multi-stacked BiLSTM modeling** resulted in rapid convergence, with the best model achieving peak performance within just **three epochs**.

## Previous Approaches

Before finalizing this approach, multiple other techniques were tested:

1. **Transformers (TinyBERT)**
   * TinyBERT was explored due to computational limitations.
   * Training from scratch resulted in **only 48% accuracy** with long training times (**~2-3 hours per epoch**).
   * Transformer-based models showed potential but were impractical due to resource constraints.

2. **N-Gram Matching**
   * Lacked contextual awareness, particularly for unseen words.
   * Could not generalize effectively beyond the training set.

3. **Reinforcement Learning (DQN)**
   * Learning was unstable and slow to converge.
   * Required over **60 hours of training** with lower batch sizes.
   * Eventually used **only for data generation**, not the final model.

4. **Simple BiLSTM**
   * A basic BiLSTM model (without guessed flags) struggled to converge.
   * Achieved only **35-40% accuracy**, leading to its abandonment.

## Challenges & Insights

* The most significant challenge was **generating relevant training data** and enabling the model to **identify patterns inherent to human word formation**.
* The word list contained **difficult and irregular words**, requiring a model capable of **contextual learning**.
* While **Transformers** were a promising alternative, **BiLSTM provided a more practical solution** given computational constraints.

## Repository Contents

* `BiLSMTNet_ModelTraining.ipynb` – BiLSTM model implementation and training pipeline.
* `DataGenViaRecycledRLcode.ipynb` – Data generation utilizing the agent-environment setup originally developed for the RL-based approach.
* `list_cleaning.ipynb` - Cleaning data to remove irrelevant words.
* `hangman_api_user.ipynb` - This file was used solely for submission as part of an assignment. It is provided for reference to display accuracy metrics. All rights to this file and its contents are held by the respective company.