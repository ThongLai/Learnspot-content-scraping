# Learnspot Content Scraping

## Description
This repository contains a Python tool designed to automatically scrape contents for students from Year 1 to Year 10-11, 11+, and Year 12-13 (A-levels). The scraper extracts quizzes and practise exams from various educational websites using Selenium, Beautiful Soup, LLM models (OpenAI API) and OCR (MathPix API). 

### Quizzes
- **Purpose**: Extract quiz-type questions from supported online sources.
- **Supported Websites**:
  - [BBC Bitesize](https://www.bbc.co.uk/bitesize)
  - [Education Quizzes](https://www.educationquizzes.com)
  - [Primrose Kitten](https://primrosekitten.org)
  - [Math Quiz](https://math-quiz.co.uk)
  - [Cognitoedu](https://cognitoedu.org)
  - [Save My Exams](https://www.savemyexams.com)
- **Dependencies**: 
  - Required Packages: `Beautiful Soup`
  - Some websites (e.g., `BBC Bitesize` and `Cognitoedu`) require `Selenium` for simulation purposes.
- **Documentation**: [Quiz Scraper Notebook Documentation](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/quiz_scraper.md)

### Practise Exam
- **Purpose**: Extract practise exam questions from PDFs or PDF online URLs. The main current source is [Physics and Maths Tutor](https://www.physicsandmathstutor.com).
- **Process**: Uses OCR through MathPix APIs to convert PDFs into LaTeX format. The extracted content is then injected into LLM models using OpenAI APIs.
- **Dependencies**: 
  - Required APIs: `Mathpix`, `OpenAI`
- **Documentation**: [Practise Exam Scraper Notebook Documentation](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/practise_exam_scraper/practise_exam_scraper.md)


### Data Attributes
   1. Pre-defined Attributes
      - **Year Group**: The year of the educational material.
      - **Subject**: The academic subject (e.g., Math, Science).
      - **Sub-Topic**: Specific sub-topics within the subject.

   2. Extracted Attributes
      - **Difficulty**: `easy`/`medium`/`hard` (default is `easy` for sources without difficulty data).
      - **Type of Question**: `MCQ`, `TextEntry`, `Order`, `TapAndFind` (for Quizzes); `Practise Exam` (for Practise Exam extraction).
      - **Question Title**: The title of the question.
      - **Images**: Any images associated with the question.
      - **Options**: The answer options (some questions may not have options).
      - **Answer**: The correct answer to the question.
      - **Source**: For internal use only.
      - **Mark**: Default is `1` for sources without mark data.
      - **Other Text**: Additional explanations or context for the question.

   3. Additional Attributes for *Practise Exams*
      - **ID**: A unique identifier for each question, based on the question number and parent question.
      - **Parent_ID**: Sub-question IDs, assigned based on the parent question (e.g., `1a`, `1b`).
      - **Mark Scheme**: The marking scheme, if available.

### Features
- **Scraping Capabilities**: Efficiently scrape quiz and practise exam data from various sources, including complex web pages requiring `Selenium`.
- **Customizable**: Scraping logic can be easily modified to support additional websites or adapt to changes in the site structure.
- **OCR Integration**: Extract text from PDFs using MathPix APIs and convert it into LaTeX format.
- **LLM Integration**: Uses OpenAI APIs to process and refine question-and-answer content.
---

## Installation Instructions
This README will guide you through the process of setting up the project, creating a virtual environment, and installing the necessary dependencies using `requirements.txt`.

- [Prerequisites](#prerequisites)
- [Setting Up the Project](#setting-up-the-project)
- [Creating a Virtual Environment](#creating-a-virtual-environment)
- [Installing Dependencies](#installing-dependencies)
- [Usage](#usage)
- [Deactivating the Virtual Environment](#deactivating-the-virtual-environment)

### Prerequisites
Before you begin, ensure you have the following installed on your machine:
- **Python 3.x**
- **pip** (Python package installer)

### Setting Up the Project

1. **Clone the Repository**

   Start by cloning the repository to your local machine. Use the following command:
      ```bash
      git clone https://github.com/ThongLai/Learnspot-content-scraping
      ```

2. **Navigate to the Project Directory**
   Change into the project directory:
      ```bash 
      [Quizzes] cd Learnspot-content-scraping/quiz_scraper
      [Practise Exams] cd Learnspot-content-scraping/practise_exam_scraper
      ```
      
### Creating a Virtual Environment

To avoid conflicts between packages and maintain a clean workspace, it's recommended to create a virtual environment. Follow these steps:

1. **Create the Virtual Environment**

   Run the following command to create a virtual environment named `quiz_scraper_env`/`practise_exam_scraper_env`:
      ```bash
      [Quizzes] python -m venv quiz_scraper_env
      [Practise Exams] python -m venv practise_exam_scraper_env
      ```

2. **Activate the Virtual Environment**
   
   On Windows:
      ```bash 
      [Quizzes] quiz_scraper_env\Scripts\activate
      [Practise Exams] practise_exam_scraper_env\Scripts\activate
      ```
   On macOS and Linux:
      ```bash 
      [Quizzes] source quiz_scraper_env/bin/activate
      [Practise Exams] source practise_exam_scraper_env/bin/activate
      ```
   After activation, you should see `(quiz_scraper_env)`/`(practise_exam_scraper_env)` at the beginning of your command prompt. 

### Installing Dependencies

With the virtual environment activated, you can now install the required dependencies.

1. **Install Dependencies from `requirements.txt`**
   
   The `requirements.txt` is in `quiz_scraper` or`practise_exam_scraper_env` directory, ensure you are currently in that working directory.

   Use the following command to install all the necessary packages:
   ```bash
   pip install -r requirements.txt
   ```
   
1. **Add the Jupyter Kernel**
   
   After installing the dependencies, you can add the virtual environment as a Jupyter kernel. 
   
   Run the following command:
   ```bash
   [Quizzes] python -m ipykernel install --name=quiz_scraper_env
   [Practise Exams] python -m ipykernel install --name=practise_exam_scraper_env
   ```

   To verify that the kernel has been added successfully, you can list all available Jupyter kernels with:
   ```bash
   jupyter kernelspec list
   ```

### Adding API Keys for OpenAI and MathPix (For **Practise Exam**)

To enable the functionality of the **Practise Exam** scraper that utilizes *OpenAI* and *MathPix* APIs, you need to set up your API keys. Follow these steps:

1. **Obtain API Keys**
   - **OpenAI API Key**: Create an `OPENAI_API_KEY` from your account dashboard at [OpenAI](https://platform.openai.com/settings/profile?tab=api-keys).
   - **MathPix API Credentials**: Obtain your `MATHPIX_APP_ID` and `MATHPIX_APP_KEY` at [MathPix](https://console.mathpix.com/convert/orgs/learnspot_4be447/apikeys).

2. **Set Environment Variables**
   
   **It is recommended to store your API keys as environment variables for security.** You can do this by adding the following lines to your environment configuration file (e.g., `.env` file) or directly in your terminal session:

   ```bash
   export OPENAI_API_KEY='your_openai_api_key_here'
   export MATHPIX_APP_ID='your_mathpix_app_id_here'
   export MATHPIX_APP_KEY='your_mathpix_app_key_here'
   ```

   Replace `your_openai_api_key_here`, `your_mathpix_app_id_here`, and `your_mathpix_app_key_here` with your actual API keys.

   Later on, in your Python scripts, you can access these environment variables using the os module:
   
      ![image](https://github.com/user-attachments/assets/05732d74-69ba-4500-9621-d1d611dc2785)
      
      ![image](https://github.com/user-attachments/assets/1bef408b-f292-48c1-8e93-ad67dbf7c3f7)

   *this code of OpenAI will automatically read the `OPENAI_API_KEY` from your environment.*

### Usage

This guide will help you use the Jupyter Notebooks `quiz_scraper.ipynb`/`practise_exam_scraper.ipynb` to read URLs from an input file and process content extraction.

#### **Step 1:** Open the Jupyter Notebook
   - **Launch Jupyter Notebook**

      Open your terminal or command prompt and navigate to the directory containing the `quiz_scraper.ipynb`/`practise_exam_scraper.ipynb` notebooks. Start Jupyter Notebook by running:
      ```bash
      jupyter notebook
      ```
   - **Open the Notebook**

      In the Jupyter interface, click on `quiz_scraper.ipynb` or `practise_exam_scraper.ipynb` to open it.
      
#### **Step 2:** Prepare the Input File
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

#### **Step 3:** Run the Notebook
   - **Execute All Cells:** Finally, run all the cells and the **Execution** code block in the notebook to complete the scraping process and analyze the results.
   - Code snippets and Expected outputs:
      - **Quizzes**
         ![image](https://github.com/user-attachments/assets/71919027-86e8-4007-a26d-337090e82b5a)
      - **Practise Exams**
      ![image](https://github.com/user-attachments/assets/d77d6ca3-6661-4c07-84f9-de8643664862)

### Deactivating the Virtual Environment
- When you're done working on the project, you can deactivate the virtual environment by running:
   ```bash
   deactivate
   ```
---

## Contributing
   Here's how you can contribute:

   1. Fork the repository
   2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
   3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
   4. Push to the branch (`git push origin feature/AmazingFeature`)
   5. Open a Pull Request

   Ensure your code adheres to the coding standards and include appropriate tests for new features.

   For major changes, please open an issue first to discuss what you would like to change.
