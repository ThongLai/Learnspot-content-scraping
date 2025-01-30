# **Quiz Scraper Notebook Documentation**

This document serves as a comprehensive guide for the notebook [quiz_scraper.ipynb](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/quiz_scraper.ipynb)


## Table of Contents
<!-- no toc -->
- [Required Packages](#required-packages)
- [Utility Functions](#utility-functions)
    - [get_urls_from_file *(deprecated)*](#get_urls_from_file-deprecated)
    - [saveJSON](#savejson)
    - [loadJSON](#loadjson)
    - [saveSoup](#savesoup)
    - [save_excel](#save_excel)
    - [load_excel](#load_excel)
    - [open_driver](#open_driver)
    - [close_driver](#close_driver)
    - [javascript_array](#javascript_array)
    - [select_sub_topic](#select_sub_topic)
- [*Azure MS-Graph* APIs](#azure-ms-graph-apis)
    - [create_GraphServiceClient](#create_graphserviceclient)
    - [get_item](#get_item)
    - [delete_item](#delete_item)
    - [create_folder](#create_folder)
    - [upload_file](#upload_file)
    - [upload_folder_from_disk](#upload_folder_from_disk)
    - [process_and_upload_all_topics](#process_and_upload_all_topics)
- [BBC Bitesize - Individual Topics Quizzes](#bbc-bitesize---individual-topics-quizzes)
- [BBC Bitesize - Exam-Style Quizzes](#bbc-bitesize---exam-style-quizzes)
- [BBC Bitesize - Test questions](#bbc-bitesize---test-questions)
- [Wrapper Function `bbc()`](#wrapper-function-bbc)
- [Education Quizzes](#education-quizzes)
- [Primrosekitten](#primrose-kitten)
- [Math Quiz](#math-quiz)
- [Cognitoedu](#cognitoedu)
- [Savemyexams](#savemyexams)
- [Execution](#execution)

## Required Packages
The following packages are required for this project:
1. **beautifulsoup4 v4.12.3** (from bs4): Used for parsing HTML and XML documents.
2. **selenium v4.27.1**: Used for web browser automation.
3. **pandas v2.2.3**: Used for data manipulation and analysis.
4. **xlsxwriter v3.2.0**: Used for creating Excel XLSX files.
5. **requests v2.32.3**: Used for making HTTP requests.
6. **msgraph-sdk v1.16.0** ([Microsoft Graph SDK](https://learn.microsoft.com/en-us/graph/overview)): Used for communicating with [Azure APIs](https://portal.azure.com) (for  automatically uploading files/folders to OneDrive).
7. **json**: Used for working with JSON data.
8. **re**: Provides support for regular expressions.
9. **time**: Provides various time-related functions.

## Utility Functions

This section documents the utility functions used in the web scraping project. These functions handle various tasks such as reading URLs from a file, saving data to JSON and Excel files, and managing the web driver.

---
### get_urls_from_file *(deprecated)*
This function reads **URLs** and **pre-defined attributes** from a text file.

- Parameters: `filename` (str, optional): The name of the input file. Default is 'input_urls.txt'.
- Returns: `tuple`: Contains year_group, subject, sub_topic, and a list of URLs.

#### Behavior:
1. Reads lines from the specified file.
2. Extracts year group, subject, and sub-topic from the first three lines.
3. Collects URLs from the remaining lines.
4. Prints the number of URLs read.
---

### saveJSON
This function saves data to a **JSON file**, useful for testing and debugging.

- Parameters: 
  - `data`: The data to be saved in JSON format.
  - `name` (str, optional): The name of the output JSON file. Default is 'data.json'.

#### Behavior:
1. Opens the specified file in write mode with UTF-8 encoding.
2. Dumps the data into the file in JSON format.
3. Ensures non-ASCII characters are preserved and adds indentation for readability.
---

### loadJSON
This function loads data from a **JSON file** and returns its contents as a Python object. Specifically here is the content file [`input_urls.json`](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/input_urls.json).

#### Parameters
- `name` (str, optional): The name of the JSON file to load. Default is 'data.json'.

#### Returns
- The loaded JSON data as a Python object.
---

### saveSoup
This function saves a **BeautifulSoup object** to an HTML file, useful for testing and debugging.

- Parameters:
  - `soup`: The BeautifulSoup object to be saved.
  - `name` (str, optional): The name of the output HTML file. Default is 'soup.html'.

#### Behavior:
1. Opens the specified file in write mode.
2. Writes the string representation of the BeautifulSoup object to the file.
---

### save_excel
This function saves quiz data to an **Excel file** with data validation for specific columns.

#### Parameters
- `output_file` (str): Name of the Excel file to be created.
- `quiz_data` (list): List of dictionaries containing quiz information.
- `output_folder` (str, optional): Directory where the file will be saved. Default is '.output'.

#### Behavior
- Creates Excel file with quiz data
- Implements dropdown validation for 'Difficulty' column with options: easy, medium, hard
- Implements dropdown validation for 'Type of question' column with predefined question types
---

### load_excel
This function reads the binary content of an **Excel file**, useful for uploading to cloud storage.

#### Parameters
- `file_path` (str): Path to the Excel file.
- `folder` (str, optional): Directory containing the file. Default is '.output'.

#### Returns
- Binary content of the Excel file.
---

### open_driver
This function initializes and opens a new **web driver** instance.

- Parameters: `hide_simulation` (bool, optional): If True, runs the browser in headless mode. Default is True.

#### Behavior:
1. Closes any existing driver instance.
2. Creates Chrome options for the new driver.
3. Sets the browser to run in incognito mode.
4. Sets the browser to run in headless mode if `hide_simulation` is True.
5. Creates a new Chrome WebDriver instance with the specified options.

Note: This function uses the global `driver` variable, which is be declared outside the function scope.

---

### close_driver
This function closes the **web driver**.

#### Behavior:
1. Attempts to close the global `driver` object.
2. Suppresses any exceptions that may occur during the closing process.
---

### javascript_array
This function converts a Python array into **JavaScript array notation**.

#### Parameters
- `array` (list): The Python array to convert.

#### Returns
- String representation of the array in JavaScript format.
- Single items are returned without array brackets.

---

### select_sub_topic
This function extracts a specific subtopic from a nested JSON structure containing content ([`input_urls.json`](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/input_urls.txt).).

#### Parameters
- `json_data` (dict): The complete JSON data structure.
- `key_stage` (str): The key stage identifier.
- `subject` (str): The subject name.
- `year_group` (str): The year group identifier.
- `sub_topic` (str): The subtopic to extract.

#### Returns
- A new dictionary containing only the selected subtopic's data, maintaining the original hierarchy.

---

## *Azure MS-Graph* APIs
This section documents the Azure Microsoft Graph API wrapper functions used for OneDrive operations. These functions handle authentication and file management tasks such as creating, uploading, and deleting files/folders.

---
### create_GraphServiceClient
This function creates a **GraphServiceClient** instance for authenticating with Microsoft Graph API.

#### Parameters
- `azure_credentials` (str, optional): Path to JSON file containing Azure credentials. Default is 'azure_credentials.json'.

#### Returns
- GraphServiceClient instance configured with the specified credentials and scopes.

#### Behavior
1. Loads credentials from JSON file
2. Sets up authentication with DeviceCodeCredential
3. Configures access scopes for file operations
---

### get_item
This function retrieves an **item** from OneDrive by its ID or name.

#### Parameters
- `ID` (str, optional): The item's unique identifier
- `name` (str, optional): The item's name
- `parent_folder_id` (str, optional): ID of the parent folder. Default is content_folder_id.

#### Returns
- DriveItem object if found, False otherwise
---

### delete_item
This function deletes an **item** from OneDrive.

#### Parameters
- `ID` (str): The unique identifier of the item to delete

#### Returns
- Boolean indicating success (True) or failure (False)
---

### create_folder
This function creates a new **folder** in OneDrive.

#### Parameters
- `folder_name` (str): Name of the folder to create
- `parent_folder_id` (str, optional): ID of the parent folder. Default is content_folder_id.
- `force_delete` (bool, optional): If True, deletes existing folder with same name. Default is False.

#### Returns
- DriveItem object representing the created folder
---

### upload_file
This function uploads a **file** to OneDrive.

#### Parameters
- `file_name` (str): Name of the file to upload
- `file_content` (bytes): Binary content of the file
- `folder_id` (str, optional): ID of the destination folder. Default is content_folder_id.

#### Returns
- DriveItem object representing the uploaded file
---

### upload_folder_from_disk
This function uploads all files from a local **folder** to OneDrive.

#### Parameters
- `folder_path` (str): Path to local folder containing files to upload
- `parent_folder_id` (str, optional): OneDrive folder ID where files will be uploaded. Default is content_folder_id.
- `files_only` (bool, optional): If True, uploads files without creating a new folder. Default is False.

#### Returns
- DriveItem object representing the parent folder
---

### process_and_upload_all_topics
This function processes educational content from JSON data and uploads to OneDrive.

#### Parameters
- `json_data` (dict): Nested dictionary containing educational content
- `drive_id` (str): The ID of the OneDrive instance
- `output_folder_id` (str): The target folder ID in OneDrive

#### Behavior
1. Retrieves and validates the output folder
2. Iterates through the nested JSON structure
3. For each topic with valid URLs:
   - Extracts quiz data
   - Saves to Excel file
   - Uploads to OneDrive


## BBC Bitesize - Individual Topics Quizzes
For <b>BBC</b> links that contains the embeded links format: `https://www.riddle.com/embed/a/[a-zA-Z0-9]*[a-zA-Z]+`

<div style="text-align: center;">

[<img src="attachment:9424a361-2641-4935-b0f8-0be17265d610.png" width="30%">

Sample Link: [`https://www.bbc.co.uk/bitesize/topics/zck96rd/articles/zv7b8p3`](https://www.bbc.co.uk/bitesize/topics/zck96rd/articles/zv7b8p3)

</div>

#### Code Overview

The `bbc_simulation` function performs the following tasks:

- **Input Parameters**:
  - `url`: The BBC Bitesize URL to scrape.
  - `embed_urls`: List of embedded quiz URLs extracted from the page.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Quiz Detection**: If no quizzes are found, the function prints an appropriate message. If quizzes are found, it loops through the embedded URLs.
  - **Page Navigation**: The function navigates through each embedded quiz URL using Selenium, waits for the page elements to load, and extracts the quiz content.
  - **Question Handling**: The function handles different types of quiz questions:
    - **Multiple Choice Questions (MCQ)**: Extracts question text, options, images (if present), and correct answers.
    - **Order Questions**: Extracts and handles questions that require the user to order items.
    - **Text Entry**: Handles questions where the answer needs to be typed in by the user.
    - **Tap and Find**: Handles questions that require users to click on a canvas element.
  - **Answer Submission**: The function attempts to submit answers for the different types of questions, either by clicking options or inputting text.
  - **Result Handling**: Once the quiz is completed, the data (e.g., question title, options, answers, images) is appended to the `quiz_data` list.

#### Detailed Walkthrough of Key Steps

1. **Switch to iFrame**: The function switches to the iframe containing the quiz.
    ```python
   driver.switch_to.frame(iframe)
    ```

2. **Scroll to the Quiz Section**: The main quiz content is brought into view.
    ```python
   driver.execute_script("arguments[0].scrollIntoView();", main_element)
   ```

3. **Handle Different Question Types:**:
    - **Multiple Choice (MCQ)**: For questions with multiple choice, it scrapes both the text and images (if available) for each option.
    - **Order Questions**: For questions requiring the user to arrange options in order.
    - **Text Entry**: Allows input of text into a designated field for user-submitted answers.
    - **Tap and Find**: The function simulates a click on the canvas to answer.

4. **Next Button Handling**: The function simulates clicking the "Next" button to proceed through the quiz.
    ```python
    next_button = driver.find_elements(By.XPATH, "//button[@data-test='next-btn']")
   ```

5. **Error Handling**: If an error occurs (e.g., page elements not loading), the function logs the error and stops the current quiz extraction process for that URL.
    ```python
    except Exception as e:
        print(f"Quiz completed for embed URL:{embed_url}{e} | Quizzes: {len(quiz_data) - pre_len} | Total:{len(quiz_data)}")
   ```
---

## BBC Bitesize - Exam-Style Quizzes
For **BBC** links that contain the embedded links format: `https://www.riddle.com/embed/a/[0-9]+`

<div style="text-align: center;">

<img src="attachment:f3edd3c8-3c7c-44f2-af93-16b03bcf1c64.png" width="30%"><br> Sample Link: [`https://www.bbc.co.uk/bitesize/articles/zwdjmbk`](https://www.bbc.co.uk/bitesize/articles/zwdjmbk)

</div>

### Code Overview

The `bbc_exam_style_quizzes` function performs the following tasks:

- **Input Parameters**:
  - `url`: The BBC Bitesize URL to scrape.
  - `embed_urls`: List of embedded quiz URLs extracted from the page.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Quiz Detection**: If no quizzes are found, the function prints a message indicating the absence of quizzes. If quizzes are found, it proceeds to extract quiz data from each embedded URL.
  - **Embed URL Processing**: For each embedded URL, it sends a request to the URL and extracts the JSON data containing quiz content.
  - **Question Handling**: The function handles different types of quiz questions:
    - **Multiple Choice Questions (MCQ)**: Extracts question text, options, and correct answers.
    - **Order Questions**: Extracts and handles questions that require ordering items.
    - **Text Entry**: Handles questions that require text answers from users.
  - **Result Logging**: After processing each quiz, the data is appended to the `quiz_data` list, and the number of quizzes extracted is logged.

### Detailed Walkthrough of Key Steps

1. **Extract JSON Data from the Embedded Page**: The function sends an HTTP request to each embedded quiz URL and retrieves the JSON data containing the quiz content.

    ```python
    embed_response = requests.get(embed_url)
    start_index = embed_response.text.find('data:{') + len('data:')
    end_index = embed_response.text.find('},\n', start_index) + 1
    json_data = json.loads(embed_response.text[start_index:end_index])
    ```

2. **Process Each Page Group**: The function iterates through the `pageGroups` in the JSON data, which contain the quiz questions.

    ```python
    for page_group in json_data['data']['pageGroups']:
    ```

3. **Handle Different Question Types**:
    - **Multiple Choice Questions (MCQ)**: Extracts the question title, options, and correct answers. If images are associated with the options, they are included as well.

      ```python
      if page_group['templateId'] == 'quiz-question':
          question_type = 'MCQ'
          options_list = []
          answer = []
          for item in page['allAnswers']:
              label = item.get('label', '')
              image_url = item['image']['original'] if item.get('image') else ''
              options_list.append(label)
              if image_url:
                  options_list[-1] += f":{image_url}" if label else image_url
      ```

    - **Order Questions**: Handles questions where the correct answer is a specific order of items.

      ```python
      elif page_group['templateId'] == 'lineup-question':
          question_type = 'Order'
          order_answers = sorted(page['lineupConfig']['listItems'], key=lambda x: x['correctIndex'])
          options_list = []
          answer = []
          for item in order_answers:
              answer.append(item['title'])
              if item.get('image'):
                  answer[-1] += f":{item['image']['original']}" if item['title'] else item['image']['original']
      ```

    - **Text Entry**: Handles questions where the user needs to enter a text answer.

      ```python
      if question_type == 'freetext':
          question_type = 'TextEntry'
          options_list = []
          answer = page.get('textAnswers', page.get('currentTextAnswer', ''))
      ```

4. **Handle Question Bank Template**: For questions stored in the `question-bank` template, the function extracts question titles, options, and correct answers differently from the quiz template.

    ```python
    elif page_group['templateId'] == 'question-bank':
        start_index = embed_response.text.find('questionBankData:') + len('questionBankData:')
        end_index = embed_response.text.find('}]', start_index) + 2
        json_data = json.loads(embed_response.text[start_index:end_index])
        
        for item in json_data:
            question_type = 'MCQ'
            question_title = item['question']
            options_list = item['answers']
            answer = [item['answerCorrectValue']]
            other_text = item['answerExplanation']
    ```
---

## BBC Bitesize - Test questions
For **BBC** links that contain the embedded links format: `https://bitesize.files.bbci.co.uk/acme-quiz/[a-zA-Z0-9-.]+`<br>
<div style="text-align: center;">

<img src="attachment:9f5e1b22-c46c-48d2-8c8a-28c24defbd34.png" width="30%"><br> Sample Link: [`https://www.bbc.co.uk/bitesize/guides/z3s4qhv/test`](https://www.bbc.co.uk/bitesize/guides/z3s4qhv/test)
</div>

### Code Overview

The `bbc_acme_quiz` function performs the following tasks:

- **Input Parameters**:
  - `url`: The BBC Bitesize URL to scrape.
  - `embed_urls`: List of embedded quiz URLs extracted from the page.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Quiz Detection**: If no quizzes are found, the function prints a message indicating the absence of quizzes. If quizzes are found, it proceeds to extract quiz data from each embedded URL.
  - **Extracting Questions**: The function extracts question data including text, options, and answers from each embedded quiz.
  - **Handling Superscripts**: Superscripts in questions are converted to the appropriate format using a predefined mapping.

### Detailed Walkthrough of Key Steps

1. **Check for Embedded URLs**: If no embedded URLs are provided, the function logs that no quiz was found and returns.

    ```python
    if not embed_urls:
        print(f"No quiz found in {url}")
        return
    else:
        print(f"Extracting {len(embed_urls)} embed link(s) in:{url}")
        pre_len = len(quiz_data)
    ```

2. **Superscript Mapping**: A dictionary maps characters to their superscript equivalents, allowing for correct formatting of question text.

    ```python
    SUPERSCRIPT_MAP = {
        '0': '⁰', '1': '¹', '2': '²', '3': '³', '4': '⁴', '5': '⁵', '6': '⁶',
        '7': '⁷', '8': '⁸', '9': '⁹', 'a': 'ᵃ', 'b': 'ᵇ', 'c': 'ᶜ', 'd': 'ᵈ',
        'e': 'ᵉ', 'f': 'ᶠ', 'g': 'ᵍ', 'h': 'ʰ', 'i': 'ⁱ', 'j': 'ʲ', 'k': 'ᵏ',
        'l': 'ˡ', 'm': 'ᵐ', 'n': 'ⁿ', 'o': 'ᵒ', 'p': 'ᵖ', 'r': 'ʳ', 's': 'ˢ',
        't': 'ᵗ', 'u': 'ᵘ', 'v': 'ᵛ', 'w': 'ʷ', 'x': 'ˣ', 'y': 'ʸ', 'z': 'ᶻ',
        '/': '′',
    }
    ```

3. **Extract Text with Superscript**: This function processes an HTML element to extract text, converting any superscripts as defined in the mapping.

    ```python
    def extract_text_with_sup(element):
        text_parts = []
        pre_elem_name = None
        for elem in element.descendants:
            if pre_elem_name == 'sup':
                pre_elem_name = elem.name
                continue
                
            if isinstance(elem, str):
                text_parts.append(elem.strip())
            elif elem.name == 'sup':
                text_parts.append(''.join(SUPERSCRIPT_MAP.get(char, char) for char in elem.get_text(strip=True)))
            pre_elem_name = elem.name
        return ''.join(text_parts)
    ```

4. **Process Each Embedded URL**: For each URL, the function sends a request and parses the response using BeautifulSoup.

    ```python
    for embed_url in embed_urls:
        embed_response = requests.get(embed_url)
        soup = BeautifulSoup(embed_response.content, 'html.parser')
        
        start_index = embed_response.text.find('"chapterData":') + len('"chapterData":')
        end_index = embed_response.text.find('}}}', start_index) + 1
        json_data = json.loads(embed_response.text[start_index:end_index])
        
        questions = soup.find_all(class_='question')
    ```

5. **Extract Questions**: For each question, the function retrieves the question number and text. It also extracts any images and options based on the question type.

    ```python
    for question in questions:
        question_number = question.find(class_='question-number').get_text(strip=True)
    
        # Extract the full question text, including handling <sup> tags for exponents
        question_text = extract_text_with_sup(question.find(class_='question-prompt'))
    
        image_tag = question.find('img')
        image_url = f"{image_tag['srcset']}" if image_tag else None
    
        if 'question--radio' in question['class']:  
            options = question.find_all(class_='radio-answer')
            options = [extract_text_with_sup(opt.find('span')) for opt in options]
        elif 'question--select' in question['class']:
            options = question.find('select').find_all('option')
            options = [opt.get_text(strip=True) for opt in options if opt.get('value') and opt.get('value') != '']
    ```

6. **Determine Correct Answer**: The function checks the responses from the JSON data to find the correct answer based on the options extracted.

    ```python
    cur_json_data = json_data["questions"][int(question_number) - 1]
    answer = None
    for idx, response in enumerate(cur_json_data["responses"]):
        if response["score"] == "1":
            answer = options[idx]
    ```
---

## Wrapper Function `bbc()`

The `bbc` function is designed to determine and categorize different types of BBC links found in a given URL. It identifies whether the links are for simulations, exam styles, or Acme quizzes and processes them accordingly.

### Parameters

- **url** (`str`): The URL to be checked for BBC links.
- **quiz_data** (`any`): Data related to the quizzes to be processed.

### Link Formats

The function looks for three specific link formats:

1. **Simulation Format**: 
   - Regular Expression: `https://www.riddle.com/embed/a/[a-zA-Z0-9]*[a-zA-Z]+`
   - This format matches links for interactive simulations (`bbc_simulation()`).

2. **Exam Style Format**: 
   - Regular Expression: `https://www.riddle.com/embed/a/[0-9]+`
   - This format matches links for exam-style quizzes (`bbc_exam_style_quizzes()`).

3. **Acme Quiz Format**: 
   - Regular Expression: `https://bitesize.files.bbci.co.uk/acme-quiz/[a-zA-Z0-9-.]+`
   - This format matches links for Acme quizzes (`bbc_acme_quiz()`).

---

### Function Logic

1. **Send a GET request** to the specified `url` using:
    ```python
    response = requests.get(url)
    ```

2. **Search for Link Formats**:
   - Use regular expressions to find matches in the response text.
   - If matches are found, corresponding functions are called to process the links.

3. **Processing Functions**:
   - **Simulation Links**: 
     - If the simulation format is found:
       ```python
       embed_urls = re.findall(simulation_format, response.text)
       bbc_simulation(url, embed_urls, quiz_data)
       ```
   - **Exam Style Links**: 
     - If the exam style format is found:
       ```python
       embed_urls = re.findall(exam_style_format, response.text)
       bbc_exam_style_quizzes(url, embed_urls, quiz_data)
       ```
   - **Acme Quiz Links**: 
     - If the Acme quiz format is found:
       ```python
       embed_urls = re.findall(acme_quiz_format, response.text)
       bbc_acme_quiz(url, embed_urls, quiz_data)
       ```
---

## Education Quizzes
For **Educationquizzes** links format: `educationquizzes.com`<br>
<div style="text-align: center;">

<img src="attachment:b21a625e-6ec7-4bf5-9632-d420879d9cb1.png" width="30%"><br> Sample Link: [`https://www.educationquizzes.com/gcse/physics/waves-the-expanding-universe/`](https://www.educationquizzes.com/gcse/physics/waves-the-expanding-universe/)
</div>

### Code Overview

The `education_quizzes` function performs the following tasks:

- **Input Parameters**:
  - `url`: The Education Quizzes URL to scrape.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Request and Parse**: The function sends a request to the specified URL and parses the HTML content to extract quiz questions.
  - **Quiz Extraction**: It loops through each question, extracting necessary information, including the question text, options, and answers.

### Detailed Walkthrough of Key Steps

1. **Set Up the Base Link**: The function defines the base URL for Education Quizzes.

    ```python
    head_link = "https://www.educationquizzes.com"
    ```

2. **Send Request and Parse HTML**: The function sends a GET request to the provided URL and uses BeautifulSoup to parse the content.

    ```python
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    ```

3. **Find Quiz Questions**: It locates all elements that contain quiz questions.

    ```python
    questions = soup.find_all(class_='quiz__question__content')
    ```

4. **Loop Through Each Question**: The function iterates over each question and extracts the required details.

    ```python
    for question in questions:
        question_text = question.find(class_='quiz__question__question')
        question_text = f"{head_link}{question_text.find('img')["src"]}" if question_text.find('img') else question_text.get_text().strip()
    ```

5. **Extract Image URL**: It checks for an image associated with the question and constructs the full URL if present.

    ```python
    image_tag = question.find(class_='quiz__question__image__link')
    image_url = f"{head_link}{image_tag['href']}" if image_tag else None
    ```

6. **Extract Answer Options**: The function retrieves the answer options and identifies the correct answer.

    ```python
    options = question.find_all(class_='quiz__question__answers__answer')
    answers = next((option for option in options if option['data-iscorrect'] == 'true'), None)
    answers = f"{head_link}{answers.find('img')["src"]}" if answers.find('img') else answers.get_text().strip()
    ```

7. **Format Options**: It formats the options, ensuring any images are included with their respective URLs.

    ```python
    options = [f"{head_link}{option_text.find('img')["src"]}" if option_text.find('img') else option_text.get_text().strip() for option_text in options]
    ```

8. **Extract Additional Text**: If any additional helpful comments are present, the function extracts them too.

    ```python
    other_text = question.find(class_='quiz__question__result__helpful-comment') if question.find(class_='quiz__question__result__helpful-comment') else ''
    if other_text:
        other_text = f"{head_link}{other_text.find('img')["src"]}" if other_text.find('img') else other_text.get_text().strip()
    ```

---

## Primrose Kitten
For **Primrosekitten** links format: `primrosekitten.org`<br>
<div style="text-align: center;">

<img src="attachment:57dc019d-5ef6-430e-8111-7960277f3e49.png" width="30%"><br> Sample Link: [`https://primrosekitten.org/courses/a-level-maths/lessons/algebra-and-functions/quizzes/as-level-maths-surds-hard/`](https://primrosekitten.org/courses/a-level-maths/lessons/algebra-and-functions/quizzes/as-level-maths-surds-hard/)
</div>

### Code Overview

The `primrosekitten` function performs the following tasks:

- **Input Parameters**:
  - `url`: The Primrose Kitten URL to scrape.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Request and Parse**: The function sends a request to the specified URL and parses the HTML content to extract quiz questions.
  - **Quiz Extraction**: It loops through each question, extracting necessary information, including the question text, options, and answers.

### Detailed Walkthrough of Key Steps

1. **Send Request and Parse HTML**: The function sends a GET request to the provided URL and uses BeautifulSoup to parse the content.

    ```python
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    ```

2. **Find Quiz Questions**: It locates all elements that contain quiz questions.

    ```python
    questions = soup.find_all(class_='wpProQuiz_listItem')
    ```

3. **Loop Through Each Question**: The function iterates over each question and extracts the required details.

    ```python
    for question in questions:
        question_text = question.find(class_='wpProQuiz_question_text').get_text(strip=True)
    ```

4. **Extract Image URL**: It checks for an image associated with the question and extracts the URL if present.

    ```python
    image_tag = question.find('img')
    image_url = image_tag['src'] if image_tag else None
    ```

5. **Extract Answer Options**: The function retrieves the answer options from each question.

    ```python
    options = question.find_all(class_='wpProQuiz_questionListItem')
    options = [option.get_text(strip=True) for option in options]
    ```

6. **Identify the Correct Answer**: It finds the correct answer by checking for the data attribute `data-pos="0"`.

    ```python
    answers = next((option.get_text(strip=True) for option in options if option['data-pos'] == '0'), None)
    ```

---

## Math Quiz
For **Math Quiz** links format: `math-quiz.co.uk`<br>
<div style="text-align: center;">

<img src="attachment:4ccbe1ea-c38f-4c8a-9650-f27774cbdc17.png" width="30%"><br> Sample Link: [`https://math-quiz.co.uk/a-level-maths/algebra/a-level-inequalities-1-easy.html`](https://math-quiz.co.uk/a-level-maths/algebra/a-level-inequalities-1-easy.html)
</div>

### Code Overview

The `math_quiz` function performs the following tasks:

- **Input Parameters**:
  - `url`: The Math Quiz URL to scrape.
  - `quiz_data`: A list to store the extracted quiz data.

- **Main Operations**:
  - **Embed URL Extraction**: The function checks if the provided URL is a test URL or retrieves the test URLs from the main page.
  - **Quiz Extraction**: It navigates to each embedded quiz, extracts question titles, options, and the correct answer.

### Detailed Walkthrough of Key Steps

1. **Set Base URL**: The function defines the base URL for the Math Quiz site.

    ```python
    head_link = "https://math-quiz.co.uk/"
    embed_urls = []
    ```

2. **Extract Embed URLs**: It checks if the provided URL contains `"start_test.php"` or fetches URLs from the main page.

    ```python
    if "start_test.php" in url:
        embed_urls += [url]
    else:
        response = requests.get(url)
        embed_urls = [f"{head_link}{page}" for page in re.findall(r'start_test.php\?id=[a-zA-Z0-9\-\.]+', response.text)]
    ```

3. **Iterate Through Embed URLs**: The function loops through each embed URL to extract quiz data.

    ```python
    for embed_url in embed_urls:
        response = requests.get(embed_url)
        soup = BeautifulSoup(response.text, 'html.parser')
    ```

4. **Extract Quizzes**: It retrieves quiz elements from the HTML and logs the number of quizzes being extracted.

    ```python
    quizzes = list(soup.find('div', id="maths-test").children)[1::2]
    print(f"Extracting {len(quizzes)} quizzes from:{url}", end='')
    pre_len = len(quiz_data)
    ```

5. **Loop Through Each Quiz**: The function processes each quiz to extract questions and options.

    ```python
    for quiz in quizzes:
        quiz = list(quiz.children)[1::2]
        question_title = f"{quiz[0].find('b').next_sibling.strip()} {quiz[0].find('math').decode().replace('\n', '') if quiz[0].find('math') else ''}"
    ```

6. **Extract Image URL**: It retrieves the image URL associated with the question, if present.

    ```python
    image_url = f"{head_link}{quiz[0].find('img')['src']}" if quiz[0].find('img') else None
    ```

7. **Extract Answer Options**: The function collects options and identifies the correct answer based on the smallest ID value.

    ```python
    options = []
    correct_answer_idx = 0
    smallest_answer_id = quiz[1].find('input', type='hidden')['value']
    for idx, answer_div in enumerate(quiz[1:]):
        options.append(answer_div.find('math').decode().replace('\n', '') if answer_div.find('math') else '')
        if answer_div.find('img'):
            options[-1] += f":{head_link}{answer_div.find('img')['src']}" if options[-1] else f"{head_link}{answer_div.find('img')['src']}"
        
        answer_id = answer_div.find('input', type='hidden')['value']
        if smallest_answer_id > answer_id:
            smallest_answer_id = answer_id
            correct_answer_idx = idx
    ```

---

## Cognitoedu
For **Cognitoedu** links format: `cognitoedu.org`<br>
<div style="text-align: center;">

<img src="attachment:37c4e629-c908-4e7b-b25a-c30cdfe2fd7f.png" width="30%"><br> Sample Link: [`https://cognitoedu.org/courserevise/b3-alevel-aqa/b3-alevel-aqa_ClHRwUVV`](https://cognitoedu.org/courserevise/b3-alevel-aqa/b3-alevel-aqa_ClHRwUVV)
</div>

### Code Overview

The `cognitoedu` function is designed to scrape quiz data from the Cognitoedu platform. It uses Selenium for interaction and BeautifulSoup for parsing HTML content.

- **Input Parameters**:
  - `url`: The Cognitoedu URL to scrape.
  - `quiz_data`: A list to store the extracted quiz data.

### Detailed Walkthrough of Key Steps

1. **Define Helper Function**: A nested function `get_soup_from_simulation` handles page interactions and returns the parsed HTML content.

    ```python
    def get_soup_from_simulation():
        driver.get(url)
        driver.execute_script("document.body.style.zoom='5%'")
        ...
    ```

2. **Clicking Buttons**: The function waits for and clicks the "All at once" button, and submits all enabled buttons on the page.

    ```python
    at_at_once_button = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.XPATH, "//button[.//p[text()='All at once']]"))
    )
    at_at_once_button.click()
    ```

3. **Waiting for Quiz Elements**: It waits for specific elements to load on the page before proceeding.

    ```python
    buttons = WebDriverWait(driver, 10).until(EC.presence_of_all_elements_located((By.XPATH, "//button[.//p[text()='Submit' or text()='Mark']]")))
    ```

4. **Get Page Source**: After interactions, it retrieves the page source and parses it using BeautifulSoup.

    ```python
    soup = get_soup_from_simulation()
    while not soup:
        open_driver()
        soup = get_soup_from_simulation()
    ```

5. **Extract Quizzes**: The function identifies and counts the quizzes present on the page.

    ```python
    quizzes = soup.find_all('div', class_='jss1410')
    print(f"Extracting {len(quizzes)} quizzes from:{url}", end='')
    pre_len = len(quiz_data)
    ```

6. **Process Each Quiz**: It loops through each quiz to extract details such as the question title, type, and options.

    ```python
    for quiz in quizzes:
        if quiz.find('canvas'):  # Skipping interaction questions
            continue
        question_type = 'TextEntry' if quiz.find(('textarea', 'input')) else 'MCQ'
        question_title = ' '.join([ques.decode_contents().strip() for ques in quiz.find_all(class_=('mathFieldWrapper', 'ML__base'))[0].find_all('p')])
    ```

7. **Handle Hidden Text**: It replaces hidden text in the question title with underscores.

    ```python
    for hidden in quiz.find_all(class_='jss838'):
        question_title = question_title.replace(hidden.get_text(), '_' * len(hidden.get_text()))
    ```

8. **Extract Image and Answers**: It retrieves the image URL and answers from the quiz.

    ```python
    image_url = quiz.find('img')['src'] if quiz.find('img') else None
    answers = [quiz.find(class_='jss835').get_text(strip=True)] if quiz.find(class_='jss835') else []
    ```

9. **Collect Options and Other Text**: The function collects answer options and any additional text.

    ```python
    for idx, answer_div in enumerate(quiz.find_all(class_=('mathFieldWrapper', 'ML__base'))[1:]):
        if answer_div.findParent(class_=('jss855', 'jss991')):
            answers.append(answer_div.decode_contents().strip())
            options.append(answer_div.decode_contents().strip()) if question_type != 'TextEntry' else None
        elif answer_div.findParent(class_='jss851'):
            options.append(answer_div.decode_contents().strip())
        elif idx == len(quiz.find_all(class_=('mathFieldWrapper', 'ML__base'))) - 2:
            other_text = answer_div.decode_contents().strip()
    ```
---

## Savemyexams
For **Savemyexams** links format: `savemyexams.com`<br>
<div style="text-align: center;">

<img src="attachment:95cea78a-eb36-48fe-8960-6499c7a7072b.png" width="30%"><br> Sample Link: [`https://www.savemyexams.com/a-level/chemistry/aqa/17/topic-questions/3-organic-chemistry/3-2-alkanes`](https://www.savemyexams.com/a-level/chemistry/aqa/17/topic-questions/3-organic-chemistry/3-2-alkanes)
</div>

### Code Overview

The `savemyexams` function is designed to scrape quiz data from the Savemyexams platform. It utilizes the `requests` library to fetch web content and `BeautifulSoup` for parsing HTML.

- **Input Parameters**:
  - `url`: The Savemyexams URL to scrape.
  - `quiz_data`: A list to store the extracted quiz data.

### Detailed Walkthrough of Key Steps

1. **Fetch Page Content**: The function sends a GET request to the specified URL.

    ```python
    response = requests.get(url)
    ```

2. **Extract JSON Data**: It parses the response content with BeautifulSoup and extracts JSON data embedded within the page.

    ```python
    soup = BeautifulSoup(response.content, 'html.parser')
    json_data = json.loads(soup.find(id='__NEXT_DATA__').decode_contents())
    ```

3. **Collect Quizzes**: The function gathers all quizzes from the extracted JSON data.

    ```python
    quizzes = [data['pages'] for data in json_data['props']['pageProps']['posts']]
    print(f"Extracting {sum(len(level) for quiz in quizzes for level in quiz)} quizzes from:{url}", end='')
    pre_len = len(quiz_data)
    ```

4. **Iterate Through Quizzes**: It loops through each quiz and quiz level to extract relevant information.

    ```python
    for quiz in quizzes:
        for quiz_level in quiz:
            for cur_quiz in quiz_level:
                difficulty = cur_quiz['difficulty']
                question_type = cur_quiz['type'].upper()
    ```

5. **Determine Question Type**: The question type is determined, and if it is not "MCQ", it is classified as 'TextEntry'.

    ```python
    if question_type != "MCQ":
        question_type = 'TextEntry'
    ```

6. **Parse Question Title**: The question body is parsed, and any images are processed.

    ```python
    title = BeautifulSoup(cur_quiz['problem'][0]['body'], 'html.parser')
    image_url = ''
    for img_tag in title.find_all('img'):
        if img_tag['src'][:5] != 'data:':
            image_url = img_tag['src']
            img_tag.decompose()
    question_title = title.decode_contents().replace('\n', '')
    ```

7. **Extract Options and Answers**: Depending on the question type, the options and answers are extracted.

    ```python
    if question_type == 'MCQ':
        for key, value in cur_quiz.items():
            if 'choice' in key:
                options.append(BeautifulSoup(value[0]['body']).find('p').decode_contents() if value else key[-1])
        answers = cur_quiz['choice' + cur_quiz['correctChoice']]
        answers = cur_quiz['correctChoice'] if not answers else [BeautifulSoup(answers[0]['body']).find('p').decode_contents()]
    elif question_type == 'TextEntry':
        answers = [cur_quiz['solution'][0]['body'].replace('\n', '')]
    ```

8. **Record Marks**: The marks associated with the quiz are stored.

    ```python
    mark = cur_quiz['marks']
    ```
---

## Execution
This section documents the execution steps involved in extracting quizzes from various sources based on provided URLs.

### Extract Quizzes Based on the Sources
This function extracts quizzes from different educational websites based on the provided URLs.

- **Code**:
    ```python
    def extract_quizzes(urls, quiz_data, hide_simulation=True):
        print(f'{year_group} - {subject} - {sub_topic}')

        open_driver(hide_simulation)
        for idx, url in enumerate(urls):
            print(f"{idx+1}) ", end='')
            if "bbc.co.uk" in url:
                bbc(url, quiz_data)
            elif "educationquizzes.com" in url:
                education_quizzes(url, quiz_data)
            elif "primrosekitten.org" in url:
                primrosekitten(url, quiz_data)
            elif "math-quiz.co.uk" in url:
                math_quiz(url, quiz_data)
            elif "cognitoedu.org" in url:
                cognitoedu(url, quiz_data)
            elif "savemyexams.com" in url:
                savemyexams(url, quiz_data)
            else:
                print(f" **Error: Did not find any source for {url}")

        close_driver()
    ```

- **Parameters**:
    - `urls` (list): A list of URLs to extract quizzes from.
    - `quiz_data` (list): A list to store the extracted quiz data.
    - `hide_simulation` (bool, optional): A flag to control the visibility of the browser during extraction. Default is `True`.

- **Returns**: 
    - None (the function modifies `quiz_data` in place).

#### Behavior:
1. Prints the current year group, subject, and sub-topic.
2. Opens a web driver for scraping.
3. Iterates through the provided URLs:
    - Calls the appropriate function based on the URL domain to extract quiz data.
    - Prints an error message if no matching source is found.
4. Closes the web driver after processing all URLs.

---

### Execution of Quiz Extraction
This section shows how to execute the quiz extraction process using the `extract_quizzes` function.

- **Code**:
    ```python
    year_group, subject, sub_topic, urls = get_urls_from_file()
    quiz_data = []

    extract_quizzes(urls, quiz_data)

    output_file = f"{sub_topic.lower()} {year_group.lower()} {subject.lower()}.xlsx"
    save_excel(output_file, quiz_data)
    ```

- **Returns**: 
    - An Excel file containing the extracted quiz data.

#### Behavior:
1. Retrieves the year group, subject, sub-topic, and URLs from the input file using `get_urls_from_file()`.
2. Initializes an empty list `quiz_data` to store the extracted quizzes.
3. Calls `extract_quizzes` to populate `quiz_data` with quizzes from the specified URLs.
4. Constructs an output filename based on the subject, year group, and sub-topic.
5. Calls `save_excel` to save the extracted quiz data to an Excel file.

---
