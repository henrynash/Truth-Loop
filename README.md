[hi-level-arch]: images/architectural-diagram.png "High Level Architecture"
[components]: images/components.jpg "Components"
[openshift]: images/openshift.png "OpenShift Console"
[dataflow]: images/dataflow.png "Data flow"

# Team Truth

## Team Members

The initial version was developed by members of IBM in the summer of 2020:
Aakansha Agrawal, Khadija Al-Selini, Parisa Babaali, Boz Bosma, Kimberly Cassidy, Stephanie Daher, Michelle Esselen, Peter Ihlenfeldt, Abiola Jones, Rahul Kalluri, Joe Konathapally, Frank Madden, Henry Nash, Sharon Osahon, Bimsara Pilapitiya, Ya Jiao Zheng,

## Contents

1. [Overview](#Overview)
   1. [What's the Problem?](#whats-the-problem)
   1. [How can technology help?](#how-can-technology-help)
1. [The Idea](#the-idea)
1. [Diagrams](#Diagrams)
1. [Technologies](#Technologies)
1. [Getting Started](#Getting-started-by-installing-and-running-the-components)
1. [Resources](#Resources)
1. [License](#License)
1. [Contributing and Developer information](#Contributing-and-Developer-information)
    1. [Contributing](#contributing)
    1. [Future Enhancements / Undecided Aspects](#future-enhancements-and-undecided-aspects-of-the-solution)
    1. [Privacy Considerations](#privacy-concerns)

## Overview

### What's the problem

Concerned and impacted citizens don't have a straightforward way of knowing 
what or how policies, regulations and legislation (throughout this document 
referred as either **Legislative Artifacts** or **PR&L**) impact them or 
what they can do in response.

### How can technology help

What is missing from this situation, and what this technology was intending to provide, is a means for people to:

- readily understand the PR&L language and intent without being a legal expert
- sort or filter the PR&L efficiently
- digest an understandable summary of PR&L
- explore related or supporting information, advocacy groups and other PR&L
- make a determination of whether it will impact them
- engage in the process
- communicate the potential effects of these PR&L on their life, family, or community
to the author or sponsor of the legislation
- share their stories and experiences with fellow residents and policy makers
- see what other people in the community are saying
- get an idea of the general sentiments people have expressed about the PR&L
- engender dialogue between residents and PR&L authors and sponsor

## The Idea

The driving idea behind the software is to provide a platform that is capable of storing
curated PR&L information, as determined by the community (however large or small) it serves.
Further, it is to provide a mobile-friendly way for users to examine that PR&L, increasing
their legal awareness, and to allow them to communicate their reactions and thoughts via
the recording of video testimonials to be shared with the community and the people
responsible for the creation of the PR&L.

## Diagrams

![architecture diagram](/images/high-level-architecture-diagram.png)

[NB Diagram above is a placeholder, need to swap React for Vue, remove HERE, and swap Watson Media for Watson Assitant]

This solution combines use of a media server (currently Watson Media) and data storage to hold the curated legislative artifcats, and the meta-data to link these together.

1. The User launches the mobile app and can view the range of curated legislative artifcast that have been created.
1. The User can post their own (video) story that may support or challenge the legislated artifacts
1. The User can...

There is a separate administrative interface that allows the site owners to curate the PR&L information, with the following attributes

- simple, intelligible summary that makes it easy to understand its potential impact
- categories it pertains to (law enforcement, healthcare, zoning)
- geospatial areas of pertinence (postal codes, cities, counties, e.g.)
- the type of the artifact (bill, law, policy, regulation, e.g.)
- (other) related PR&L in the system
- legislator, author, sponsor of the PR&L
- related advocacy groups or digital social communities
- related articles or supporting documentation
- a link to the actual full text of the PR&L  

[HN - do we actually do any of the above yet?]

## Technologies

- [IBM Cloud Documentation Home](https://cloud.ibm.com/docs/home/alldocs)
- [Kubernetes in IBM Cloud](https://cloud.ibm.com/docs/containers?topic=containers-cs_cluster_tutorial)
- [PostgreSQL](https://cloud.ibm.com/docs/databases-for-postgresql?topic=databases-for-postgresql-getting-started)
- [OpenShift](https://cloud.ibm.com/docs/openshift?topic=openshift-getting-started)
- [Node.js](https://nodejs.org/en/docs/)
- [ExpressJS](https://expressjs.com/)
- [Vue.js](https://vuejs.org/)

## Getting started by installing and running the components

### Prerequisites

- Register for an [IBM Cloud](https://www.ibm.com/account/reg/us-en/signup) account.
- Install and configure [IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started#overview).
- Install [Vue CLI dependencies](hhttps://cli.vuejs.org/guide/installation.html) which should ensure you have the dependencies installed, namely
    - [Node.js](https://nodejs.org/en/)
    - [Watchman](https://facebook.github.io/watchman/docs/install)
    - [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) if using macOS/iOS
    - [CocoaPods](https://guides.cocoapods.org/using/getting-started.html) if using macOS/iOS
- Clone the [repository](https://github.com/henrynash/policy-truth-frontend).

### Steps

1. [Provision a Postgres instance on the IBM Cloud](#1-Provision-a-Postgres-instance).
1. [If you want to use the video services, provision an instance of Watson Media](#2-Provision-a-CouchDB-instance-using-Cloudant).
1. [Run the server](#3-run-the-server).
1. [Run the mobile application](#4-run-the-mobile-application).

### 1: Provision a Postgres instance

Log into the IBM Cloud and provision a [Postgres instance](https://cloud.ibm.com/catalog/services/databases-for-postgresql).

[HN - These are not accurate - also, there is no Postgres lite tier!! eeek]

1. Choose your Databases for Postgres plan. You should choose an appropriate region, give the service a name, and it is recommended you choose **Use only IAM** under **Available authentication methods**. You can leave the other settings with their defaults. Click the blue **Create** button when ready.
1. Once your Postgres instance has been created, you need to create a service credential that the CIR API Server can use to communicate with it. By selecting your running Postgres instance, you can choose **Service credentials** from the left-hand menu. Create a new service credential and give it a name (it doesn't matter what you call it).
1. Once created, you can display the credentials by selecting **view service credentials**, and then copy the credential, so you are ready to paste it into the code of the API server in Step 3.

### 2. Set up an instance of Watson Media

Log in to IBM Cloud and provision a Watson Media instance.

[HN - These are not accurate]

1. Provision an instance of **Watson Media** [IBM Watson Media](https://www.ibm.com/products/video-streaming/pricing).
1. Launch the Watson Assistant service.
1. [more]
1. Note the **Assistant ID**, **API Key**, and **Assistant URL**. For **Assistant URL**, make note of the base URL/domain (e.g., `https://api.us-south.assistant.watson.cloud.ibm.com` or `https://api.eu-gb.assistant.watson.cloud.ibm.com`) and not the full directory/path. You will need all three of these values in Step 3 below.

1. Go to **Preview Link** to get a link to test and verify the dialog skill.

### 3. Run the server

To set up and launch the server application:

1. Go to the `policy-truth-backend` directory of the cloned repo.
1. Copy the `.env.example` file in the `policy-truth-backend` directory, and create a new file named `.env`.
1. Update the newly created `.env` file and update the `DB_HOST`, `DB_USERNAME`, `DB_PASSWORD`, `DB_PORT` and `DB_DATABASE_NAME` with the values from credential you obtained when create the Database instance Step 1.
1. Also update the `MEDIA_USERNAME`, `MEDIA_PASSWORD`, `MEDIA_CLIENT_ID`, `MEDIA_CLIENT_SECRET`, and `MEDIA_TYPE` with the values from creating your instance of Watson Media, from Step 2.

1. From a terminal:
    1. Go to the `policy-truth-backend` directory of the cloned repo.
    1. Install the dependencies: `npm install`.
    1. Launch the server application locally or deploy to IBM Cloud:
        - To run locally:
            1. Start the application: `npm start`.
            1. The server can be accessed at <http://localhost:3000>.
        - To deploy to IBM Cloud:
            1. Edit the **name** value in the `manifest.yml` file to your application name (for example, _my-app-name_).
            1. Log in to your IBM Cloud account using the IBM Cloud CLI: `ibmcloud login`.
            1. Target a Cloud Foundry org and space: `ibmcloud target --cf`.
            1. Push the app to IBM Cloud: `ibmcloud app push`.
            1. The server can be accessed at a URL using the **name** given in the `manifest.yml` file (for example,  <https://my-app-name.bluemix.net>).

### 4. Run the mobile application

To run the mobile application (using the Xcode iOS Simulator):

1. Go to the `client/mobile-app` directory of the cloned repo.
1. Copy the `.env.example` file in the `client/mobile-app` directory, and create a file named `.env`.
1. Edit the newly created `.env` file:
    - Update the `SERVER_URL` with the URL to the server app launched in the previous step.
1. From a terminal:
    1. Go to the `client/mobile-app` directory.
    1. Install the dependencies: `npm install`.
    1. Install the dependencies: `npm run serve`.
    1. Go to the `ios` directory: `cd ios`.
    1. Install pod dependencies: `pod install`.
    1. Return to the `mobile-app` directory: `cd ../`.
    1. Launch the app in the simulator: `npm run ios`. You should be running at least iOS 13.0.
    1. The first time you launch the simulator, you should ensure that you set a Location in the Features menu.

With the application running in the simulator, you should be able to navigate through the various screens:

[HN - replace the items below with our own screen shots]

![Intro Screen](/images/0-screen-home.png)
![Donate Screen](/images/1-screen-donate.png)
![Search Screen](/images/2-screen-search.png)
![Chat Screen](/images/5-screen-chat.png)
![Map1 Screen](/images/3-screen-map.png)
![Map2 Screen](/images/4-screen-map.png)

## Resources

- Any non-tech resources go here (e.g. links/videos to legislation, and the problem being solved)

## License

This solution is made available under the [Apache 2 License](LICENSE).

## Contributing and Developer information

**[HN - still working on everything below this line ....]**

### The data flow

![dataflow]

### The components of the system

![components]

### Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

### Future Enhancements and Undecided Aspects of the Solution

[HN - we should summarize here, the detail shoudl become github issues in our repo]

One first step in contribution would be to route the back end database to allow viewing and filtering of policies on the front end through API calls.

One functionality that has caused technical issues is the implementation of the video testimonial uploading for different policies. Various approaches have been researched, including use of a no-streaming solution using Cloud Object Storage to call stored videos to be downloaded and then played back to the user, upon loading of the page. As the videos will have a restriction of 60 second time limits, optimisation can be made to minimise the overhead of waiting to download the entire video before playback. This implementation is certainly feasible, however it is not scalable when taking into account users with potentially weaker network connections.

- **security:** An efficient expansion to secure data storage (particularly regarding the video implementation) is required to ensure all user data is kept safe. Implementation of user accounts that safely store user data may even assist in developing a more convenient solution, however the privacy implications that comes with this should also be assessed.
- **privacy considerations around videos and location information:** Consideration of the metadata around user testimonials will be essential in providing a solution that focuses on privacy risks.
- **sourcing legislation information:** Several data sources will be required to adapt the solution for all locales, therefore expansion of this project will be impacted massively by taking into account the structures of legislation from other countries. 
- **policy upload and curation:** Thought would be needed in deciding who would curate the uploading of policy data. Would the database population of policy data be automated to pull all newly proposed policies from government sources, requiring an interface to filter suitable policies to then be pushed into the database? Or would the curation be done manually, which allows for more control with the drawback of lower scalability?
- **moderation of uploaded videos/text:** How is video or accompanying text reviewed to ensure community guidelines are being followed, and that users are misusing functionalities? Applications of moderation can include profanity detection, manual moderation via user admins, or through flagging and reporting of user content. Furthermore, consideration should be made to assess whether these implementations address the spirit of the solution, e.g. how to distinguish the software from social media settings where users already share political views. 
- **Natural Language technology:** Work has already begun on refining a pipeline to extract text from video submissions to implement tone analysis, which will help to identify various characteristics and give more meaning to the video testimonials from users. Further expansion can be made to analyse profanity and inappropriate language submitted by users, to address moderation of user content.

### Privacy Considerations

**Personal Information (PI) is prevalent within the Team Truth solution (please see the PI Data Map below). The following risks are noteworthy:** 
 - **Child-related:** There is the possibility that child-related PI could be entered into the system. Data protection law requires verification and authorisation by parents or guardians.  In turn, a formal Privacy Impact Assessment may be required to mitigate risks; 
 - **Encryption:** The storage and movement of PI must have technical and organisational security measures such as encryption, to ensure it is not accessed by those without authorisation (which is a data breach, and breach compliance obligations then follow);
 - **Storage:** Where the PI is stored is an issue, as external providers would need to have security measures in place to protect it. There is also indemnity-related issues with the use of third parties. Also, it unclear at this stage where PI will actually be stored;  
 - **Cross-Border Transfers:** After determining storage, it is imperative to know where the data actually resides, which enables managing “adequacy” of cross border transfers.  Many Cloud providers have overseas operations, and employ a ‘follow the sun’ approach to storage; 
 - **Access to the Data:** Any unauthorised access to PI is a data breach and therefore comes with considerable data protection compliance obligations (e.g. a requirement to notify regulators and possibly the data subjects as well);
 - **Monitoring of Qualified Content:** Forwarding the content on without any monitoring or vetting of it beforehand risks not only defamation claims, but could be used by anonymous people to espouse hate speech. Both of which undermine the value of the solution; 
 -	**Deletion:** Under data protection law, you can only hold on to the data for as long as is necessary. So at some point it will have to be deleted;  
 -	**Data Sharing:** If any data sharing is desired with third parties, e.g. Social Media, data sharing agreements will need executing. Otherwise, processing may be deemed a breach; 
 - **Privacy Notice(s):** A notice informing data subjects of the purpose of the processing, as well as the rights of the data subjects, is required. Otherwise, this would be a breach;
 - **Controller:** Processor obligations – IBM will act as both a Controller – determining the purpose of the processing – and as a Processor.  Accordingly, obligations are two-fold.  

![privacy map](images/privacy-map.png)
