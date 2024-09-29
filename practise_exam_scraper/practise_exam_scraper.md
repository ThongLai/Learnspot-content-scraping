# **Practise Exam Scraper Notebook Documentation**

This document serves as a comprehensive guide for the notebook [practise_exam_scraper.ipynb](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/practise_exam_scraper/practise_exam_scraper.ipynb)

## Table of Contents
- [Required Packages](#required-packages)
- [Utility Functions](#utility-functions)
    - [get_pdf_embed_links](#get_pdf_embed_links)
    - [get_urls_from_file](#get_urls_from_file)
    - [fix_latex_delimiters](#fix_latex_delimiters)
    - [save_excel](#save_excel)
    - [saveJSON](#savejson)
    - [read_json_file](#read_json_file)
    - [append_to_json_logs](#append_to_json_logs)
- [*MathPix* APIs Wrapper Functions](#mathpix-apis-wrapper-functions)
    - [process_pdf](#process_pdf)
    - [process_pdfs](#process_pdfs)
    - [get_result_in_latex](#get_result_in_latex)
    - [get_results_in_latex](#get_results_in_latex)
- [*OpenAI* APIs Wrapper Functions](#openai-apis-wrapper-functions)
    - [count_tokens](#count_tokens)
    - [get_completion_from_messages](#get_completion_from_messages)
    - [extract_from_llm](#extract_from_llm)
    - [fix_llm_response](#fix_llm_response)
    - [extract_practise_exams](#extract_practise_exams)
- [Execution](#execution)

## Required Packages
The following packages are required for this project:
1. **openai**: The official OpenAI package used to interact with OpenAI APIs.
2. **tiktoken**: Provides utilities to manage tokenization for OpenAI models.
3. **pandas**: Used for data manipulation and analysis.
4. **xlsxwriter**: Used for creating Excel XLSX files.
5. **requests**: Used for making HTTP requests.
6. **urllib.parse**: Used for parsing URLs into their components.
7. **os**: Provides functions for interacting with the operating system.
8. **json**: Used for working with JSON data.
9. **re**: Provides support for regular expressions.
10. **time**: Provides various time-related functions.
---

## Utility Functions
This section documents the utility functions used in the web scraping project. These functions handle various tasks such as extracting PDF links, managing URLs, saving data to JSON and Excel files, and processing LaTeX strings.

### get_pdf_embed_links
This function extracts and returns the embedded PDF link from a given URL.

- **Parameters**: 
  - `url` (str): The URL from which to extract the PDF link.
  
- **Returns**: 
  - `str`: The decoded PDF URL.

#### Behavior:
1. Checks if the URL contains "https://pmt". If it does, it returns the URL with spaces replaced by `%20`.
2. If not, it extracts the 'pdf' parameter from the query string of the URL.
3. Decodes the extracted PDF URL and replaces spaces with `%20`.
4. Returns the final PDF URL.
---

### get_urls_from_file
This function reads URLs and pre-defined attributes from a text file.

- **Parameters**: 
  - `filename` (str, optional): The name of the input file. Default is 'input_urls.txt'.
  
- **Returns**: 
  - `tuple`: Contains `year_group`, `subject`, `sub_topic`, and a list of URLs.

#### Behavior:
1. Reads lines from the specified file.
2. Extracts `year_group`, `subject`, and `sub_topic` from the first three lines.
3. Collects URLs from the remaining lines, using `get_pdf_embed_links` to process each URL.
4. Prints the number of URLs read and returns the extracted values.
---

### fix_latex_delimiters
This function ensures that LaTeX delimiters in a string are correctly paired.

- **Parameters**: 
  - `latex_string` (str): The LaTeX string to be processed.
  
- **Returns**: 
  - `str`: The modified LaTeX string with correctly paired delimiters or the original input if not a string.

#### Behavior:
1. Checks if the input is a string. If not, returns the input as is.
2. Counts the occurrences of double dollar signs (`$$`).
3. If the count is odd, appends an additional `$$`.
4. Counts the occurrences of single dollar signs (`$`).
5. If the count is odd, appends an additional `$`.
6. Returns the modified string.

---

### save_excel
This function saves data to an Excel file with data validation.

- **Parameters**: 
  - `practise_data` (list): The data to be saved in the Excel format.
  - `output_file` (str, optional): The name of the output Excel file. Default is "file.xlsx".

#### Behavior:
1. Creates a DataFrame from the given data and adds relevant metadata (Year Group, Subject, Sub-Topic, etc.).
2. Calls `fix_latex_delimiters` for specified columns to ensure LaTeX is formatted correctly.
3. Saves the DataFrame to an Excel file.
4. Applies data validation to the 'Difficulty' column.
5. Prints a success message indicating the number of practise exam data entries saved.
---

### saveJSON
This function saves data to a JSON file, useful for testing and debugging.

- **Parameters**: 
  - `data`: The data to be saved in JSON format.
  - `name` (str, optional): The name of the output JSON file. Default is 'data.json'.

#### Behavior:
1. Opens the specified file in write mode with UTF-8 encoding.
2. Dumps the data into the file in JSON format.
3. Ensures non-ASCII characters are preserved and adds indentation for readability.
---

### read_json_file
This function reads and returns data from a JSON file.

- **Parameters**: 
  - `filename` (str, optional): The name of the input JSON file. Default is 'pdf_ids_logs.json'.
  
- **Returns**: 
  - `dict`: The data loaded from the JSON file.

#### Behavior:
1. Opens the specified JSON file in read mode with UTF-8 encoding.
2. Loads the JSON data into a Python dictionary and returns it.
---

### append_to_json_logs
This function appends a new set of PDF IDs to the JSON logs file.

- **Parameters**: 
  - `pdf_ids` (list): The PDF IDs to be appended.
  - `logs_file` (str, optional): The name of the logs file. Default is 'pdf_ids_logs.json'.

#### Behavior:
1. Checks if the specified JSON file exists.
2. Reads existing data if the file is found; initializes an empty list if not.
3. Appends the new PDF IDs to the data list.
4. Writes the updated data back to the JSON file.
---

## *MathPix* APIs Wrapper Functions

This section documents the wrapper functions for *MathPix* APIs. These functions handle various tasks such as processing PDFs, retrieving LaTeX content, and managing PDF IDs.

[MathPix APIs Documentation](https://docs.mathpix.com)

---

### process_pdf

This function sends a request to the MathPix API to process a PDF and extract its contents.

- **Parameters**: 
  - `url` (str): The URL of the PDF to be processed.
  - `app_id` (str, optional): The MathPix application ID. Default is retrieved from environment variables.
  - `app_key` (str, optional): The MathPix application key. Default is retrieved from environment variables.

- **Returns**: 
  - `dict`: The JSON response from the MathPix API containing the PDF ID.

#### Behavior:
1. Sends a POST request to the MathPix API with the PDF URL and conversion formats.
2. Returns the JSON response containing the processed PDF information.

---

### process_pdfs

This function processes pairs of question and answer PDFs and stores their IDs.

- **Parameters**: 
  - `url_pairs` (list of tuples): A list of tuples containing pairs of question and answer PDF URLs.

- **Returns**: 
  - `dict`: A dictionary containing lists of question and answer PDF IDs.

#### Behavior:
1. Initializes a dictionary to store PDF IDs for questions and answers.
2. Iterates over the provided URL pairs, processing each question and answer PDF using `process_pdf`.
3. Appends the PDF IDs to the respective lists in the dictionary.
4. Calls `append_to_json_logs` to save the PDF IDs for future reference.

---

### get_result_in_latex

This function retrieves the LaTeX content for a given PDF ID from the MathPix API.

- **Parameters**: 
  - `pdf_id` (str): The ID of the PDF for which to retrieve the LaTeX content.
  - `app_id` (str, optional): The MathPix application ID. Default is retrieved from environment variables.
  - `app_key` (str, optional): The MathPix application key. Default is retrieved from environment variables.

- **Returns**: 
  - `str`: The LaTeX content retrieved from the MathPix API.

#### Behavior:
1. Sends a GET request to the MathPix API to retrieve the LaTeX content for the specified PDF ID.
2. Returns the response text containing the LaTeX content.

---

### get_results_in_latex

This function retrieves LaTeX contents for all processed question and answer PDFs.

- **Parameters**: 
  - `pdf_ids` (dict): A dictionary containing lists of question and answer PDF IDs.

- **Returns**: 
  - `dict`: A dictionary containing the LaTeX contents for questions and answers.

#### Behavior:
1. Initializes a dictionary to store the LaTeX contents for questions and answers.
2. Iterates over the PDF IDs, retrieving the LaTeX content for each question and answer PDF using `get_result_in_latex`.
3. Implements a retry mechanism to wait for processing if the content status is "split".
4. Returns the dictionary containing the LaTeX contents.

---

## *OpenAI* APIs Wrapper Functions

This section documents the wrapper functions for *OpenAI* APIs. These functions handle various tasks such as interacting with the *OpenAI* API, processing LaTeX content, and managing responses.

[OpenAI APIs Documentation](https://platform.openai.com/docs/api-reference)

---

### count_tokens

This function counts the number of tokens in a given text based on the specified model.

- **Parameters**: 
  - `text` (str): The text for which to count tokens.
  - `model` (str, optional): The model to use for tokenization. Default is "gpt-4o-mini".

- **Returns**: 
  - `int`: The number of tokens in the text.

#### Behavior:
1. Uses the `tiktoken` library to encode the text for the specified model.
2. Returns the length of the encoded text as the token count.

---

### get_completion_from_messages

This function retrieves a completion response from the OpenAI API based on the provided messages.

- **Parameters**: 
  - `messages` (list): A list of messages to send to the OpenAI API.
  - `model` (str, optional): The model to use for the completion. Default is "gpt-4o-mini".

- **Returns**: 
  - `str`: The content of the response from the OpenAI API.

#### Behavior:
1. Sends a request to the OpenAI API with the specified messages.
2. Returns the content of the first choice from the response.

---

### extract_from_llm

This function extracts structured data from the OpenAI API using provided question and answer contents.

- **Parameters**: 
  - `questions_contents` (str): The LaTeX content of the questions.
  - `answers_contents` (str): The LaTeX content of the answers.
  - `system_message` (str, optional): The system prompt to guide the LLM. Default is defined at the beginning of the code.

- **Returns**: 
  - `str`: The response from the OpenAI API containing structured data.

#### Behavior:
1. Constructs the input contents by combining the questions and answers.
2. Sends the input contents to the OpenAI API and returns the response.

---

### fix_llm_response

This function attempts to correct a JSON response from the OpenAI API that failed to load properly.

- **Parameters**: 
  - `LLM_response` (str): The response string from the OpenAI API.
  - `error` (Exception): The error encountered while trying to load the response into JSON format.
  - `model` (str, optional): The model to use for the correction. Default is "gpt-4o-mini".

- **Returns**: 
  - `str`: The corrected JSON response.

#### Behavior:
1. Constructs an input message detailing the error encountered.
2. Sends the input message to the OpenAI API to obtain a corrected JSON format.

---

### extract_practise_exams

This function extracts practise exam data from the OpenAI API based on provided question and answer contents.

- **Parameters**: 
  - `practise_data` (list): The list to which extracted data will be appended.
  - `contents` (dict): A dictionary containing lists of question and answer contents.

- **Returns**: 
  - `list`: A list of responses from the OpenAI API.

#### Behavior:
1. Iterates over the provided question and answer contents.
2. Counts tokens for the input and output.
3. Sends the contents to the OpenAI API and attempts to load the response into JSON format.
4. Handles errors by attempting to fix the response using the `fix_llm_response` function.
5. Appends successfully parsed data to the `practise_data` list.

---

## Execution
This section documents the execution steps involved in the web scraping project, including reading URLs, converting PDFs to LaTeX, and processing the content with a language model.

### Read URLs from File
This step retrieves the year group, subject, sub-topic, and URLs from a specified input file.

- **Code**:
    ```python
    year_group, subject, sub_topic, urls = get_urls_from_file()
    year_group, subject, sub_topic
    ```

- **Returns**: 
    - `year_group`, `subject`, `sub_topic`, and a list of `urls`.

#### Behavior:
1. Reads lines from the specified file.
2. Extracts `year_group`, `subject`, and `sub_topic` from the first three lines.
3. Collects URLs from the remaining lines.
---

### Convert PDFs into LaTeX (MathPix APIs)
This step processes the provided URLs to convert the associated PDFs into LaTeX format using the MathPix API.

- **Code**:
    ```python
    pdf_ids = process_pdfs(urls)
    ```

- **Returns**: 
    - A list of `pdf_ids` corresponding to the processed PDFs.

#### Behavior:
1. Sends requests to the MathPix API to process each PDF URL.
2. Collects and returns the PDF IDs for further processing.
---

### Get LaTeX Contents Using `pdf_ids` (MathPix APIs)
This step retrieves the LaTeX content for each processed PDF. Note that some PDFs may have many pages, requiring additional time to process.

- **Code**:
    ```python
    pdf_ids = read_json_file()[-1]
    contents = get_results_in_latex(pdf_ids)
    ```

- **Returns**: 
    - The LaTeX contents extracted from the PDFs.

#### Behavior:
1. Reads the most recent PDF IDs from the JSON logs.
2. Uses the MathPix API to retrieve the LaTeX content based on the PDF IDs.
3. Handles processing delays for large PDFs.
---

### Injecting into LLM (OpenAI APIs)
This step involves using the OpenAI API to inject the extracted content into a language model for further processing.
- **Important Note**: 
    - Currently using [gpt-4o-mini](https://platform.openai.com/docs/models/gpt-4o-mini), the most cost-efficient small model that’s smarter and cheaper than GPT-3.5 Turbo, and has vision capabilities. (Max output tokens: **16,384** tokens)
    - Other GPT models with higher output tokens can be used if there are many questions in a single PDF.
---

- **Code**:
    ```python
    practise_data = []
    LLM_responses = []
    LLM_responses = extract_practise_exams(practise_data, contents)
    ```
    ```
    output_file = f"{sub_topic.lower()} {year_group.lower()} {subject.lower()}.xlsx"
    save_excel(practise_data, output_file)
    ```

- **Returns**: 
    - An Excel file containing the processed practise data.

#### Behavior:
1. Utilizes the OpenAI API to process the LaTeX contents and extract practise exam questions and answers.
2. Constructs an output filename based on the subject, year group, and sub-topic.
3. Calls `save_excel` to save the practise data to an Excel file.
---
