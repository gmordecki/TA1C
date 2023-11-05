# TA1C: A Dataset for Clickbait Detection in Spanish News

TA1C (**Te Ahorré Un Click**, Spanish for Saved You A Click) is the first open source dataset for clickbait detection in Spanish.

It consists of 3,500 manually annotated tweets, coming from 18 well known media sources. Tweets were downloaded throughout a year between October 2020 and October 2021, most of them are from that date range, but there are some from even as early as 2015.

There are two versions of the dataset, containing the same tweets and labels, but different amounts of information.

In the **minimal version** each row contains
- Tweet ID
- Tweet date
- Media name
- Media origin country
- Teaser text
- Tag value

The **complete version** adds:
- Tweet account
- Tweet URL
- Tweet media URL (if it exists)
- Tweet text
- Linked article's URL
- Article's scraped headline
- Article's scraped subheadline
- Article's scraped clean text (only the news body)
- Article's scraped images with their captions (in a JSON list)
- Article's scraped embedded URLs (usually social media links)
- Secondary annotator tag

We are going to release a third version including the article's complete HTML.

The second annotation reaches reaching a Fleiss' κ inter annotator agreement of 0.78, demonstrating that the annotation criteria are precise and reduce subjectivity. We are currently working to achieve a third independent annotation and take the majority vote as ground truth.

## Teaser text

Teaser text is a preprocessed mix of headline and tweet text that we suggest using for the classification.

For both headline and tweet text it has the following **preprocessing**:
- Substitution of emojis for the word EMOJI
- Substitution of hashtags *#Columna*, *#Editorial* and *#Opinión* for words *columna*, *editorial* and *opinión* respectively.
- Substitution of remaining hashtags for the word *HASHTAG*
- Removal of regex *RT|por|vía @cuenta*
- Substitution of accounts (*@account*) for the word *NombrePropio*
- Removal of strange characters and multiple spaces
- Removal of URLs

Subsequently, the text used emerges as a combination of both headline and tweet text. Twitter provides the option to display a link preview or not, as well as to accompany the post with an image. Often, newspapers utilize these tools to avoid revealing key information, for instance, by not showing a preview if the title spoils the information gap created by the post. Therefore, the selection of which texts to use is not trivial, and a single criterion can lead to inconsistencies with what the user reads. However, due to the technical impossibility of knowing how users see it (among other things because it depends on the device they have) and for the sake of consistency, the Teaser Text combination is always made according to the following criteria:
- If the title is included in the tweet text or the tweet text is longer than the title, and the editing distance (a.k.a. [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)) is less than 5, or the normalized Levenshtein similarity (computed with [textdistance](https://pypi.org/project/textdistance/)) is greater than 0.9, the tweet text is used.
- If the tweet text is included in the title or the title is longer than the tweet text, and the editing distance is less than 5, or the normalized Levenshtein similarity is greater than 0.9, the title is used.
- Otherwise, the title followed by the tweet text is used.
