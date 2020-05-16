import csv
import requests
from bs4 import BeautifulSoup
from unidecode import unidecode

surl = "https://www.youtube.com/results?search_query="

topics = [ "Restaurant", "food", "Restaurant Style Biryani","Restaurant Style Dragon Chicken",
           "kitchen","foodie", "eating", "street food", "restraunts"]

page = "&page="
out = open("output.csv", "a+")

for topic in topics:
    page_count = 1
    total_pages = 50
    count = 1

    while page_count < total_pages:
        url = surl + topic + page + str(page_count)
        print(url)
        headers = {'Accept-Language': 'en-US,en;q=0.8'}
        r = requests.get(url,headers=headers)
        soup = BeautifulSoup(r.content, "html.parser")
        videos = soup.find_all("div", class_="yt-lockup-video")
        if videos is not None:
            for vid in videos:
                title = vid.find("a", "yt-uix-tile-link").get('title')
                link = vid.find("a", "yt-uix-tile-link").get('href')
                title = unidecode(title)
                div = vid.find("div", "yt-lockup-byline")
                channel = div.find("a", "yt-uix-sessionlink spf-link").text
                channel = unidecode(channel)
                channel_link = div.find("a", "yt-uix-sessionlink spf-link").get('href')
                info = ["", ""]
                ul = vid.find("ul", "yt-lockup-meta-info")
                tm = ul.find_all("li")
                i = 0
                for item in tm:
                    if i < 2:
                        info[i] = item.text
                        info[i] = unidecode(info[i])
                        i += 1

                # if vid.find("div", "yt-lockup-description yt-ui-ellipsis yt-ui-ellipsis-2") is not None:
                #   des = vid.find("div", "yt-lockup-description yt-ui-ellipsis yt-ui-ellipsis-2").text
                # else:
                #   des = ""

                url_vid = "https://www.youtube.com/" + link
                req_vid = requests.get(url_vid,headers=headers)
                soup_vid = BeautifulSoup(req_vid.content, "html.parser")

                if soup_vid.find("button",
                                 "yt-uix-button yt-uix-button-size-default yt-uix-button-opacity yt-uix-button-has-icon no-icon-markup like-button-renderer-like-button like-button-renderer-like-button-unclicked yt-uix-clickcard-target yt-uix-tooltip") is not None:
                    likes = soup_vid.find("button",
                                          "yt-uix-button yt-uix-button-size-default yt-uix-button-opacity yt-uix-button-has-icon no-icon-markup like-button-renderer-like-button like-button-renderer-like-button-unclicked yt-uix-clickcard-target yt-uix-tooltip")
                    like = likes.find("span").text
                    like = unidecode(like)
                else:
                    like = ""

                disd = soup_vid.find("button",{"title":"I dislike this"})
                if soup_vid.find("button",{"title":"I dislike this"}) is not None:
                    if disd.find("span") is not None:
                        dis = disd.find("span").text
                else:
                    dis = ""

                des=""
                if soup_vid.find("div",{"id":"watch-description-text"}) is not None:
                    div = soup_vid.find_all("div",{"id":"watch-description-text"})
                    for desc in div:
                        des = des + desc.text
                        des = unidecode(des)

                if soup_vid.find("meta", property="og:keywords") is not None:
                    keywords = soup_vid.find("meta", property="og:keywords", content=True).get('content')
                    keywords = unidecode(keywords)
                else:
                    keywords = ""

                if soup_vid.find("span",
                                 "yt-subscription-button-subscriber-count-branded-horizontal yt-subscriber-count") is not None:
                    subs = soup_vid.find("span",
                                         "yt-subscription-button-subscriber-count-branded-horizontal yt-subscriber-count").text
                    subs = unidecode(subs)
                else:
                    subs = ""
                title = title.replace(',', '')
                des = des.replace(',', '')
                subs = subs.replace(',', '')
                info[0] = info[0].replace(',', '')
                info[1] = info[1].replace(',', '')
                like = like.replace(',', '')
                dis = dis.replace(',',"")
                channel = channel.replace(',', '')
                print(str(count) + ' ' + title + ',' + link + ',' + keywords + ',' + channel + ',' + channel_link + ',' + subs + ',' +
                      info[0] + ',' + info[1] + ',' + like + ',' + dis + ',' + des)
                out.write(title + ',' + channel + ',' + subs + ',' + info[0] + ',' + info[1] + ',' + like + ',' + dis + ',' + des+'\n')
                count += 1
                print('\n')
            page_count += 1
        else:
            page_count = total_pages
out.close()
