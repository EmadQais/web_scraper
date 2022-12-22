# web_scraper
Scrape data from websites quickly &amp; automatically

	<div class="Explenation">
		<h1>How to TextPrePro works</h1><br>
		<p>With the rapid growth of text data in recent years, there has been considerable effort toward understanding the topics of online discussions. Unfortunately, state of the art text analysis tend to perform poorly on this new form of data, due to their noisy and unstructured nature.</p><br>
		<p>In this case, We perform this tool on the many of website data and in the process show the importance of preprocessing and the usefulness of our preprocessing pipeline when dealing with different kind of text data.</p><br>
		<p>In this explination we explore in a comprehensive way how to make scraping and text preprocessing within visulization the result like shown below</p><br>
		<p><strong>Step 1:</strong> You have to collect all websites that wanted to make scraping for it by tool above as the explination below:</p>
		<img src="images/TextPrePro.gif" alt="" class="alignleft border" class="language-python" style="margin-bottom:20px; width: 100%; height: 70%" />
		
		 
		 <p><strong>Step 2:</strong> You have to download all Python libraries that required in this tool</p>
		
		  <pre>
				<code class="language-python">
					from bs4 import BeautifulSoup
					import requests
					import pandas as pd
					import matplotlib.pyplot as plt
					import random
					from nltk.tokenize import sent_tokenize, word_tokenize
					from nltk.corpus import stopwords
					from nltk.stem import WordNetLemmatizer
					import re
			 </code>
		 </pre>
		<br>
		<p><strong>Step 3:</strong> Scraping the text from websites and save the text to the local text</p>
		
		  <pre>
				<code class="language-python">
					lineCount = 0
					paragraphsCount = []
					paragraphs = []
					listCount = []
					with open(r'C:\Users\Emad-Laptop\Downloads\Links.txt','r') as txt:
						for line in txt:
							page = requests.get(line.strip())
							soup = BeautifulSoup(page.content, 'html.parser') # Extract contect
							paragraphsCount.append(len(soup.find_all("p")))
							lineCount += 1
							listCount.append(lineCount)
							FileName = str(lineCount)
							FileName = 'TextPreprocessing\\Text\\'+FileName+'.txt'
							f = open(FileName,'w',encoding = 'utf-8')
							for i in range(len(soup.find_all('p'))) : 
								text = soup.find_all('p')[i].get_text() 
								paragraphs.append(text)
								f.write(soup.find_all('p')[i].get_text())
								f.write('\n')
								f.close()
			 </code>
		 </pre>
		 <br>
		 <p><strong>Step 3:</strong> Scraping the text from websites and save the text to the local text</p>
		
		  <pre>
				<code class="language-python">
					lineCount = 0
					paragraphsCount = []
					paragraphs = []
					listCount = []
					with open(r'C:\Users\Emad-Laptop\Downloads\Links.txt','r') as txt:
						for line in txt:
							page = requests.get(line.strip())
							soup = BeautifulSoup(page.content, 'html.parser') # Extract contect
							paragraphsCount.append(len(soup.find_all("p")))
							lineCount += 1
							listCount.append(lineCount)
							FileName = str(lineCount)
							FileName = 'TextPreprocessing\\Text\\'+FileName+'.txt'
							f = open(FileName,'w',encoding = 'utf-8')
							for i in range(len(soup.find_all('p'))) : 
								text = soup.find_all('p')[i].get_text() 
								paragraphs.append(text)
								f.write(soup.find_all('p')[i].get_text())
								f.write('\n')
								f.close()
			 </code>
		 </pre>
		 <br>
		 <p><strong>Step 4:</strong> Display the links those scraped data of them and how many paragraphs they have</p>
		
		  <pre>
				<code class="language-python">
					with open(r'C:\Users\Emad-Laptop\Downloads\Links.txt','r') as txt:
						for line in txt:
							print(line)
					fig, ax = plt.subplots(figsize=(12, 7))
					plt.bar(listCount,paragraphsCount)
					ax.set_title("Number of Paragraphs in each file")
					plt.show()
			 </code>
		 </pre>
		 <br>
		 <p><strong>Example Result</strong></p>
		 <img src="C:\Users\Emad-Laptop\Downloads\TextPrePro Graph1.png" alt="" class="alignleft border" class="language-python" style="margin-bottom:20px; width: 100%; height: 70%" />
		 
		 <br>
		 <p><strong>Step 5:</strong> Add all text files to one corpus</p>
		
		  <pre>
				<code class="language-python">
					import glob
					import os

					file_list = glob.glob(os.path.join(os.getcwd(), "TextPreprocessing\\Text\\", "*.txt"))
					corpus = []
					for file_path in file_list:
						with open(file_path, mode="r", encoding="utf-8") as f_input:
							corpus.append(f_input.read())
							f_input.close()
					len(corpus)
					print(corpus[0])
			 </code>
		 </pre>
		 
		 <br>
		 <p><strong>Step 6:</strong> Counting the stop words included in the corpus</p>
		
		  <pre>
				<code class="language-python">
					stopWords = set(stopwords.words('english'))
					document = ''.join(paragraphs)
					Removed_SW = ' '.join([SW for SW in document.split() if SW in stopWords])
					# print(Removed_SW)

					from collections import Counter
					cnt = Counter()
					for text in Removed_SW.split():
						cnt[text] += 1
					# See most common ten words
					cnt.most_common(10)
					word_freq = pd.DataFrame(cnt.most_common(20), columns=['words', 'count'])
					word_freq.head(30)
			 </code>
		 </pre>
		 
		 <br>
		 <p><strong>Step 7:</strong> Plot for most frequency stop word</p>
		
		  <pre>
				<code class="language-python">
					word_freq.sort_values(by='words').plot.barh(x='words', y='count')
					ax.set_title("Common Words Found")
					plt.show()
			 </code>
		 </pre>
		 
		 <br>
		 <p><strong>Example Result</strong></p>
		 <img src="C:\Users\Emad-Laptop\Downloads\TextPrePro Graph2.png" alt="" class="alignleft border" class="language-python" style="margin-bottom:20px; width: 100%; height: 70%" />
		 
		 <br>
		 <p><strong>Step 8:</strong> Remove the stop words, unwanted charaters and make lemmatization of words</p>
		
		  <pre>
				<code class="language-python">
					stemmer = WordNetLemmatizer()
					documents = []
					for sen in range(0, len(paragraphs)):    
						document = re.sub(r'\W', ' ', str(paragraphs[sen])) # Remove all the special characters    
						document = re.sub(r'\s+[a-zA-Z]\s+', ' ', document) # remove all single characters    
						document = re.sub(r'\^[a-zA-Z]\s+', ' ', document) # Remove single characters from the start    
						document = re.sub(r'\s+', ' ', document, flags=re.I) # Substituting multiple spaces with single space    
						#document = re.sub(r'^b\s+', '', document) # Removing prefixed 'b'    
						document = document.lower() # Converting to Lowercase
						
						stopWords = set(stopwords.words('english'))
						words = word_tokenize(document)
						sent = []
						for w in words:
							if w not in stopWords:
								sent.append(w)

						# Lemmatization
						# document = document.split() 
						document = [stemmer.lemmatize(word) for word in sent]
						document = ' '.join(document)
						
						documents.append(document)
			 </code>
		 </pre>
		 
		 <br>
		 <p><strong>Example Result</strong></p>
		
		  <pre>
				<code class="language-html">
