**NOTE: THIS DOCUMENT IS VERY MUCH A WORK IN PROGRESS.**

# Overview

This specification summarizes requirements to implement voting tech to support IOTA DAO.

It highlights various use cases and high level technological design together with mock-ups. It can be eventually expanded with detailed technological solution although it might be a good idea to leave them loose so various technological approaches can be prototyped and decided down the track. It's desired to allow more agile process. 

**Voting tech consists of three parts:**
- Hornet Node plug-in. See RFC: https://github.com/WernerderChamp/protocol-rfcs/blob/master/text/0000-chrysalis-referendum/0000-chrysalis-referendum.md
- DAO Website
- Firefly plug-in

Below requirements were established based on various discussion that happen on IOTA's Discord primarily within #governance-discussion and #voting-tech channels. There were also numerous discussion sessions typically on Thursday's 4pm CEST. You can also find  various documented meeting notes in governance [Github repository](https://github.com/iota-community/Community-Governance/tree/main/meetings). 

> **Once this document is finalized**, it should be scrutinized by community and agreed to. This **"can"** essentially become requirement document for voting tech that will need to be build and base specification. Hornet node plug-in is build by Hornet team. Other two part is to be decided. Again, ideally finalizing this document **"can"** speed up this process of finding the right team(s) to build it. You might argue there is a better approach and we are open to discuss it.

## Terms
- [Hornet](https://github.com/gohornet/hornet) - Community driven IOTA Full Node
- [Hornet Plugin](https://github.com/gohornet/hornet/tree/main/plugins) - Extension within Hornet that can be used by various node operators
- [Firefly](https://github.com/iotaledger/firefly) - IOTA's official wallet 
- Proposal - Proposal to be voted on by IOTA's token holders
- Question - Each proposal can consists of multiple question and each of the can have various voting options.
- Voting Option - Option that can be voted on within the question.
- Vote - A casting vote by IOTA token holder.
- [ISCP](https://github.com/iotaledger/wasp) - IOTA Smart Contract Platform

## Key design decisions

It desired for us to get this tech up and running ASAP. We want to avoid building way to complex tech because many parts might change due [ISCP](https://github.com/iotaledger/wasp) introduction and DAO wants to start taking votes ASAP. 

Once ISCP is introduced on the IOTA's Mainnet we should move away from some parts highlighted below system (i.e. Github) and use ISCP instead. Ideally this could be done without a significant impact to the users. 

Lastly, it also very important for community to start making decision ASAP if we are going ahead to BUILD or BURN treasury funds as this can have significant impact on IOTA's future adoption.

### Github
IOTA's Community Governance Github repository is utilized for proposal management prior the submission to nodes and Firefly. It is considered as true source for proposals at this stage. This seems to be sufficient method before ISCP are introduced. It provides enough transparency into the process and allows everyone to get participate.

### Hornet Node plugin
As specified within the RFC, Hornet plugin can be utilized by participating node operators to provide key functions to support voting process. Such as: 
- Available proposals for vote, their status and progress
- Ability to submit IOTA token's holder vote

### Website
Website should be a simple single page application (React, Angular, Vue, etc.) that sources data from above two sources:
- Github
- Hornet Node

Proposal management and voting will be managed within above two and website it's a simple lipstick on the top to present the information in a better fashion and help users to understand the process.

> Potentially website can integrate with Github and allow proposal management within the application (i.e. simplify it for users without Github knowledge). This can help to transition to ISCP as user does not necessary need to get used to new system and it'll be just underlying tech that changes.

## Firefly plugin
Extension for Firefly will support ability to view proposal available for vote, in progress proposals and finished proposal. User is also able to cast their vote, change their vote and see votes by other users.

# Use cases

## Github
### Draft/Request for Comments Proposal
* User creates a new pull request with proposal (DRAFT) specification against DAO repository master branch
* User provides sufficient information within the proposal
* Pull request gets shared within community to provide feedback/comments/etc.
* He/She should make sure to gather much interested before it's passed for submission
### Submit Proposal
* User is comfortable to proceed to submit the proposal for approval and submits pull request
* Approvals are set and pull request awaits all approvals (mixture of IF/community members)
    * This can be automated via GitHub Actions inc. various verification 
* Approvals open pull request and decide to approve/reject based on numerous factors (TBD):
    * number of votes within pull request?
    * number of people monitoring pull request?
    * number of followups?
    * etc.
### Proposal Approval
* Once pull request approved, it'll get merged into master branch under the "proposal" folder. 
    * GitHub Action
    * GitHub Action can also post information about new proposal to #governance-discussion channel on Discord
* Any new proposal within the "proposals" master branch are picked and promoted within Hornet nodes (with voting plugin) & Firefly
    * Upload to nodes can be done manually. Ideally, we can build tool where this can be automated and node operators can enable this if they want to.
### Proposal Termination
* Do we want to handle it, how, when? Amendment to existing proposal with new file "proposal" folder in master branch?

## Firefly
### View Proposals
* User navigates into the voting tab
* User is able to view existing proposals (only those approved, sorted by commencing date).
    * Note, Firefly gets proposals from master branch and node. There might be a scenario where it's not yet uploaded to node.
* By default only see upcoming/in progress proposals
* They can filter and see ended ones
* User can easily identify proposals pending their vote 
* User can easily open proposal on DAO Website to access fully history (i.e. pre-approval)
### Submit Vote
* User selects proposal that's pending their vote
* User votes on a voting option within each question of the proposal
* User has the ability to select wallet they want to vote with
* User can share their vote options online (discord/twitter/etc.) This could potentially include link to their actual "vote" other users can view within DAO Website. 
### Change Vote
* User opens commencing/holding proposals and are able to view their existing vote
* They can change their vote
* They share their vote easily online again
### View Vote Status
* User is able to view status and progress of each proposal and their vote
* Ability to view individual votes by other token holders
* Ability to view overall results 
* Ability to view votes from various nodes allowing them to understand if nodes agree to the count.

## DAO Web
### Home page
* Ideally starts with video what DAO is all about
* Continues with more detailed explanation
* Ends with "cast your vote" / "create a proposal"
### Proposals
* Ability to view all proposals (drafted/submitted/approved/commencing/holding/ended)
* List shows all proposals
* Ideally widget shows currently commencing/holding proposals
* Ability to draft new proposal
    * This could be explanation how to do it in Github
    * Or we build integration on top of the Github - ideal!
* Ability to cast vote and Firefly opens automatically (deep link)
#### View Individual proposal
* Current status and all attributes
* If active/ended
    * User is able to see all votes by individual users with linked to explorer
    * Overall progress of the vote
    * Reported numbers by various nodes
    * Ability to cast vote and Firefly opens automatically
### Subscribe to news
* Email
* Twitter
* Discord

> Anyway, I'm not a big fan of the email database tbh. We can discuss good approach on this. 

### About
* About DAO and it's history
* Members that approves proposals within GitHub (members of iota-community/Community-Governance ?)

## Firefly Voting Logic
![](https://i.imgur.com/Fu9Npwg.png)

## Firefly / DAO Website Mock-ups
TBD

# Questions
- What do we think about mock-ups?
- Is there any sample plugin already developed for Firefly (cound't find one)? 
- Are we all right for Firefly to contact github API's, any concern?
- Wallet.rs supports no value transaction? (to me it seems it does)
- Any concerns for ledger users? as we would have to support that.
- Will it be all right to expand deep links in Firefly to support voting?
- How does Firefly access historical data (i.e. connects to permanode)?
- How long would you rough estimate mobile support? Should be make this solution dependent on it?
- Any concerns opening external links from Firefly (to our website)?
- Any concerns to source data from multiple nodes? (i.e. present voting stats from various nodes)

----------------
# Notes (to initiate above)

## Simple notes / overview for now to agree on for RFC
- Node implementation
    - see other RFC https://github.com/WernerderChamp/protocol-rfcs/blob/master/text/0000-chrysalis-referendum/0000-chrysalis-referendum.md

- Proposals management / retrieval within Firefly
    - Github will be used to manage and store proposals. Webside can provide guide for this. 
    - Pull Requests will be used to manage the flow. This includes submission, review, opinions and approval. This can all be handled within pull request before we've got smart contracts.
    - There will be committee that can approve pull requests into the master. Ideally consists of members from community and IF. Once merged into the master, hashing of the proposal will be created and stored in tangle for verification purposes. This hash must be posted as value transaction against particular address - (this might not be required if /referendum/{referendumID} can be used to validate it exists on the node and hashes are the same)
    - (optional) Once merged into the master, Github release must be created to release these proposals to Firefly / Nodes (I'm not sure this step might be necessary)
    - (out of this RFC) - node operator can retrieve merged proposals from the master branch and upload it on their node to make it available for referedum automatically (this will have to be optional)
    - Because Firefly checks proposals within Github repository (true source) we're able to tell user if node they're using did not yet upload the proposals
- Voting in Firefly
    - New plugin for Firefly will be developed to support voting
        - Tell user when there is a new vote 
        - Allow them to vote
        - Monitor progress of the vote
        - See historical votes and their choices
        - Mobile support should not be blocking (too much delay)
    - Firefly must support voting per wallet. It'll not support voting per address (each wallet can have multiple addresses)
    - Voting will be a similar process to sending payments although user will never loose ownership of their funds. This must be clearly explained.
    - user have the ability to change the vote and will be informed if his vote is no longer valid because they have moved some balance.(Users should be informed before they take any action that would influence, change or invalidate the current vote)
    - User will be provided with visualisation
        - Other addresses votes - not his/hers (if possible)
        - Number of votes per each option
        - progress of the vote over time. (include in a chart?)
    - Firefly should show on-going proposals, historical and future proposals. For historical proposals, could we store results within Github as nodes would most likely only keep recent proposals? (Yes history would need be be stored in Github, or in Tangle explorer, firefly could have a list saved and link to the gitbub or explorer?)

- Web
    - Build website with videos to explain the process
    - Show upcoming/historical/on-going proposals
        - for on going it can show all the details (i.e. votes per question, votes per day per question, etc.)
    - Build basic integration with Github or redirect to Github
    - No need for any user management. It will most likely be more a static website mainly reading information from Github/Nodes **(Future user management IOTA Identity could be a great match here)**
    - Ability to subscribe news email/twitter/discord/else?
    - Agree to domain name
