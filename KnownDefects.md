Known Defects:

1. ocr.py file
    - Some methods in ocr.py return a list and they don’t need to be a list, requires
      unnecessary step of using .join()
    - get_city() function in ocr.py is broken and needs to be fixed
    - We are no longer using the precendents nor statutes field, this has been commented out in the make_fields() so the
      get_precedents function is no longer being used
    - the current version of the DS API is deployed from the db_refactor branch, this needs to be merged with main
    - the backend team needs to make sure to change the table they are using to db_cases for uploading case files

1. Fields
    - All fields should be validated and fine-tuned for accuracy
    - Field `get_credibility` could be improved, because the field's output is dependent on occurrence of specific tokens in the document
    - Field `get_applicant_language` needs to be further refined to capture language names that are both one- and two-words long
    - Field `get_indigenous_status` needs to be further refined to properly capture names of indigneous groups/nations that are longer and shorter than two words.
    - Field `get_credibility` needs to have a wider search breadth, as to include the following search terms:
        - Consistent
        - Inconsistent/inconsistencies
        - Totality of the circumstances
        - Corroborating evidence
        - Documentary evidence
        - Burden
        - Testimony
        - Oral testimony
        - Explanation

2. API
    - <strike>Locally the API works, but it takes about 20-45 seconds to run. This is a problem for Heroku. Heroku has a set max 30-second timeout limit and any request must be below this threshold. Currently, our TL, Robert Sharp, has been working on speeding it up. He may solve this problem before students run into it; however, future labs students may want to explore deploying to AWS. However, this is no easy task. A dependency named Poppler is required for the OCR and must be installed through Docker. Docker is hard to get deployed on AWS so finding a workaround with Robert may be a better approach. One way to get around the 30-second timeout is through background processes, but this will require more resources on Heroku.</strike>
    - The API has been fixed to work on AWS with Docker.
3. Scraper Class
    - <scrape>The scraper class works however is not completely optimized for efficiency and not always accurate. I would encourage future labs student to throw as many pdfs of case files through the scraper API locally and see if they can find and fix problems within the scraper class. As of right now, we have the scraper working as a proof of concept, but not as a viable and completely accurate product.
    - Most functions in the Scraper have been updated and almost every field is hitting at high accuracy on the training data, https://drive.google.com/drive/folders/1won2SiIu90WyeCG5WZK07fd7LIWGsG7N?usp=sharing 
    
    - Accuracy of each field:
         95% = get_panel(),
         100% = is_appellate(),
         = get_application(),
         = get_date(),
         95% = get_country_of_origin(),
         100% = get_outcome(),
         94% = get_state(),
         80% = get_city()
         94% = get_circuit()
         = get_protected_grounds(),
         = get_based_violence(),
         95% = get_gender(),
         = get_indigenous_status(),
         = get_applicant_language(),
         = get_credibility(),
         100% = check_for_one_year(),
         = get_precedent_cases(),
         = get_statutes(),
    - <strike>If the OCR suffers from noticeable time issues then it may be worth looking into using MuPDF and PyMuPDF as a replacement for Poppler, PyTesseract and PDF2Image. PyMuPDF is capable of reading various text and image formats without the need of extra libraries.  In addition it has faster read times when compared against Poppler based libaries.  LABS32 opted to hold off on PyMuPDF as we were faced with connecting codebases and deploying to AWS.  These issues have been solved so if the OCR sees slow scraping times then it may be beneficial to further investigate this option.  It will also be prudent to check whether the project is covered under the AGPL licensing linked.  Below are links to documentation and speed comparisons. 
        - https://hzqtc.github.io/2012/04/poppler-vs-mupdf.html Speed comparison of Poppler vs MuPDF
        - https://pymupdf.readthedocs.io/en/latest/app1.html PyMuPDF compared to other PDF readers
        - https://mupdf.com/docs/ MuPDF Documentation
        - https://pymupdf.readthedocs.io/en/latest/intro.html# PyMuPDF Documentation *IMPORTANT* Check whether use case is covered under AGPL Open Source Licensing.</strike>
    - Since this project may fall out of compliance with an AGPL license in the future, it has been determined that PyMuPDF is not a suitable package.
