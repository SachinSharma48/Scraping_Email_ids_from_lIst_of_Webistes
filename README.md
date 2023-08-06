# Scraping_Email_ids_from_lIst_of_Webistes
Scraping email addresses from websites can be a powerful tool for various purposes, using Python to scrape email addresses from websites has emerged as a powerful technique.
Initiate the process by compiling a list of website URLs. These URLs act as gateways to the online presence of businesses, individuals, or organizations.

Utilize Beautiful Soup to navigate the intricate web structure. This navigation facilitates pinpointing email address-related elements.
Organize the collected email addresses systematically. Employ dynamic data structures for efficient storage and accessibility.
incorporate loops for iterating through the list of URLs. Automation ensures a systematic extraction process for each website.

Effortlessly extract email addresses from a list of websites' home pages.


import pandas as pd
import requests
from bs4 import BeautifulSoup
import re

def get_email_addresses(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    email_addresses = set()

    # Find email addresses using regex
    email_pattern = re.compile(r'[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+')
    for tag in soup.find_all('a'):
        if 'mailto:' in tag.get('href', ''):
            email = tag.get('href').split('mailto:')[1]
            if email_pattern.match(email):
                email_addresses.add(email)

    return list(email_addresses)

sachin_df = pd.read_csv('C:/Users/Sachin_Sharma/EMail.csv')
websites = sachin_df['Website'].tolist()

rahul_df = pd.DataFrame(columns=['Website', 'Email_ID'])

for website in websites:
    try:
        # Fetch email addresses from the front page of the website
        email_addresses = get_email_addresses(website)

        # Append data to rahul_df DataFrame
        if email_addresses:
            rahul_df = rahul_df.append({'Website': website, 'Email_ID': ', '.join(email_addresses)}, ignore_index=True)
        else:
            rahul_df = rahul_df.append({'Website': website, 'Email_ID': 'No email addresses found'}, ignore_index=True)

        # Save the data to Rahul.csv after each iteration
        rahul_df.to_csv('C:/Users/Desktop/sachin.csv', index=False)

        print(f"Fetched email addresses from {website}")
    except Exception as e:
        print(f"Error fetching email addresses from {website}: {e}")

print("Done)
