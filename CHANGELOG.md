# Changelog

v1.4 - 29/11/2024
- Fix: Some math symbols notation changed into HTML tag (e.g.: `°` into `<sup>`) for *BBC Bitesize* and *EducationQuizzes*

v1.3 - 19/11/2024
- New:
  - The [`.beta`](https://github.com/ThongLai/Learnspot-content-scraping/tree/main/.beta/) directory now includes new files for testing.
  - Testing on the [Azure Portal](https://portal.azure.com) and [Microsoft Graph APIs](https://learn.microsoft.com/en-us/graph/overview) located at [`.beta/.onedrive_access_apis_testing`](https://github.com/ThongLai/Learnspot-content-scraping/tree/main/.beta/.onedrive_access_apis_testing). These tools facilitate file manipulation in OneDrive, specifically for accessing the [LearnSpot Content Folder](https://mujahidrashid-my.sharepoint.com/personal/shargel_tariq_learnspot_co_uk/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fshargel_tariq_learnspot_co_uk%2FDocuments%2FLearnSpot%20Content&ga=1).

v1.2 - 12/11/2024
- Changed: 
  - Remove format of include both text and images in `Options` and `Answer`. New format example file: [geometry ks3 maths.xlsx](https://mujahidrashid-my.sharepoint.com/:x:/g/personal/shargel_tariq_learnspot_co_uk/Efcccnyt2-pBheiLbYipflEBztpPNhgsRxnGNF24-uMfmA?e=DINWC2)
  - Update `requirements.txt`: Now contains lesser library identifiers which is easier to install library.
  - Update `.gitignore`: Excel files produced by scraper will be ignored when commit

v1.1 - 12/11/2024
- Changed:
  - Implemented changes based on the [new format proposal](https://docs.google.com/document/d/1j3eZygtLekdJfdZIvB60yKXwzuPoZw8zx4ZhbuWQYeA/edit?usp=sharing)
  - Change `Type of question` format indicators.
   - Updated format for adding multiple values in a single column are now format these values as JavaScript arrays `["item 1", "item 2", "item 3", "item 4"]`.
