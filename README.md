# Web Scraping and NLP Analysis: Moby Dick Text Analysis 
You'll scrape a novel from an `html` file taken from the website Project Gutenberg (which contains a large corpus of books) using the Python requests package. Then you'll extract text from this web data using the BeautifulSoup library. Finally, you'll analyze the distribution of words using the Natural Language ToolKit (nltk), a very common NLP task used to gain insights on textual data and lay the groundwork for deeper analysis.

The natural language processing tools used here are required for many important data science tasks as a vast proportion of the world's data is unstructured and includes a great deal of text. To complete this project, you need to know how to import web data into Python and how to work with natural language text.

## Project Overview
Web scraping and NLP analysis of Herman Melville's "Moby Dick" from Project Gutenberg to identify and visualize the most frequent words in the text.

### Project Instructions

**What are the most frequent words in Herman Melville's novel *Moby Dick*, and how often do they occur?**
Your project will follow these steps:

1. **Request the HTML file:**
   - Use the `requests` library and encode it to utf-8.
   - Here is the URL to scrape from: [Project Gutenberg's Moby Dick](https://www.gutenberg.org/files/2701/2701-h/2701-h.htm)

2. **Extract the HTML and create a BeautifulSoup object:**
   - Use an HTML parser to get the text.

3. **Initialize a regex tokenizer object:**
   - Use `nltk.tokenize.RegexpTokenizer` to keep only alphanumeric text, assigning the results to `tokens`.

4. **Transform the tokens:**
   - Convert the tokens to lowercase, remove English stop words, and save the results to `words_no_stop`.

5. **Find the ten most common words:**
   - Initialize a `Counter` object and find the ten most common words, saving the result to `top_ten` and printing to see what they are.

## Implementation

### Import required tools
- Python two essential modules for web scraping: Requests and Beautiful Soup
```
import requests
from bs4 import BeautifulSoup
```
### 1. Fetching HTML
To start the web scraping, fetch the HTML content of a webpage via request
```
# Send an HTTP GET request to the webpage and set encoding
r = requests.get('https://s3.amazonaws.com/assets.datacamp.com/production/project_147/datasets/2701-h.htm')
r.encoding = 'utf-8'

# Store the HTML content in a variable
html = r.text

print(html[0:2000])
```
### 2. Parsing HTML via BeatifulSoup
Use built-in parser (reads HTML code and turns it into a structured format -a parse tree) provided by BeautifulSoup
```                                               
html_soup = BeautifulSoup(html, "html.parser") # Create a BeautifulSoup object from the HTML
moby_text = html_soup.get_text() # Get the text out of the soup
```
### 3. Regex Tokenizer via nltk (Natural Language Toolkit) library
Use RegexpTokenizer, a tokenizer (splitting a string of text into individual pieces) from the nltk library
\w+ - extract only the words (alphanumeric characters) from the text, excluding any punctuation or special characters
```                                               
# Create a tokenizer
tokenizer = nltk.tokenize.RegexpTokenizer('\w+')

# Tokenize the text
tokens = tokenizer.tokenize(moby_text)

# Inspect
tokens[:8]
```
### 4. Transforming Tokens to Lowercase
```                                               
# Create a list called words containing all tokens transformed to lowercase
words = [token.lower() for token in tokens]

# Print out the first eight words to inspect changes
words[:8]
```
### 5. Removing Stop Words (to focus on the more meaningful words in the text) Part 1
Use the nltk.corpus.stopwords module to get a list of English stop words.
```                                               
from nltk.corpus import stopwords

# Download the set of stop words the first time you run this
nltk.download('stopwords')
# Get the English stop words from nltk
stop_words = nltk.corpus.stopwords.words('english')

# Print out the first eight stop words
stop_words[:8]
```
### 6. Initialize a Counter Object and Find the Ten Most Common Words
Use a Counter object (a class from the collections module in Python) to count the occurrences of each word in the list words_no_stop
Find the ten most common words and their frequencies
Save the result to top_ten and print it to see the most frequent words
```                                               
from collections import Counter
# Step 1: Initialize a Counter object with words_no_stop
word_freq = Counter(words_no_stop)
# Step 2: Find the ten most common words
top_ten = word_freq.most_common(10)
print(top_ten)
```
### 7. Visualize
Use a Counter object (a class from the collections module in Python) to count the occurrences of each word in the list words_no_stop
Find the ten most common words and their frequencies
Save the result to top_ten and print it to see the most frequent words
```                                               
import matplotlib.pyplot as plt

# Extract words and their frequencies
words, frequencies = zip(*top_ten)

plt.figure(figsize=(10, 5))
plt.bar(words, frequencies)
plt.xlabel('Words')
plt.ylabel('Frequency')
plt.title('Most Common Words in Moby Dick')
plt.show()

```
![](https://github.com/snoowbirvd/Web-Scraping-and-NLP-Analysis-Text-Analysis/blob/f190aae6f10c156f60d2cffac645395aad3c66e1/Most%20Common%20Words.png)
