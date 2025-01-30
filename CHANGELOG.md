# Changelog
v1.6 - 30/01/2025
- Changes:
  - Updated [`input_urls.json`](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/input_urls.json) for improved content organization and mass quiz reproduction:
    - Grouped **Years 7-8-9** under **KS3**
    - Grouped **Years 10-11** under **GCSE**
    - Grouped **Years 12-13** under **A-Levels**
  - Updated the scraper function for [Savemyexams](https://www.savemyexams.com):
    - Modified the scraper to accommodate the website's updated structure, ensuring continued compatibility and accurate data extraction

v1.5 - 13/01/2025
- New:
  - Quizzes Scraper now automatically uploads files to [`Learnspot Content`](https://mujahidrashid-my.sharepoint.com/personal/shargel_tariq_learnspot_co_uk/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fshargel_tariq_learnspot_co_uk%2FDocuments%2FLearnSpot%20Content) folder using [Microsoft Graph SDK](https://learn.microsoft.com/en-us/graph/overview)
  - Replaced `input_urls.txt` with [`input_urls.json`](https://github.com/ThongLai/Learnspot-content-scraping/blob/main/quiz_scraper/input_urls.json) for better content organization and mass quiz reproduction (Currently only for Quiz Scraper)
  - Added `.output` folder for storing files from both scrapers (will be hidden when pushing into branch)
  - Included library version specifications in both scraper documentation files
- Changes:
  - Renamed *venv* folders to use leading dots (hidden files convention)
  - New version of [`selenium`](https://pypi.org/project/selenium/) can be used (upgraded from `v4.26.1` to `v4.27.1`)
  - Enclosed `options` and `answers` in $ ... $ for math formula formatting for **Primrose Kitten** urls
  - Enhanced quiz loading consistency with improved wait times for **BBC Bitesize** urls
- Known Issues:
  - Potential printing bug in `Python 3.12.8` (Testing needed with earlier Python versions)


v1.4 - 29/11/2024
- Changes: 
  - Some math symbols notation changed into HTML tag (e.g.: `°` into `<sup>`) for *BBC Bitesize* and *EducationQuizzes*

v1.3 - 19/11/2024
- New:
  - The [`.beta`](https://github.com/ThongLai/Learnspot-content-scraping/tree/main/.beta/) directory now includes new files for testing.
  - Testing on the [Azure Portal](https://portal.azure.com) and [Microsoft Graph APIs](https://learn.microsoft.com/en-us/graph/overview) located at [`.beta/.onedrive_access_apis_testing`](https://github.com/ThongLai/Learnspot-content-scraping/tree/main/.beta/.onedrive_access_apis_testing). These tools facilitate file manipulation in OneDrive, specifically for accessing the [LearnSpot Content Folder](https://mujahidrashid-my.sharepoint.com/personal/shargel_tariq_learnspot_co_uk/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fshargel_tariq_learnspot_co_uk%2FDocuments%2FLearnSpot%20Content&ga=1).

v1.2 - 12/11/2024
- Changes: 
  - Remove format of include both text and images in `Options` and `Answer`. New format example file: [geometry ks3 maths.xlsx](https://mujahidrashid-my.sharepoint.com/:x:/g/personal/shargel_tariq_learnspot_co_uk/Efcccnyt2-pBheiLbYipflEBztpPNhgsRxnGNF24-uMfmA?e=DINWC2)
  - Update `requirements.txt`: Now contains lesser library identifiers which is easier to install library.
  - Update `.gitignore`: Excel files produced by scraper will be ignored when commit

v1.1 - 12/11/2024
- Changes:
  - Implemented changes based on the [new format proposal](https://docs.google.com/document/d/1j3eZygtLekdJfdZIvB60yKXwzuPoZw8zx4ZhbuWQYeA/edit?usp=sharing)
  - Change `Type of question` format indicators.
   - Updated format for adding multiple values in a single column are now format these values as JavaScript arrays `["item 1", "item 2", "item 3", "item 4"]`.