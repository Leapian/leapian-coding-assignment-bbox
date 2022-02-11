# leapian-coding-assignment-bbox

## Instructions

1. Create a public repository based on this template
2. Clone your repository on your machine and implement the exercises
3. When you're done push your commits & send us a link to your repository (public)

Task instructions:

1. Read the background information
2. Implement as many of the [requirements](#detailed-requirements) as you can within a 30-40 minute time-frame
3. Make a commit for each of the [requirements](#detailed-requirements), before you move on
4. Push your work to github & contact the team for a review.

## Background

A PDF Document file contains information about the words on each PDF page, so that the PDF Renderer can draw these on the screen.

> A little known fact is that the PDF File format stores the words "in a random order", ie words that belong to the same sentence may be saved in random locations (x,y coordinates) inside the PDF.
> This makes it very difficult to parse a PDF document and obtain sentences, which would be a requirement for any NLP text analysis that requires full sentences.

We can obtain the locations of words on a PDF page from a PDF parsing library. The coordinate system of a PDF starts with (x=0, y=0) in the top-left corner of a page.
The bottom-right coordinate of the page is (x=pageWidth, y=pageHeight).
What we know about each individual word is the bounding box for that word.
The format of the words bounding box is:
top-left coordinate (wordX, wordY) relative to the **page’s top-left** coordinate and the **width** and **height** of the **rectangle**.

![Screenshot_2020-06-23_at_10 08 03](https://user-images.githubusercontent.com/402956/151150665-9595be60-cf59-42e9-af1f-02c07b370eb0.png)

## Python Dataset Input

**In this exercise you are given a data file with all bounding boxes of the words on the first PDF page.**

The dataset script contains a single variable, `data`, which is a dictionary. Each item in the dictionary was obtained by splitting each row in the CSV file by the comma, thus obtaining between 8 & 9 items in each row. There are 9 items when the wordText contains a comma: i.e the original text in the document has a comma.

### Format of the dataset.py

```
# dataset.py
data = [
    ['pageId', 'wordId', 'wordIdOnPage', 'wordX', 'wordY', 'wordWidth', 'wordHeight', 'wordText'] ,
    ['0', '0', '0', '81.0718', '78.9557', '92.3539', '16.1216', 'Trace-based'] ,
		...
]
```

> Hint: you can now import this dataset in another script with `from dataset import data`

## Data Ranges

- **pageID** from 0 to n-1 pages in the document
- **wordID** from 0 to n-1 words in the whole document
- **wordIDOnPage** from 0 to m-1 words on that same page

The bounding box is the box around a word. It consists of the top left x y coordinate of the word and the width and height.

Each page has the (0, 0) coordinate in the top left of the page. x increases along the page and y increases down the page.

## Additional Information

This is the first page of the sample document.
The words are color coded by their word height, with blue words being the most frequent words on this page.

![Screenshot_2020-06-23_at_10 14 46](https://user-images.githubusercontent.com/402956/151151477-ee567ef6-b3d5-4a52-820e-446517da0c4d.png)

### Detailed Requirements

1. Create a class to represent one `Word` from the document, including its bounding box coordinates and text
2. Represent the words in the provided dataset as a dictionary of `Word` objects. For this, you can load each CSV word from the dataset, representing each one with an object the `Word` class that you designed for step 1. When loading words from the file, use the `round(wordHeight)` as the word’s wordHeight so that the word heights come in discrete integer values. Ie. a wordHeight = 8.015 from the file should be stored as 8.0 when loaded and a word with wordHeight = 7.70 should be 8.0.
3. Create a dictionary that groups all words that have the same word-height together (this is a reflection of words that have the same font). Hint: word height is the key in the dictionary.
4. What height do the words from the main body of text have? Ie. Find out, from the dictionary in 3, what is the largest groups of words, by word height.
5. Using only the main body of text (ie, the group of words identified in step 4):
   1. Create a dictionary and group all the words by their y-coordinate (ie. place each word into a list that represents the words on the same text line)
   2. Compute: the mean, mode and median of the vertical distance between lines of words
   3. Sort the words that exist on the same text line (step 5.1) by their x-coordinate so that they are ordered by their ‘reading order’
   4. Compute: the mean, mode, median of the distance between the words on each line

> There should be 720 words on this page. There should be 553 words that have a wordHeight >7.5 and <8.5 pixels, which is the main body of text that you should identify for step 4.
