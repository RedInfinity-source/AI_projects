# reads an article and answers questions using what it learned
# import
from newspaper import Article
import random
import string
import nltk
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np
import warnings
warnings.filterwarnings('ignore')

# download the punk package
nltk.download('punkt',quiet = True)

# get the article
A1 = "https://docs.python.org/3/tutorial/index.html"
A2 = "https://www.nfl.com/news/nfl-roundup-latest-league-news-from-thursday-april-14"
A3 = "https://www.vanityfair.com/news/2022/04/fox-corp-staffers-are-fed-up-with-fox-news-hateful-lgbtq-coverage"
article = Article(A1)
article.download()
article.parse()
article.nlp()
corpus = article.text

# print the article text
#print(corpus)

# tokenization
text = corpus
sentence_list = nltk.sent_tokenize(text)
# print
#print(sentence_list)

# index_sort
def index_sort(list_var):
    length = len(list_var)
    list_index = list(range(0, length))
    x = list_var

    # sort list
    for i in range(length):
        for j in range(length):
            if x[list_index[i]] > x[list_index[j]]:
                #swap
                temp = list_index[j]
                list_index[i] = list_index[j]
                list_index[j] = temp

    return list_index

# a function that makes a random greeting
def greeting_response(text):
    text.lower()

    # bot gets the response
    bot_greeting = ['hello','welcome','how can i help you?']
    # user greeting
    user_greeting = ['hi','hey','hello','i need help']

    # bot response
    for word in text.split():
        if word in user_greeting:
            return random.choice(bot_greeting)

# create bots response
def bot_response(user_input):
    user_input = user_input.lower()
    sentence_list.append(user_input)
    bot_response = ''
    cm = CountVectorizer().fit_transform(sentence_list)
    similarity_score = cosine_similarity(cm[-1],cm)
    similarity_score_list = similarity_score.flatten()
    index = index_sort(similarity_score_list)
    index = index[1:]
    responce_flag = 0

    # if found 2 or less similar sentences
    j = 0
    for i in range(len(index)):
        if similarity_score_list[index[i]] > 0.0:
            bot_response = bot_response + ' ' + sentence_list[index[i]]
            responce_flag = 1
            j = j + 1
        if j > 5:
            break

        if responce_flag == 0:
            bot_response = bot_response + ' ' + "sorry i don't understand."

        sentence_list.remove(user_input)
        return  bot_response

# start the chat
exit_list = ['exit','quit','thats all','break']
while True:
    user_input = input('> ')
    if user_input.lower() in exit_list:
        print('thank you, have a good day')
        break
    if user_input.lower() == 'next':
        number = int(input('what number article would you like to look at? > '))
        if number == 1:
            article = Article(A1)
        elif number == 2:
            article = Article(A2)
        elif number == 3:
            article = Article(A3)
        else:
            print("sorry that's not an option")
    else:
        if greeting_response(user_input) != None:
            print('bot:',greeting_response(user_input))
        else:
            print('bot:',bot_response(user_input))
