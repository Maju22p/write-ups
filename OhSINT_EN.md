# OhSINT (English)

Made by: Maria Julia Souza
Category: OSINT Site: tryhackme.com
Link: <https://tryhackme.com/room/ohsint>

## Introduction

The challenge is to find as much information as possible using only a single image of Windows XP.

[![GoogleXp](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/googlexp-OhSINT1.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/googlexp-OhSINT1.png)

To make the process easier, I used the virtual machine provided by the platform itself; however, within the room it is also possible to download the image to investigate it locally. The challenge image on the TryHackMe machine can be found at `/Rooms/OhSint`.

[![Diretorio](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT2.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT2.png)

### Beginning of the investigation

Since the challenge mentions an image, the natural first step is to check whether it contains EXIF metadata (information such as date, time, author, GPS, etc.). I opened the directory in the terminal using the command `cd Rooms/OhSINT` to locate the image. After that, I used the exiftool (a tool specifically designed for reading, writing, and editing metadata), which allowed me to read the image's EXIF information.

[![exiftool](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT3.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT3.png)

```
Copyright                     : OWoodflint  

GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W  
```

Among the returned lines, I found some very useful information, such as the image's author and a possible location of where the image was taken. For better clarity, I trimmed the output to show only the relevant lines; the rest are standard technical metadata that would not be useful in this context.

After finding the author's name (OWoodflint), I did a simple Google search and found profiles under the same username on two sites: X.com and github.com (which we will dig into further below).

[![busca](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT4.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT4.png)

Once I found the user's profile on X, I was able to answer the questions requested by the room:

[![perfil x](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT5.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT5.png)

### What is this user's avatar of?

By investigating the user's profile on X, I found that their profile picture is a cat.

**Answer:** `cat`

### What is the SSID of the WAP he connected to?

In one of his posts on X, we can find the BSSID of his network `(B4:5D:50:AA:86:41)`.

[![gato do perfil](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT6.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT6.png)

I initially tried Wigle's basic search using only the BSSID, but got no results from the site. After several attempts, I realized the Advanced Search was needed instead. Using Wigle's advanced search (a platform that maps and indexes Wi-Fi networks, Bluetooth, and base radio stations) and entering the BSSID, I found the following information:

[![BSSID](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT7.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT7.png)

Where we can check the SSID name: `UnileverWiFi`.

**Answer:** `UnileverWiFi`

### What is his personal email address?

To answer this question and the following ones, I investigated the GitHub profile found in the search: <https://github.com/OWoodfl1nt/people_finder>, where I found a repository called `people_finder`. In this public repository, I found a README file that provided more information about the user, such as their email, personal blog (which we will explore further below), and where they are from.

[![GitHub](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT8.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT8.png)

**Answer:** `OWoodflint@gmail.com`

### What site did you find his email address on?

**Answer:** `Github`

### What city is this person in?

**Answer:** `London`

### Where has he gone on holiday?

By accessing the site previously found in the GitHub repository, I was able to access the user's personal blog. It's a very simple blog, and the only piece of information we can obtain is a text in which the user mentions his trip to New York. We can therefore assume his holiday was in New York.

[![Site do usuário](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT9.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT9.png)

**Answer:** `New York`

### What is the person's password?

After checking the blog's features, I decided to inspect the page's source code. There, I was able to find a paragraph written in white text: "pennYDr0pper.!" in the HTML code (likely an attempt to hide the password by camouflaging it against the background). When I tested this as the answer, it was confirmed to be the user's password.

[![Codigo-fonte](https://github.com/Maju22p/write-ups/raw/main/Write%20up%20(POR)/imagens/OhSINT10.png)](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT10.png)

**Answer:** `pennYDr0pper.!`

## Conclusion

This challenge showed how it is possible to build a nearly complete profile of a person from a single image, without using any hacking technique — just publicly available information and metadata that most people don't even know exists. Starting from a simple "Copyright" field in a photo, it was possible to trace social media accounts, an email address, a city, a Wi-Fi network, a password, and even a travel destination.

This highlights an important point about digital security: image metadata (such as GPS and author) and information shared on social media may seem harmless on their own, but together they form a trail capable of compromising someone's privacy. As a good practice, it is recommended to strip metadata from photos before publishing them and to avoid sharing sensitive details (such as a network's BSSID or passwords, even if "hidden" in a website's source code) publicly.
