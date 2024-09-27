# Learnspot Content Scraping
This repository contains a Python tool designed to automatically scrape contents for students from Year 1 to Year 10-11, 11+, and Year 12-13 (A-levels). The scraper extracts quizzes and practice exams from various educational websites using Selenium, Beautiful Soup, LLM models (OpenAI API) and OCR (MathPix API).

## Features
- **Scraping Capabilities**: Efficiently scrape quiz data from various sources.
- **Data Storage**: Save scraped data in multiple formats (e.g., CSV, JSON).
- **Customizable**: Easily modify the scraping logic for different quiz sites.
- **User-Friendly**: Simple setup and usage instructions.

# Installation Instructions
This README will guide you through the process of setting up the project, creating a virtual environment, and installing the necessary dependencies using `requirements.txt`.


## Table of Contents
- [Prerequisites](#prerequisites)
- [Setting Up the Project](#setting-up-the-project)
- [Creating a Virtual Environment](#creating-a-virtual-environment)
- [Installing Dependencies](#installing-dependencies)
- [Usage](#usage)
- [Deactivating the Virtual Environment](#deactivating-the-virtual-environment)

## Prerequisites
Before you begin, ensure you have the following installed on your machine:
- **Python 3.x**
- **pip** (Python package installer)



## Setting Up the Project

1. **Clone the Repository**

   Start by cloning the repository to your local machine. Use the following command:
      ```bash
      git clone https://github.com/ThongLai/Learnspot-content-scraping
      ```

2. **Navigate to the Project Directory**
   Change into the project directory:
      ```bash 
      [Quizzes] cd quiz_scraper
      [Practice Exams] cd practice_exam_scraper
      ```
      
## Creating a Virtual Environment

To avoid conflicts between packages and maintain a clean workspace, it's recommended to create a virtual environment. Follow these steps:

1. **Create the Virtual Environment**

   Run the following command to create a virtual environment named `quiz_scraper_env`/`practice_exam_scraper_env`:
      ```bash
      [Quizzes] python -m venv quiz_scraper_env
      [Practice Exams] python -m venv practice_exam_scraper_env
      ```

2. **Activate the Virtual Environment**
   
   On Windows:
      ```bash 
      [Quizzes] quiz_scraper_env\Scripts\activate
      [Practice Exams] practice_exam_scraper_env\Scripts\activate
      ```
   On macOS and Linux:
      ```bash 
      [Quizzes] source quiz_scraper_env/bin/activate
      [Practice Exams] source practice_exam_scraper_env/bin/activate
      ```
   After activation, you should see `(quiz_scraper_env)`/`(practice_exam_scraper_env)` at the beginning of your command prompt. 
   
   Change back to the `quiz_scraper`/`practice_exam_scraper_env` working directory using:
      ```bash
      cd ../../
      ```

## Installing Dependencies

With the virtual environment activated, you can now install the required dependencies.

1. **Install Dependencies from `requirements.txt`**
   
   The `requirements.txt` is in `quiz_scraper` or`practice_exam_scraper_env` directory, ensure you are currently in that working directory.

   Use the following command to install all the necessary packages:
   ```bash
   pip install -r requirements.txt
   ```

## Usage

This guide will help you use the Jupyter Notebooks `quiz_scraper.ipynb`/`practice_exam_scraper.ipynb` to read URLs from an input file and process content extraction.

### Step 1: Open the Jupyter Notebook

- **Launch Jupyter Notebook**

   Open your terminal or command prompt and navigate to the directory containing the `quiz_scraper.ipynb`/`practice_exam_scraper.ipynb` notebooks. Start Jupyter Notebook by running:
   ```bash
   jupyter notebook
   ```
- **Open the Notebook**

   In the Jupyter interface, click on `quiz_scraper.ipynb` or `practice_exam_scraper.ipynb` to open it.
   
### Step 2: Prepare the Input File

   - Ensure you have a file named `input_urls.txt` in the same directory as your notebooks. This file should contain the URLs you want to scrape, with each URL on a new line.

   - **Example of `input_urls.txt`**
      ```
      Year 1
      English
      Comprehension
      https://www.educationquizzes.com/ks1/english/comprehension-a-letter-to-the-queen-summer-of-year-1/ 
      https://www.educationquizzes.com/ks1/english/comprehension-aliens-stole-my-homework-summer-of-year-2/ 
      https://www.educationquizzes.com/ks1/english/comprehension-inference/ 
      https://www.educationquizzes.com/ks1/english/comprehension-inference/ 
      https://www.educationquizzes.com/ks1/english/proverbs/ 
      https://www.educationquizzes.com/ks1/english/reading-fairy-tales-characteristics/ 
      https://www.educationquizzes.com/ks1/english/reading-fairy-tales-characters/  
      ```
### Step 3: Run the Notebook
- **Execute All Cells:** Finally, run all the cells and the **Execution** code block in the notebook to complete the scraping process and analyze the results.

## Deactivating the Virtual Environment
- When you're done working on the project, you can deactivate the virtual environment by running:
   ```bash
   deactivate
   ```