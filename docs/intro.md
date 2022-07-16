---
sidebar_position: 1
slug: /
---

# Fulfilment Connector Intro

The main purpose of this system is to **collect and aggregate** orders from our **Order Management Systems (OMS)** such as Shopify and Amazon and **schedule fulfilments (delivery)** with their corresponding **Third Party Logistics (3PL)** partners e.g. Aramex and Flextock. We've created a data model - which encompasses **Orders**, **Customers**, etc... - for our operations, which is stored in our internal database - currently MongoDB. This allows us to perform other stuff such as **post sale feedback** and **creating/sending personalised discount codes**.

## Getting Started
### Workers
We currently have 7 workers, each doing a specific task. This is where most of the business logic happens. These workers will periodically (e.g. every 5 mins) execute their intended purpose. 
- [FetchNewOrdersWorker](workers/FetchNewOrdersWorker) - Fetches new orders from various e-commerce stores configured in [config](#config) file and writes them into our internal database.
- [CreateFulfilmentRequestsWorker](workers/CreateFulfilmentRequestsWorker) - Scans through our database for new orders and schedules fulfilment with the corresponding 3PL partner.
- [FetchFulfilmentStatusWorker](workers/FetchFulfilmentStatusWorker) - Scans through our database for orders which have been scheduled for fulfilments and queries their status. May also notify customers through SMS or email.
- [SendNpsFormWorker](workers/SendNpsFormWorker) - Sends a feedback form to customers who have made an order 2 weeks ago to see customer satisfaction.
- [FetchNpsResponseWorker](workers/FetchNpsResponseWorker) - Fetches the NPS responses from customers and writes them into our database.
- [DiscountCodeWorker](workers/DiscountCodeWorker) - Scans through the fetched NPS responses and creates a personalised discount code which is then sent to the customer through SMS and email if possible.
- [FetchFxRatesWorker](workers/FetchFxRatesWorker) - Fetches the daily foreign exchange rates and commodity prices and writes them into our database.
### Order Management Systems
This represents a particular OMS which could be Shopify or Amazon. Each OMS can have multiple endpoints. For example, Shopify has Morocco, Egypt, Dubai, etc... endpoints which are configured in the config files (see [config](#config)).
- [Shopify](OMS/shopify-OMS) - This is a class which wraps the Shopify API e.g. to pull the latest orders from a specific store. Currently 4 endpoints are in use: Morocco (shemsiworld), Egypt (shemsi-egypt), Dubai (shemsi-uae) and Lebanon (shemsi-lebanon)
### Third Party Logistics
- [Aramex](pl3/aramex) - 3PL Partner for our Morocco operations
- [Flextock](pl3/flextock) - 3PL Partner for our Egypt operations
- [Farfill](pl3/farfill) - 3PL Partner for our Dubai operations
### Data models
- [Order](data-models/order)
- [Customer](data-models/customer)
- [Product](data-models/product)
- [Address](data-models/address)
### Utility API Wrappers
- [Rebrandly](wrappers/rebrandly) - URL shortener service
- [SmsClient](wrappers/smsClient) - Send SMS to customers
- [MailSender](wrappers/mailSender) - Send emails to customers
### Config
- [About Config Files](config/about)
- [config.ci.ts](config/config-ci)
- [config.staging.ts](config/config-staging)
- [config.production.ts](config/config-production)

[//]: # (### What you'll need)

[//]: # ()
[//]: # (- [Node.js]&#40;https://nodejs.org/en/download/&#41; version 16.14 or above:)

[//]: # (  - When installing Node.js, you are recommended to check all checkboxes related to dependencies.)

[//]: # ()
[//]: # (## Generate a new site)

[//]: # ()
[//]: # (Generate a new Docusaurus site using the **classic template**.)

[//]: # ()
[//]: # (The classic template will automatically be added to your project after you run the command:)

[//]: # ()
[//]: # (```bash)

[//]: # (npm init docusaurus@latest my-website classic)

[//]: # (```)

[//]: # ()
[//]: # (You can type this command into Command Prompt, Powershell, Terminal, or any other integrated terminal of your code editor.)

[//]: # ()
[//]: # (The command also installs all necessary dependencies you need to run Docusaurus.)

[//]: # ()
[//]: # (## Start your site)

[//]: # ()
[//]: # (Run the development server:)

[//]: # ()
[//]: # (```bash)

[//]: # (cd my-website)

[//]: # (npm run start)

[//]: # (```)

[//]: # ()
[//]: # (The `cd` command changes the directory you're working with. In order to work with your newly created Docusaurus site, you'll need to navigate the terminal there.)

[//]: # ()
[//]: # (The `npm run start` command builds your website locally and serves it through a development server, ready for you to view at http://localhost:3000/.)

[//]: # ()
[//]: # (Open `docs/intro.md` &#40;this page&#41; and edit some lines: the site **reloads automatically** and displays your changes.)
