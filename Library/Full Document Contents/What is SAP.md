---
Author: Adam Louie
Full Title: What is SAP?
Category: #articles
Publication date: 2020-02-04
URL: https://retool.com/blog/erp-for-engineers/
Summary: SAP is a major player in enterprise resource planning (ERP) software, selling around $25 billion annually. ERP systems act like a company's "brain," storing crucial operational data and enabling seamless integration across various business processes. Despite their complexity and high implementation costs, many large companies rely on ERP systems to enhance efficiency and adapt their operations.
---

![rw-book-cover](https://retool.com/blog/content/images/2019/11/r2_clip.png)

# Notes

[[Adam Louie - What is SAP?]]

***

What's SAP? And why is it worth $163B?

Every year companies spend $41B on **enterprise resource planning** software, commonly known as **ERP**. Today, almost every large business has some sort of ERP system implemented. But most smaller businesses generally don’t purchase any ERP system off the shelf, and most engineers probably haven’t seen them in the wild. So for those of us who haven’t used an ERP system ourselves… what’s the big deal? How does a company like SAP sell $25B worth of ERP software every year? And how does [77% of the world’s transaction revenue](https://www.sap.com/corporate/en/company.html), and 78% of the world’s food all flow through SAP?

ERP is where companies store their core operational data. We’re talking about sales projections, purchase orders, and inventory, as well as the processes that act upon that data (e.g. paying out vendors when a purchase order is issued). In a sense, ERP is the “brain” of a company — it stores all important pieces of data and all of the actions possible in data-driven workflows.

But before taking over the modern business world, how did ERP software get started? The story of ERP begins with major automation efforts of the 1960s: while the 1940s and 50s were focused on mechanical automation of blue collar work — think General Motors establishing their automation department in 1947 — the automation of white collar work (often via the computer!) began in the 1960s.

#### Midcentury automation: Computers enter industry

Payroll and billing were among the first business processes that computers automated. Employers needed armies of white-collar labor to manually tally employee hours in ledgers, manually multiply hours with hourly rates, and manually subtract taxes and benefit deductions… all just to run a single month of payroll! This time-consuming, repetitive process was error-prone for humans, and was a natural fit for computerized automation.

By the 1960s, many companies used IBM computers to automate payroll and billing tasks. Data processing – an outdated term whose lasting legacy is  

[Automatic Data Processing, Inc](https://en.wikipedia.org/wiki/ADP_(company))’s name – is what we’d call IT today. Software engineering wasn’t yet a discipline, so these departments often took employees from analytical backgrounds taught them to program on the job. Purdue had just formed the first Computer Science department in the U.S. in 1962, and the first CS grads started appearing a few years later.

![](https://retool.com/blog/content/images/2019/11/early_eng.jpg)
Programming for automation/data processing was challenging in the 1960s because of memory constraints. There were no high level languages, no standardized operating systems, and no personal computers – only big expensive mainframes with little memory running programs on reels of data tape! Programmers often developed at night in order to maximize compile time. It was common for companies like GM to write their own operating systems to get the most out of their mainframes.

While today we run application software on top of a few common operating systems, this wasn’t the case until the 1990s. During the [middle ages of computing on mainframe machines](https://www.authorhouse.com/BookStore/BookDetails/230036-Computing-in-the-Middle-Ages), 90% of all software sold was custom-built; only 10% was purchased off the shelf.

This landscape deeply influenced how companies developed their technology. Some imagined that the future of software would involve industry-standardized hardware, OS, and programming languages, like [the SABRE system](https://tryretool.com/blog/air-travel-software/) for the airline industry (that’s still used today!). Most companies stuck to building their own completely isolated software, often reinventing the wheel.

#### The birth of standard software: SAP’s extensible development

In 1972, five engineers left their jobs at IBM to pursue a software contract with a large chemical firm called ICI. Their new company, SAP (Systemanalyse und Programmentwicklung, or System Analysis and Program Development in English), like most software companies at the time, was essentially a software consultancy. SAP would embed themselves in their customers’ offices and develop on their customers’ computers, writing mostly logistics management software.

![](https://retool.com/blog/content/images/2019/11/sap_history_founders_001_t_40900x589_920715.jpg)
Business went well: SAP ended their first year with DM620,000 in revenue, a little over $1M in today’s dollars. Soon, they started selling their software to additional customers, porting it to different operating systems when necessary. Over the next four years, they acquired over 40 customers, 6x-ed their revenue, and grew from 9 to 25 employees. This might be a far cry from today’s coveted [T2D3 growth curve](https://blog.fusebill.com/t2d3-path-to-saas-growth-1b-valuation), but SAP’s future was bright.

SAP’s software was special for a few reasons. At the time, most software ran overnight and produced output on paper data tapes, which you’d check the next morning. Instead, SAP designed their software to produce output in real-time for customers, and ran it on computer monitors (which cost around $30K at the time).

But most importantly, SAP’s software was designed to be extensible from the start. For SAP’s original contract with ICI, SAP didn’t build software from scratch, as was the norm at the time, and instead built on top of a previous project. When SAP released their financial accounting software in 1974, their intention was to build additional software modules on top of it and sell them in the future. This extensibility was SAP’s defining feature. The interoperability across client contexts was considered radical at the time because other approaches started from scratch for every client.

#### The importance of integration

When SAP introduced their second software module for manufacturing to go with their first finance module, the two modules were able to seamlessly interact with each other since they shared the same database. This integration made the combination of the two modules significantly more valuable than the individual pieces of software.

As software automated particular business processes, its impact was highly dependent on access to data. Purchase order data sits in the sales software, inventory data sits in the warehousing software, etc. And because these systems don’t interact, they needed to be synced regularly, and that often meant having a human manually move data around.

Integrated software solves this by facilitating communication across company systems and enabling new kinds of automation. This kind of integration – between various business processes, as well as data sources – is a key feature of ERP systems. This became especially important as hardware spoke more to software, opening new automation opportunities; and ERP shined.

The speed at which data can be accessed with integrated software empowered companies to [completely rehaul their business models](http://facweb.cti.depaul.edu/jnowotarski/is425/hbr%20enterprise%20systems%20davenport%201998%20jul-aug.pdf). For Compaq, ERP allowed them to shift their distribution strategy to a made-to-order model (i.e. building a computer only once the order is explicitly placed). This model saves money by eliminating inventory cost, but relies on a speedy turn-around time — precisely what a good ERP implementation helps with. When IBM underwent a similar effort, it reduced replacement part shipping time from 22 days to only 3 days!

**Subscribe to the Retool monthly newsletter**  

Once a month, we send out top stories (like this one) along with Retool tutorials, templates, and product releases.

#### What ERP software actually looks like

The words “enterprise software” don’t provoke images of slick, user-friendly UIs, and SAP’s ERP software is no exception. A basic installation of SAP has 20,000 database tables, 3,000 of which are configuration tables. In those tables, there are ~8,000 configuration decisions you need before even getting started. And that’s why [SAP Configuration Specialist](https://www.google.com/search?q=SAP+configuration+specialist) is an actual job title!

Despite being difficult to set up, SAP’s ERP software delivers on its key value of broad integration across multiple business processes. That integration leads to thousands of use cases in an organization. SAP organizes these use cases into Transactions, which represent business actions. Some example transactions include “create an order” and “display a customer.” These transactions are organized in a nested directory format. So to find the `Create a Sales Order` transaction you’d navigate to `Logistics` and then `Sales`, and then `Order`, where you’d find the actual transaction.

![](https://retool.com/blog/content/images/2019/11/sap_screenshot.png)
Calling ERP a “transaction browser” would be surprisingly accurate. It very much looks like one, complete with a back button, zoom controls, and a text box for “TCodes”, the equivalent for URLs in a web browser. SAP has [over 16,000 different types of transactions](http://www.easymarketplace.de/transactions-f-h.php), so navigating through the tree of transactions can be daunting without these codes.

Despite the dizzying number of available configs and transactions, companies still have unique use cases and need highly customized actions. To handle such unique workflows, SAP also has a built-in programming environment. Here’s how it each part works:

##### Data

Developers can create their own database tables using the SAP interface. These are relational tables, with the feature you come to expect from a SQL database: columns of various types, foreign keys, value constraints, as well as read/write permissions.

##### Logic

SAP developed a language called ABAP (footnote: Advanced Business Application Programming, originally Allgemeiner Berichts-Aufbereitungs-Prozessor, German for "general report creation processor") that lets developers execute custom business logic in response to certain events or on a timed schedule. ABAP is a verbose language, with about 3x the number of keywords compared to JavaScript (here’s a [2048 implementation in ABAP](https://help.sap.com/doc/abapdocu_750_index_htm/7.50/en-US/abengame_2048_abexa.htm)). Once you’ve written your program (SAP provides an editor), you ship it as own transaction, complete with its own TCode. You can also customize existing behavior through an extensive system of hooks called “business add-ins”, where you can configure your program to run when a certain transaction executes – similar to SQL triggers.

##### UI

SAP also comes with a drag-and-drop form builder for creating UIs. It comes with handy features like generated forms based off your database table, but it’s fairly difficult to use. My favorite part of the form builder is drawing out the table columns:

![](https://retool.com/blog/content/images/2019/11/sap_gif.gif)
#### Challenges with Implementing ERP

ERP isn’t cheap. A large multinational company may spend **$100 million to $500 million** in total on implementation: $30 million in software license fees, $200 million in consulting fees, millions for hardware, and millions more to train managers and employees. A full implementation takes four to six years. [A CEO of a large chemical firm](http://facweb.cti.depaul.edu/jnowotarski/is425/hbr%20enterprise%20systems%20davenport%201998%20jul-aug.pdf) was quoted as saying, "competitive advantage in this industry might just come from doing the best and cheapest job at implementing SAP.”

And it’s not just about money — implementing ERP is a risky undertaking, and the results vary wildly. A best case scenario looks like Cisco’s ERP implementation, which took them 9 months and $15 million. In comparison, Dow Chemical’s implementation, for example, took $1 billion and 8 years; the [U.S. navy spent $1 billion on four different ERP projects that all failed](https://www.computerworld.com/article/2558133/gao--navy-sinks--1b-into-failed-erp-pilot-projects.html). A whopping [65% of executives](https://go.galegroup.com/ps/anonymous?id=GALE%7CA54003837&sid=googleScholar&v=2.1&it=r&linkaccess=abs&issn=00178012&p=AONE&sw=w) believe ERP implementations have a moderate chance of hurting their businesses (which isn’t something you’d typically hear when evaluating software!).

The integrated nature of ERP makes adopting it a company wide effort. And because companies only extract the value once ERP is adopted everywhere, it’s particularly risky! Implementing ERP isn’t just a purchasing decision: it’s committing to redoing how you handle operations. Installing the software is the easy part; adjusting the whole company’s workflow is the most of the work.

A company implementing an ERP system oftentimes hires a consulting firm like Accenture and pays them millions of dollars to work with individual business units and figure out how to integrate ERP into their processes. And once the integration is underway, the company has to start training all the employees on how to use the system. [Gartner recommends](https://books.google.com/books?id=GAOHQvgpeNYC&pg=PA138&lpg=PA138&dq=gartner+erp+training+budget+17%25&source=bl&ots=QLxeLWy0s-&sig=ACfU3U2yxiaE3A-sP43Ctbxg1cRhfO9ofA&hl=en&sa=X&ved=2ahUKEwif5oT1zqzlAhWRop4KHVebCA0Q6AEwGHoECAkQAQ#v=onepage&q&f=false) reserving 17% of the budget just for training!

Despite all the hassle, most Fortune 500 companies had ERP systems implemented by 1998, a process that was accelerated by the approach of Y2K. ERP has continued to grow and is a [>$40B market today](https://www.alliedmarketresearch.com/press-release/global-ERP-software-market-is-expected-to-reach-41-69-billion-by-2020.html), one of the biggest segments in the software industry.

#### The modern ERP software industry

The largest players in ERP today are Oracle and SAP. While both are market leaders, their ERP products are surprisingly different. SAP’s product has been largely built in-house, while Oracle has aggressively acquired their way to the front of the pack, buying out competitors like PeopleSoft and NetSuite.

Oracle and SAP are so dominant that even [Microsoft uses SAP](https://www.erpsoftwareblog.com/2012/11/why-does-microsoft-hq-use-sap-instead-of-microsoft-dynamics-erp/) instead of their own ERP offering, Microsoft Dynamics.

Because most industries have fairly specific ERP needs, Oracle and SAP have pre-built configurations for many industries, such as food, automotive, and chemical, as well as vertical-built configurations, such as sales organization specific processes. Nonetheless, there’s always space for niche players, which are generally vertical-focused:

Vertical specific ERPs can specialize through integrations and workflows that are specific to their target market: healthcare ERPs, for example, [can implement HIPAA protocols](https://www.ifourtechnolab.com/hipaa-compliant-enterprise-resource-planning-system-for-hospitals).

Specialization isn’t the only differentiator, though: a few startups are bringing more modern software platform practices to the ERP “table.” An example is [Zuora](https://www.zuora.com/), a platform specifically focused on enabling companies to integrate (with ERPs!) and manage their subscription businesses. Others like Anaplan and Zoho are doing the same.

#### ERP on the rise?

It’s 2019, and SAP is a giant: they had $25B in top line revenue last year, and have a [market cap](https://en.wikipedia.org/wiki/SAP_SE) of almost $150B. But the software world isn’t what it used to be. When SAP first came out, data was siloed and difficult to integrate; storing it all in SAP was the obvious answer.

That’s now rapidly changing; on the back-end, most modern enterprise software (e.g. Salesforce, Jira, etc.) now have good APIs for exporting data. ETL + data lakes are on the rise: [Presto](http://prestodb.github.io/), for example, facilitates cross-database joins that weren’t possible just a few years ago.
