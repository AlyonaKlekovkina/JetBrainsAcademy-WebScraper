import requests
from bs4 import BeautifulSoup
import os


def create_body_content(list_of_links, list_of_titles, list_of_pages):
    for i in range(len(list_of_links)):
        article_title = list_of_titles[i]
        folder_name = list_of_pages[i]
        r = requests.get(list_of_links[i], headers={'Accept-Language': 'en-US,en;q=0.5'})
        if r.status_code == 200:
            path = folder_name
            os.makedirs(path, exist_ok=True)
            file = open(path + '/' + article_title, 'w', encoding='utf-8')
            soup = BeautifulSoup(r.content, 'html.parser')
            article_body = soup.find_all('p', {'class': 'article__teaser'})
            for i in article_body:
                file.write(i.text)
            file.close()
        else:
            return 'The URL returned {}!'.format(r.status_code)
    return list_of_titles


def filter_type_news(article_type, number_of_pages):
    link_list = []
    title_list = []
    pages_list = []
    for i in range(number_of_pages):
        url = 'https://www.nature.com/nature/articles?sort=PubDate&year=2020&page={}'.format(i+1)
        page = 'Page_{}'.format(i+1)
        r = requests.get(url, headers={'Accept-Language': 'en-US,en;q=0.5'})
        if r.status_code == 200:
            soup = BeautifulSoup(r.content, 'html.parser')
            filtered_news = soup.find_all('span', {'data-test': 'article.type'})
            for j in filtered_news:
                if j.text == article_type:
                    create_link = j.parent.parent.parent.findChildren('a', {'data-track-action': "view article"})
                    for k in create_link:
                        href = k.get('href')
                        title = k.text
                        name = title.replace('.', '').replace(',', '').replace('?', '').replace("'", "").replace(':', '').replace('-', '').replace(' ', '_')
                        file_name = '{}.txt'.format(name)
                        pages_list.append(page)
                        title_list.append(file_name)
                        article_link = 'https://www.nature.com/nature{}'.format(href)
                        link_list.append(article_link)
    return link_list, title_list, pages_list


page_n = int(input())
type_of_article = input()
all_links = filter_type_news(type_of_article, page_n)
the_body = create_body_content(all_links[0], all_links[1], all_links[2])
print("Saved articles:", the_body)