get 5000 welcome voucher login download appsamsung cookysite us cooky clicking accept continuing browse site agreeing use cooky findchoose location languagegalaxy fold4 flip4let neo qled 8kbespoke refrigeratorsmartthingsfreestyle sound towercart emptysorry insufficient stock cartremove productwithout product applied coupon promotion code redeemed sure remove productprivacy policytick box proceed samsung comsamsung com service marketing information new product service announcement well special offer event newslettercheck preferencehelp u make recommendation updating product preferencelookingsuggested searchsuggestionsearch historyrelated searchmatched contentexperience live demomkbhd mkbhd studio refinement disclaimer tech reviewer share thought galaxy flip4 payment made create review license obtained fee content posted youtube availability model color may vary country carrier hear expert say galaxy flip4 unboxtherapy unbox therapy hold unfolded galaxy flip4 bora purple hand speaks quote unbox therapy ultimate kind activity phone ultimate wearable phone design gadgetmatch black box flip written cover placed wooden picnic table gadget match michael josh slide finger word galaxy flip4 written box next michael josh hold phone hand rotates show various angle durability extreme close hinge unfolded bora purple device quote mkbhd bit stronger durable next shot mkbhd unfolding device one hand shown side show edge profile camera shot cover screen device partially folded position stand upright slightly le 90 degree quote gadget match expect better looking photo next side side comparison photo taken ultra wide mode galaxy flip4 versus galaxy flip3 galaxy flip4 took color rich photo predecessor cover screen live preview unbox therapy appears cover screen galaxy flip4 flexcam mode recording video rear camera next main screen display photo gallery app quote gadget match probably best selfie camera market photo shown portrait mode subject clear background blurred battery close shot rear galaxy flip4 hand mkbhd us device snapdragon 8 plus gen 1 3 700 milliamp hour battery phone held shown unfolded rear next phone partially folded acute angle stand top bottom edge rear cover facing upward samsung dot com samsung logooverhead shot folded galaxy flip4 bora purple seen hinge pulled purse next device unfolded flex mode camera app running preview screen group people getting ready take picture together new galaxy flip4 disclaimer image simulated ux ui subject change flex mode supported angle 75 degree 115 degree next group friend smile shot device sitting coffee table front friend sitting couch main screen facing next device seen main screen
			 </code>
		 </pre>
	</div>
	
	
## 🔗 Links
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/emad-qais-28017561/)
[![twitter](https://img.shields.io/badge/twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/EmadQais)
