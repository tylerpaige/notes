# Competition Be Damned - Features - Source: An OpenNews project

![rw-book-cover](https://media.opennews.org/cache/c2/78/c278bc6c586a9bb80d69ef872f341a68.jpg)

## Metadata
- Author: [[opennews.org]]
- Full Title: Competition Be Damned - Features - Source: An OpenNews project
- Category: #articles
- Publication date: 2017-04-27
- URL: https://source.opennews.org/articles/news-nerds-against-pdfs/
- Summary: How reporters at the Washington Post, New York Times, ProPublica, and more self-organized to free trapped FEC data

# Notes

[[opennews.org - Competition Be Damned - Features - Source: An OpenNews project]]

***

Features

#### How reporters at the Washington Post, New York Times, ProPublica, and more self-organized to free trapped FEC data

![](https://media.opennews.org/cache/c2/78/c278bc6c586a9bb80d69ef872f341a68.jpg)
Last Wednesday, the Trump Inaugural Committee’s FEC filing appeared on the FEC site in its image-based glory. ProPublica’s Derek Willis noted its arrival on Twitter.

![](https://pbs.twimg.com/profile_images/1061425559945822214/ns-bDp0l.jpg)

[Derek Willis](https://twitter.com/derekwillis)
[@derekwillis](https://twitter.com/derekwillis)

Here's the 510-page filing from Trump inaugural committee. It could have been filed electronically but wasn't: [docquery.fec.gov/pdf/286/201704…](http://docquery.fec.gov/pdf/286/201704180300150286/201704180300150286.pdf)

![](https://pbs.twimg.com/media/C9xotSdWsAAJikx.jpg)

[Posted Apr 19, 2017 at 12:28PM](https://twitter.com/derekwillis/status/854673161975193600)

![](https://pbs.twimg.com/profile_images/1061425559945822214/ns-bDp0l.jpg)

[Derek Willis](https://twitter.com/derekwillis)
[@derekwillis](https://twitter.com/derekwillis)

Imagine 25 news orgs each taking 20 pages, then they get access to full dataset and public access soon after.

[Posted Apr 19, 2017 at 2:22PM](https://twitter.com/derekwillis/status/854701724724015104)

Later that day, the Washington Post’s Steven Rich made the first commit to a [public dataset on GitHub](https://github.com/washingtonpost/data-inaugural-committee/) extracted from the filing. Since then, several more news nerds, including Willis, have jumped in to contribute to and correct the data. Huffington Post White House reporter [Christina Wilkie](https://twitter.com/christinawilkie) used the resulting dataset as the jumping-off point for a crowdsourced investigation of donors that became the basis for [her own](http://www.huffingtonpost.com/entry/trump-inauguration-fec_us_58fec92ae4b00fa7de170e73) and [other outlets’ investigations](https://theintercept.com/2017/04/20/donald-trumps-inauguration/).

![](https://pbs.twimg.com/profile_images/1553168502030303232/qMwu9jtL.jpg)

[Steven Rich](https://twitter.com/dataeditor)
[@dataeditor](https://twitter.com/dataeditor)

Hi, here's my first pass at converting the Inaugural committee donations PDF into data. Will add more fields soon. [github.com/washingtonpost…](https://github.com/washingtonpost/data-inaugural-committee)

[Posted Apr 19, 2017 at 6:34PM](https://twitter.com/dataeditor/status/854765104792993792)

We spoke with four of the project’s contributors—Rich, Willis, the New York Times’ Rachel Shorey, and the Connecticut Mirror’s Andrew Tran (soon to join Rich at the Post), about how the data got freed.

**Source: How did this collaboration happen? I assume [this filing](http://docquery.fec.gov/pdf/286/201704180300150286/201704180300150286.pdf) was what you started with—who decided to turn it into useable data, and how did other people help out?**

**Steven Rich:** So on Wednesday morning, I got an email with that link to the FEC website from a reporter on the political team here, [Roz Helderman](https://twitter.com/PostRoz), asking if I could scrape the data from the PDF into a spreadsheet so we could analyze it for a story. I said sure and created a dataset originally just consisting of transaction ID, name, and amount donated (or refunded). That was great for our purposes, and so I decided to put it on GitHub and continue to add fields myself and seek help from those who wanted the data as well.

![](https://pbs.twimg.com/profile_images/1553168502030303232/qMwu9jtL.jpg)

[Steven Rich](https://twitter.com/dataeditor)
[@dataeditor](https://twitter.com/dataeditor)

Hi, here's my first pass at converting the Inaugural committee donations PDF into data. Will add more fields soon. [github.com/washingtonpost…](https://github.com/washingtonpost/data-inaugural-committee)

[Posted Apr 19, 2017 at 6:34PM](https://twitter.com/dataeditor/status/854765104792993792)

> Hi, here's my first pass at converting the Inaugural committee donations PDF into data. Will add more fields soon. <https://t.co/2PE5J4tnma>
> 
> — Steven Rich (@dataeditor) 

**Derek Willis:** I saw the filing when it came out and had that usual familiar pang of annoyance at an image PDF, but it wasn’t something ProPublica needed to jump on right away, so I didn’t give it much thought other than looking though it a bit.

Then I saw Steven’s tweet about it and thought yeah, that’s the right thing to do. One thing about working on OpenElections is that I’ve become much better at data entry—not that there’s a scientific technique, but just from doing it. So I thought…I can contribute to this effort.

**Andrew Tran:** I saw a tweet about the FEC document from Derek Willis and thought it’d be fun to try to scrape the data from the PDFs into a spreadsheet. It took a while to script and generate and by the time it was done, I noticed some columns had a higher success rate and others didn’t—mostly in the donation amounts.

But then I saw my future co-worker Steven tweet out a repo of his clean data and saw that he was just missing the location information like address, state, and zip from the donors in the PDF, which my script had done a much better job of scraping.

It made sense to then focus on the data that he didn’t have yet. I let him know on Twitter that I was very close to putting together the last part of the data on Twitter in case anyone else was about to go down the same rabbit hole I was in.

**Source: How did you do the actual extraction work? Did you hit any weird issues along the way?**

**Steven Rich:** The extraction was a bit of a few things for me. It involved some Tabula to extract things like names and states, but some of the fields had to be manually extracted, like amounts and dates. The data was OCRed, but it wasn’t great for a lot of fields. Much of the data needed to be cleaned, but that’s probably the extent of weird issues.

**Andrew Tran:** I used the tabulizer package in R to go through the sheets and scrape the data. It was as if the whole PDF was optimized to be as difficult as possible for journalists to parse. Things that would result in the data not getting scraped: Sometimes pages of the PDF would be slightly shifted so if my scraper was looking for data in a specific area of the page, nothing would get picked up. And sometimes what was picked up just couldn’t be interpreted correctly. Numbers would have letters mixed in. Spaces between words disappeared. I’m surprised there wasn’t spilled coffee stains on any of the pages. What wasn’t picked up by my script, I filled in manually.

![](https://pbs.twimg.com/profile_images/1553168502030303232/qMwu9jtL.jpg)

[Steven Rich](https://twitter.com/dataeditor)
[@dataeditor](https://twitter.com/dataeditor)

Added month and year. Any soul out there want to do me a solid and add day? [github.com/washingtonpost…](https://github.com/washingtonpost/data-inaugural-committee)

[Posted Apr 19, 2017 at 7:51PM](https://twitter.com/dataeditor/status/854784470486011904)

> Added month and year. Any soul out there want to do me a solid and add day? <https://t.co/2PE5J4tnma>
> 
> — Steven Rich (@dataeditor) 

**Derek Willis:** I added a couple of things. One was the page number of the PDF each record appeared on, because that can be useful, and that’s a simple thing to add. Each form is just three transactions per page, so you just count. One thing is when the FEC does get around to typing things is they use an entity type—whether a donor is an individual or entity or whatever. Most are individuals so I went in and marked all individual and then marked the non-individuals as organizations. I also wanted to identify the members of congress. They were likely buying tickets for their constituents and other folks and lastly, I did the day.

Steven or someone had already done the month and the year [for each filing], and unfortunately some things you could get by OCR but not the days. So I blew through those—I did about a third of them that night and then the next morning on my commute on the Metro, I pounded out the rest.

**Rachel Shorey:** At the New York Times, I’m responsible for extracting and processing campaign finance data for regular deadlines. We’ve got a lot of tools internally to deal with the campaign filings, but the inaugural committees only file once every four years, so we hadn’t done a lot of prep work for those, and we knew it’d be custom. We expected a relatively easy-to-parse CSV file based on the past two inaugurals. But when the file came as a scanned PDF, I decided that the fastest approach was to read all 510 pages and manually enter the biggest donors into a spreadsheet so we could get some quick aggregates and get reporters’ eyes on it.

When Steven started putting the donor list on GitHub, I realized it’d be easy to correct a few OCR errors I had noticed while hand entering the data internally (one of which was on a million dollar donor’s name) so I submitted a quick pull request, which I continued doing as I noticed other issues as part of our reporting. The only “technical” part I contributed is writing a script to fix zip code formatting, which kept getting messed up when people would edit the data in Excel. We used the sheet as a backup check on some of the numbers I had hand entered. For example, I searched Steven’s doc for “$1,000,000” to make sure I hadn’t missed any million-dollar donors in my own count.

**Source: When and how did you release the extracted data?**

**Steven Rich:** I released the data on GitHub as soon as I had three fields completed and (relatively) clean. From there, I and others continued to add missing fields and clean. Once it was up, I added things like month, year, and notes. Rachel Shorey of the New York Times helped clean and standardize data. Derek Willis of ProPublica helped to add days, entity types and page references. Andrew Tran of the Connecticut Mirror (and soon to be my colleague at the Post) finished by adding address information. Chris Zubak-Skees of the Center for Public Integrity and Justin Reese, formerly of DocumentCloud, have since come in and helped to clean the data.

**Source: I know Christina Wilkie is crowdsourcing research on the individual donors—do you know of other reporters or projects using it as well?**

**Steven Rich:** I have seen some other folks visualize the data but I think what Christina is doing is by far the biggest thing being done with this data. I helped her get the ball rolling with this and am excited to see how well it’s done. It was a great idea on her part.

![](https://pbs.twimg.com/profile_images/1575309292084797443/q2Or78VE.jpg)

[Christina Wilkie](https://twitter.com/christinawilkie)
[@christinawilkie](https://twitter.com/christinawilkie)

Here's the public spreadsheet we're using to verify the records of Trump's inaugural donors. Help us! #CitizenSleuth [docs.google.com/spreadsheets/d…](https://docs.google.com/spreadsheets/d/1JaV4FmB9eoS78HR1WlHIZ0aw6GEqkYi6ccdiupVmfOA/edit#gid=2076979633)

[Posted Apr 23, 2017 at 12:18AM](https://twitter.com/christinawilkie/status/855938996228288512)

> Here's the public spreadsheet we're using to verify the records of Trump's inaugural donors. Help us! [#CitizenSleuth](https://twitter.com/hashtag/CitizenSleuth?src=hash) <https://t.co/Dna3e25KEN>
> 
> — Christina Wilkie (@christinawilkie) 

**Andrew Tran:** I pulled out the information on donors who said they were from Connecticut and sent it to the Mirror’s political reporter to dig into. I’m not sure if other local organizations are doing something similar but I hope they are. That was the whole point.

**Derek Willis:** My sister sent me [an article from the Willamette Week](http://www.wweek.com/news/2017/04/21/report-portland-hotel-owner-covertly-steered-cash-to-trump/) which mentioned that the owner of the—it was [originally a piece on the Intercept](https://theintercept.com/2017/04/21/portland-executive-covertly-donates-1-million-to-inauguration-after-being-shamed-over-trump-support/), and the Willamette Week expanded on it—a local hotel owner in Portland was tied to four donations of $250,000 through a network of LLCs. And this person had repudiated the message around the [Trump] campaign, but then they gave a million dollars to the inaugural.

The one that really caught my eye was [the Katherine Johnson one](https://theintercept.com/2017/04/20/donald-trumps-inauguration/). Had I been doing data entry of the whole thing, of just the names, I wouldn’t have given it a second thought—if I’d done the name and the address, maybe it would have come up, but probably not, it’s the next person to look at it who might be like “wow!” It underscores the value of having multiple eyeballs on all of these things. I mean, all of us looked at that record, and it didn’t strike a chord with anybody—until it did.

![](https://pbs.twimg.com/profile_images/1553168502030303232/qMwu9jtL.jpg)

[Steven Rich](https://twitter.com/dataeditor)
[@dataeditor](https://twitter.com/dataeditor)

I argued that the whole point of collecting data others didn't have was so others could have it. Competition be damned. The editors agreed.

[Posted Apr 25, 2017 at 6:21PM](https://twitter.com/dataeditor/status/856936255141158912)

> I argued that the whole point of collecting data others didn't have was so others could have it. Competition be damned. The editors agreed.
> 
> — Steven Rich (@dataeditor) 

> You can draw a line from [@Fahrenthold](https://twitter.com/Fahrenthold)'s notebook to [@christinawilkie](https://twitter.com/christinawilkie)'s spreadsheet. Collaborative data reporting, done in the open.
> 
> — Chris Zubak-Skees (@zubakskees) 

#### Credits

* Editor, Source, 2012-2018.

#### Recently

* [![](https://media.opennews.org/cache/73/5d/735d3fc10c174becd8c03bcd073bf3e6.png)](https://source.opennews.org/articles/github-actions-scrapers/)[Running scrapers on GitHub to simplify your workflow](https://source.opennews.org/articles/github-actions-scrapers/)
* [![](https://media.opennews.org/cache/72/1a/721ad1e1ec7ce1eb4e85957019c26496.png)](https://source.opennews.org/articles/mapping-historic-street-signs-nyc-chinatown/)[How we tracked down and mapped historic street signs in New York City’s Chinatown](https://source.opennews.org/articles/mapping-historic-street-signs-nyc-chinatown/)
