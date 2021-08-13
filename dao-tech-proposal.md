**NOTE: THIS DOCUMENT IS VERY MUCH A WORK IN PROGRESS.**

# Overview

This specification summarizes requirements to develop the technology to be used in the initial vote to burn the tokens or form the IOTA Community Treasury.

Assuming the decision is made to form a Community Treasury, this voting system can be adapted to support the early operations of the newly created Community Treasury.

The following overview highlights various use cases and high-level technical designs combined with mock-ups of the proposed system.
A more detailed design will follow once requirements,dependencies and scope are refined and agreed upon. 

Flexibility is important at this stage. Therefore the initial design provides the opportunity to evaluate various technical approaches that could be prototyped and decided upon in the future.The intent is to encourage a more agile process as we move forward.

**Voting tech consists of three parts:**
- Hornet Node plug-in. See RFC: https://github.com/WernerderChamp/protocol-rfcs/blob/master/text/0000-chrysalis-referendum/0000-chrysalis-referendum.md
- DAO Website
- Firefly plug-in

The requirements detailed below were established based on numerous discussions and meetings that took place on IOTA's Discord #governance and #voting-tech channels. 
#Governance meetings took place on Thursdays 4pm CEST.

You can find various documented meeting notes in the governance [Github repository](https://github.com/iota-community/Community-Governance/tree/main/meetings). 

Finally, many conversations and  meetings with several community members took place to help develop the approach.


> **Once this document is finalized**; it should be scrutinized by the community and agreed upon. This proposal **"could"** essentially become a requirements document for the voting tech that will be need to be built. Thus establishing the base specification.

Key caveats:
>   The Hornet node plug-in is (supposed to be) built by the Hornet team.
>   
>    The other two-parts are still to be decided. 
>   
Finalizing this document **"will"** speed up the process of finding the right team(s) to build the voting tech. 

We are flexible on the approach taken in this proposal and are open to discuss alternative strategies.

## How to propose changes in this document
Please submit your suggestions via pull request within [this](https://github.com/iota-community/Community-Governance) repository.

## TODO

- [ ] expand uses cases in more details
- [ ] expand possible technical solutions
- [ ] understand how to store/access historical vote results (hornet permanode with voting plugin??, github?)
- [ ] will Firefly be used to manage proposals or only active one / voted on
- [ ] explain in detail how we post new proposal into the tangle and make it truly immutable (before it’s uploaded to Node)
- [ ] Establish potential transition plan to ISCP and develop an approach to make this transition invisible to the end user
- [ ] IOTA DID Is that something we want to consider already or wait until it's mature enough? Github/Discord login might be sufficient enough for now. 
- [ ] Initiate process to agree to a domain name

## Terms
- [Hornet](https://github.com/gohornet/hornet) - Community driven IOTA Full Node- [Hornet Plugin](https://github.com/gohornet/hornet/tree/main/plugins) - Extension within Hornet that can be used by various node operators
- [Firefly](https://github.com/iotaledger/firefly) - IOTA's official wallet 
- Proposal - Proposal to be voted on by IOTA's token holders
- Question - Each proposal can consist of multiple question and each of them can have various voting options.
- Voting Option - Option that can be voted on within the question.
- Vote - A casting vote by IOTA token holder.
- [ISCP](https://github.com/iotaledger/wasp) - IOTA Smart Contract Platform

## Key design decisions

It is desired to get this tech up and running ASAP. The intent is to avoid building an overly complex system that will require several modifications over time. 

For example, many parts might change due to the [ISCP](https://github.com/iotaledger/wasp) introduction and the DAO intends to start voting ASAP. 

Once ISCP is introduced on IOTA's Mainnet, we should move away from some parts highlighted below in the system (i.e. Github) and use ISCP instead. Ideally, we could do this without a significant impact to the users. 

Lastly, it is essential for the community to start making decisions ASAP if we are going ahead to BUILD or BURN treasury funds, as this can significantly impact IOTA's future adoption.

### Github
We envision IOTA's Community Governance Github repository to be utilized for proposal management. This will serve as a staging area for proposals prior to  their final submission to the community nodes and Firefly.

Github is considered a trustworthy source and provides a fairly secure environment for proposals at this stage. 

This method should be sufficient for our initial rollout as we await the release of ISCP.

The benefits of this approach is that it provides enough transparency into the process and allows everyone to participate.

### Hornet Node plugin
As specified within the RFC, participating node operators can utilize the Hornet plugin to provide critical functions to support the voting process. Such as: 
- Track available proposals for the vote, their status, and progress
- provide API endpoints for users and applications to fetch this data from the nodes
- Ability to count the issued votes based on token amount, holding period and stated opinion on UTXO's and produce the final results of every vote out of this counting process.

### Website
The Website should be a simple single page application (React, Angular, Vue, etc.) that sources data from the these two sources:
- Github
- Hornet Node

Proposal management and voting will be managed within the two listed above and the website. It's a simple UI design on the top to present the information in a friendlier fashion to help users better understand the process.

Additionally, it could be leveraged to provide an introduction to the voting process and provide tutorials for the user on how to manage his/her voting attempt.

> Potentially, the Website can integrate with Github and allow proposal management within the application (i.e. simplify it for users without Github knowledge). This can help transition to ISCP as users do not need to get used to the new system, and it'll be just underlying tech that changes.
> 
> We could build the Website with Docosaurus similar to the Wiki System. The rich text editor developed by Jeroen could be an optimal tool to make it easy for users to create proposals and submit them as pull requests to the GitHub Repo. The plugin is designed to do exactly this so that we could leverage this tool. The structure of a proposal would be predefined in a template and the user will be able to fill in the fields with the required information.

## Firefly plugin
The extension for Firefly will support the ability to view proposals that are "available for a vote", "in progress proposals", and "finished proposals". 
Main purpose is the ability for the user to cast their vote, change their vote and see votes of other users (generalized).

# Detailed Usage

## Github
### Draft/Request for Comments Proposal
* A user creates a new pull request with a proposal (DRAFT) specification against the governance repository master branch
* The user provides sufficient information within this proposal
* The created pull request gets shared within the community to provide feedback/comments/etc.
* The proposer should make sure to gather a lot of interest and promote their idea before it's passed to the submission stage.
### Submit Proposal
* Once the proposer is comfortable to proceed, they will need to submit their proposal for approval by moving the created pull request from "Draft"- status to "Ready for Review"
* The pull request enters a defined, required approval process, and awaits all approvals (mixture of IF/community members)
    * We can automate this via GitHub Actions inc. various verifications. 
* **(TBD)** approval opens a pull request and decide to approve/reject based on numerous factors:
    * number of votes within pull request?
    * number of people monitoring pull request?
    * number of follow-ups?
    * etc.
### Proposal Approval
* Once the pull request is approved, it'll get merged into the master branch under the "proposal" folder. 
    * GitHub Action can be used to publish the proposal
    * This tool could be used to automatically create an immutable hash of the release and post it as a message in the tangle, so everyone could have a proof and the hash of the valid proposal: https://github.com/iotaledger/gh-tangle-release
    * GitHub Action can also post information about a new proposal to the #governance-discussion channel on Discord
* Any new proposal within the "proposals" master branch are picked and promoted within Hornet nodes (with voting plugin) & Firefly
    * Upload to nodes can be done manually. Ideally, we can build a tool which can automate this and node operators can enable this if they want to.
### Proposal Termination
* **(TBD)** Do we want to handle it, how, when? Amendment to an existing proposal with new file "proposal" folder in master branch?

## Firefly
### View Proposals
* User navigates into the voting tab
* User can view existing proposals (only those approved from the GitHub Master Branch, sorted by commencing date).
    * Note, Firefly could get proposals from the master branch and also from nodes depending how we design the API requests. There might be a scenario where it's not yet uploaded to nodes. **(we should discuss if we will not just use a request to GitHub and skip the node part)**
* By default, users only see upcoming/in progress proposals
* They can filter and see ended ones (history)
* User should be able to easily identify proposals still pending their vote 
* User can easily open proposal on the DAO Website to access fully history (i.e. pre-approval)
### Submit Vote
* User selects the proposal which is still pending their vote
* User votes on a voting option within each question of the proposal
* User can select the wallet they want to vote with (Amount of Funds to allocate as opinion)
* User can share their vote options online (discord/Twitter/etc.) This could potentially include link to their actual "vote" other users can view within DAO Website. **(Also - discussable option, most users may want a bit privacy in this?)**
### Change Vote
* User opens commencing/holding proposals and can view their existing vote
* They can change their vote
* They get the direct information of the impact of the change (i.e "due to the change you have currently gained XXX voting power for option X and now will continue to build up voting power on option Y")
* They share their vote easily online again
### View Vote Status
* User is able to view status and progress of each proposal and their own vote
* **TBD**(Ability to view individual votes by other token holders??)
* Ability to view overall results 
* Ability to view votes from various nodes allowing them to understand if nodes agree to the count.

## DAO Web
### Home page
* Ideally starts with video what DAO is all about
* Continues with more detailed explanation
* Ends with "cast your vote" / "create a proposal"


### User management
- This might be an ideal test case for IOTA DID implementation
### Proposals
* Ability to view all proposals (drafted/submitted/approved/commencing/holding/ended)
* List shows all proposals
* Ideally widget shows currently commencing/holding proposals
* Ability to draft a new proposal
    * This could be an explanation how to do it in Github
    * Or we build integration on top of the Github with the markdown editor that created pull requests - ideal!
* Ability to cast vote and Firefly opens automatically (deep link)
* For the DAO with ISCP this will need to be extended into a full forum style discussion board where the community can communicate with the proposal submitters and discuss all topics related to the proposals
#### View Individual proposal
* Current status and all attributes
* If active/ended
    * User can see all votes by individual users with linked to explorer
    * Overall progress of the vote
    * Reported numbers by various nodes
    * Ability to cast a vote and Firefly opens automatically
### Subscribe to news
* Email
* Twitter
* Discord

> Anyway, I'm not a big fan of the email database tbh. We can discuss a good approach to this. 

### About
* About DAO and it's history
* Members that approve proposals within GitHub (members of iota-community/Community-Governance ?)

### Firefly Voting Logic
![](https://i.imgur.com/Fu9Npwg.png)

## Firefly / DAO Website Mock-ups
TBD

### IDEAS 
Proposals on DAO Website could be similar to Github Discussions:
![](https://i.imgur.com/5jjwyax.png)

# Questions
- What do we think about mock-ups? - see figma design by IF I shared
- Is there any sample plugin already developed for Firefly (I couldn't find one)? - yes, Umair started to build, but they didn't finish due to timing, but the architecture allows it
- Are we all right for Firefly to contact GitHub APIs, any concerns? - Yes, that's the favored approach, no concern's - it needs to be whitelisted in Firefly
- Wallet.rs supports no value transaction? (to me, it seems it does) - Yes, it does
- Any concerns for ledger users? as we would have to support that. - No, that should be no big deal; they have all the blueprints now ready from the ledger integration
- Will it be all right to expand deep links in Firefly to support voting? Yes, deep links are not active yet, but the PR is open, and this should be done
- How does Firefly access historical data (i.e., connects to permanode)? - No permanode atm, so history gets lost in the nodes. Firefly stores what it knows in Stronghold atm. We could potentially save the Proposal history in Github - Database and Firefly access this with API
- How long would you rough estimate mobile support? Should make this solution be dependent on it?
- Any concerns opening external links from Firefly (to our Website)? Need to be whitelisted beforehand
- Any concerns to source data from multiple nodes? (i.e. present voting stats from various nodes)

# Notes (to initiate above)

## Simple notes/overview, for now, to agree on for RFC
- Node implementation
    - ~~see the other RFC https://github.com/WernerderChamp/protocol-rfcs/blob/master/text/0000-chrysalis-referendum/0000-chrysalis-referendum.md~~
  - This needs to be still worked out more...  data sizes are too big; IMHO 1 MB proposal size is not useful

- Proposals management/retrieval within Firefly
    - ~~Github will be used to manage and store proposals. The Website can provide a guide for this.~~ 
    - During the building / creating phase we can think about keeping the community proposal option flat, as it would be ideal that mostly the "builders" of the DAO system use this as a way to gather the opinion of the community how to proceed in the development. Random community proposals might hinder and slow down the development uneccessary
    - ~~Pull Requests will be used to manage the flow. This includes submission, review, opinions, and approval. This can all be handled within pull requests before we've got smart contracts.~~
    - ~~There will be a committee that can approve the pull requests into the master. Ideally consists of members from the community and IF.~~ Once merged into the master, hashing of the proposal will be created and stored in the tangle for verification purposes. This hash must be posted as value transaction against a particular address - (this might not be required if /referendum/{referendumID} can be used to validate it exists on the node and hashes are the same)
    - (optional) Once merged into the master, Github release must be created to release these proposals to Firefly / Nodes (I'm not sure this step might be necessary)
    - (out of this RFC) - node operator can retrieve merged proposals from the master branch and upload it on their node to make it available for referendum automatically (this will have to be optional)
    - Because Firefly checks proposals within the Github repository (true source), we're able to tell the user if the node they're using did not yet upload the proposals
- Voting in Firefly
    - New plugin for Firefly will be developed to support voting
        - ~~Tell the user when there is a new vote ~~
        - ~~Allow them to vote~~
        - ~~Monitor progress of the vote~~
        - ~~See historical votes and their choices~~
        - Mobile support should not be blocking (too much delay)
    - ~~Firefly must support voting per wallet.~~ It'll not support vote per address (each wallet can have multiple addresses)
    - Voting will be a similar process to sending payments, although the user will never lose ownership of their funds. This must be clearly explained.
    The user can change the vote and will be informed if his vote is no longer valid because they have moved some balance. (Users should be informed before they take any action that would influence, change or invalidate the current vote)
    - User will be provided with visualization
        - Other addresses votes - not his/hers (if possible)
        - Number of votes per option
        - progress of the vote over time. (include in a chart?)
    - Firefly should show ongoing proposals, historical and future proposals. For historical proposals, could we store results within Github as nodes would most likely only keep recent proposals? (Yes, history would need to be stored in Github or Tangle explorer, Firefly could have a list saved and link to the GitHub or explorer?)

- Web
    - Build a Website with videos to explain the process
    - ~~Show upcoming/historical/ongoing proposals~~
        - for ongoing (historical too ideally), it can show all the details (i.e., votes per question, votes per day per question, etc.)
    - ~~Build essential integration with Github or redirect to Github~~
    - No need for any user management. It will most likely be more a static website mainly reading information from Github/Nodes **(Future user management IOTA Identity could be a great match here)**
    - ~~Ability to subscribe to news email/Twitter/discord/else?~~
    - ~~Agree to a domain name~~
