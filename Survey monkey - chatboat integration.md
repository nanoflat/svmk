# Survey monkey - chatboat integration

**Time spend**: 5h (spread over weekend, I was sick, my wife was sick, our 6m old son caught cold from us and first time in his life was sick, total chaos :D Only cat looked healthy)

**Problem**: 
 We want to collect feedback in a new channel. We already support feedback triggered from websites, applications and e-mail, and now we want to introduce feedback triggered via a Chatbot. For websites and applications we already render a set of questions to gather feedback in-place in order to make the feedback experience as low-friction as possible (it is rendered from our SDK embedded in their website/app). See the “Feedback” button on the right side of our website at Usabilla.com for an example of a small set of questions. We would like to do something similar for Chatbots, so the questions render not via a link to a website with the
questions, but an actual back-and-forth conversation-like experience. For an example consider after a purchase where a customer’s pre-existing chatbot (owned and written by the customer) is already communicating with them details about their purchase, and we add a set of feedback
questions:

Please architect a solution for our customers to use Usabilla for getting feedback via Chatbot. Describe the location, aim and general logic of every component end-to-end. Assume the ChatBots can be used against multiple chat providers, including WhatsApp, Facebook Messenger and SMS. Outline the pros and cons of your design. Decide on intermediate milestones, from internal milestone to first publicly available beta to a full GA release. Estimate a timeline to complete each milestone (assume average developers on any team you’ve worked with). Please describe why you made these choices.


**Answers on Questions :**
![](Survey%20monkey%20-%20chatboat%20integration/Screenshot%202021-02-28%20at%2017.22.45.png)

- - - -
## Process of working on assignment:
1. Review text, formulate and ask questions - 10min
2. Research on chatboats - 15min
3. Plan high-level milestones - 15min
4. Work on architecting solution - 2.5h
5. Refine milestones, provide reasoning and outline positive and negative sides - 20min
6. Polish assignment - 10min
7. Publish and send out.
8. And most important - have fun all the way!


## 2. Research on chatboats.
### *Types of chatbots*
Chatboats can exist on customer site(“live chat”), various messengers and some custom channels (like sms or maybe even voice calls)
Chatboats can be either :
1. 3-rd party SAAS-like solution, tailored specifically to chatboats
2. Big vendor solution with chat-bot integrated (Like Salesforce). BTW will Salesforce buy SurveyMonkey as they did with Slack and Tableu? :D hope so one day
3. Custom-build chat-boats by customer for customer chat systems ( can be fully custom of with some parts of SAAS, like [Dialogflow  |  Google Cloud](https://cloud.google.com/dialogflow)
4. Social-media and messenger chat boats, which itself can be 3rd-party or own build

### *Chatbot Workflows*
Essential part of any chatbot is **workflow**. What are interactions with users? What to say and what to say next depend on input? Are there some cyclic flows? etc.
Example
![](Survey%20monkey%20-%20chatboat%20integration/1*mNXkTH36R7grjyck2NTk5Q.png)

Bots are fun when they are smart and conversational. It means that they should have some well-written scripts
Here are some of the scenarios and state in flow to keep in mind while writing scrips.
* Onboarding (starting chat)
* Questions (“so how do you like your big hawaiian cheezecrust glutten-free pizza”)
* Reaction/acknowledgemen(“that’s great!”)
* Missing or ambiguous inputs (rate our service from 1-5? User answers 7)
* Verification - (“are you are sure that your name is Obi Van?”)
* Resolution - (‘That’s it! You helped us enormously!Thanks for feedback and here you candy”)
* Switching intents - (“wow, we so sorry that you had such horrible service, do you want to talk with our Customer Service right now, sure they can help you out!” And chat transferred to Customer Service Agent and continues outside of chatbot workflow)
* Abandon flow - (“Are you still with us?” After X minutes of wait time)
* Fallback (In case of customer went to a place in worklow where there is no next state and no transition, so instead of error - rollback to last know position and continue. Like “I’m sorry, got distracted by magnetic sunstorm, you are saying that you love action figures, can you share what are you favourite? ” )

### *Existing chatbot solutions*
https://chatfuel.com/
[Chatbot Builder Software | HubSpot](https://www.hubspot.com/products/crm/chatbot-builder?utm_source=motionai_website&utm_medium=referral&utm_campaign=motionai_acquisition)
[ChatBot | AI Chatbot Software for Your Website](https://www.chatbot.com/)
[OBI Bots: Smart chatbots and AI applications | OBI4wan](https://www.obi4wan.com/en/solutions/chatbots-ai-solutions/)

Most of this solutions are no-code or low-code SAASes. So you need to build your workflow in some cloud platform and integrate with channels (web site via simple JS add-on, FB, Whatsup, twitter via APP on social media platform where you add some permissions to bot).
As well this SAAS solutions can integrate with chat provider (Freshchat, Zendesk chat, Salesforce) etc. or trigger some things via IFTTT or Zappier 

Now fun begins! How we can integrate in this chat-bots workflow? We need be able stop chat-bot workflow, and control bot interaction with customer until feedback session is not over. ( we don’t want customers to write feedback workflows in their platforms, we want them to use our!)


Lets pause on research on chat boats and plan high-level what are things we need to do.


## 3.  Plan high-level milestones
So we got the problem, how we will find a solution?
I believe in **maximising learning** while **minimising time to learn** and negative impact(on brand, on customers etc).
I will suggest following process.
High-level Hypotheses Generation -> Idea generation -> Design sprint -> MVP for internal use-> Launch with selected customer (alpha-release) -> Public Beta -> GA release.

Now steps by steps
1. **High-level Hypotheses Generation**. Why we are doing it at all? Will it bring anything to company? Why we are focusing on it and not on 200 other ideas we have on the table. It helps to think on Why, what, for whom and what it will bring. [Experiment Hypothesis Generator](https://apps.conversionista.se/hypothesis-creator/) is my go-to place fo new experiment ideas 
![](Survey%20monkey%20-%20chatboat%20integration/Screenshot%202021-02-27%20at%2017.51.01.png)
2. **Idea generation.** So now we have hypotheses, now can start designing idea, on high level, mostly by product
3. **Design sprint.** We take 1 product, 1 backend, 1 frontend, 1 designer and some customer and lock them for 1 week to build a prototype. Good explanation there [The Design Sprint — GV](https://www.gv.com/sprint/)
4. **MVP for internal use**. With prototype we learned first feedback of customers and get team engaged. Now we want to build WORKING solution for very limited case. Let say we want to build feedback collection solution just for one channel (can be hard-coded thing for now, channel [adapters](https://refactoring.guru/design-patterns/adapter) and configurability comes later) and use it internally for collecting feedback. Example - add feedback option to company Slack-bot (“hey you just closed sprint 10, care to share how it went so we can have focused retrospective?”).  Solution might have limited UI on platform side (feedback constructor)
5. **Launch (darkly) with selected customer (alpha-release)**. Now we polished all obvious issues, we also worked to scale up integrations and developed public API that is secure, scalable and reliable), customer-facing UI is simple yet adhere to company standards. Time to find customer who can benefit from solution the most and as well can tolerate alpha-issues. We have acceptable monitoring. We support simple sequential workflow (we go from A to Z questions)
6. **Public beta.** We had a few iterations of alpha releases  (important to note that release != delploy, release is something marketing, deploy is something engineering). We were able to learn on real customer feedback and prepare for Public beta. Public beta scope contains almost feature-complete UI, robust and load-ready backend, we have full observability and pagers ready to bip for incidents. Selected customer service and sales agents and are trained and ready to onboard customers. We prepped few blog posts and full documentation on main tech and use cases topic. We support 3-5 main channels (live chat, FB and Whatsup). 
7. **GA Release.** We have big marketing event. We support multiple channels and we developed integration guides and APIs for easy onboarding of new channels (**Signal/WeChat** anyone?). We support complex tree-like workflows (so next question is depending on combination of answers, in some conditions we can trigger different action then question) Documentation is as full as it can be, support is trained. Customer-facing UI and internal-facing tooling is complete. We all know that services will handle 10x load.

### 4. Architecture.
Now time to think about solution design. We need to solve couple problems.
1. How we going to integrate with ready chat-bots. 
2. How we going to integrate with current processes of feedback collection
3. How we going to integrate with current feedback-form building

Lets go one by one.
### *Integration with chatbots*
I have no idea, but I have assumptions. First assumption that we cannot integrate easily with livechats(“add this snipped on your website”-kind of integration), because there are megazillion of them. Second assumption that chats and chat-bots might have API. So we can integrate over API (still megazillion but better)
What kind of API it could be? It can be backend-API (like customer opened chat, customer typed something, customer finished workflow, customer reached feedback-stage in workflow). Or it can be frontend-api, like some event is triggered when some interactions happened with chat, example [Web widget · Documentation - Flow.ai](https://flow.ai/docs/integrations/web#trigger-event)

So, we can try to integrate over that API. Lets take Backend Api as example here.

Lets imagine simple interaction with chat bot where user asked for 1 point of feedback
Bot: Hi James! | standart bot flow
User: Wassup? | standart bot flow
Bot: nothing much | -> now bot stops his flow and triggers feedback flow goes on
Bot: Care to share how you like our new gym? | -> feedback flow started, question send
User: ok, not too crowded | -> user responded, that send to bot which sends to feedback flow
Bot: Thats cool! | -> feedback flow ended, control back to bot
Bot: KTHANKSBYE | -> bot continues his flow

So, we have here action to: 
1. Trigger Feedback Session
2. Send message to customer
3. Send message from customer back
4. End Feedback Session

So main entities here are:
* Feedback session
* User
* Chat-bot
* Feedback 
* User input
* Questions/messages to user
* And last one - customer for whom feedback is collected

Lets pause here and look on other problems
### *integrate with current processes of feedback collection*
I bet we already have some way how we collect and store feedback. Assumption is - we operate mostly with feedback forms which has some  snapshot of current edits(we want users to continue editing what they wrote if they went to a tunnel or somehow returned to feedback later), some intermediate state (in case feedback is multi-stage form and allow navigation back and forth) and some final state, maybe with versioning (if we allow to change feedback once submitted).

Now we are adding new, conversational feedback, which is more async than feeling normal form and maybe even with logic(depends on user input). As well, conversational feedback is ambiguous (on question from 1 to 10 rate blah, user can answer not 1 but “one”, or ”zero” or “banana”). We definitely want to store raw data(in some specific to conversational feedback form) as well we want to normalise this data for our standard of “feedback”, so we can leverage other solutions (build by Analyse or Act teams) we have in place instead of heavily customising whole platform for another channel. While some really cool opportunities opens up for both Analyse and Act teams, as now we are able to react on feedback immediately while in conversation with customer (Analyse - filter out and recognise “bananas” of user inputs, sentiments etc. For Act, we can integrate in same chat some external actions, like call to CustomerService, create ticket in their CRM or other)

Summary: We need to store Raw Data, as well as we need to normalise this data for generic Feedback and pipe it to generic feedback collection mechanism. 

So entities here:
* Feedback - chatbot
* Feedback - normalised
* Normaliser (GenericFeedbackConverter)

Lets move on to next challenge
### *integrate with current feedback-form building*

So, we want to make it easy for customers to build conversational feedback collection. It mean that it should look very familiar. I think we can split it on 2 parts.
1. Use classic feedback builder, with quick option to convert or use this form for( or also for) chatbot > then customer will be directed to integration page with some quick documentation. This is good option for beta-releases
2. Another option is to have custom feedback builder, where customer can customise flow with specific interactions, state conditions and actions. This is more for GA and even later releases.

Lets take first option where we convert standard form to chatbot use. So most-likely we need to have convertor and some specific storage as it’s rather ineffective to convert on the fly by request.

So entities here:
* Survey builder
* Survey
* ChatbotSurveyConventer
* ChatbotCustomizedSurvey

We outlined problems, and potential solutions, we have some entities. Now lets have some picture how we can integrate all of it.

High-level diagram
![](Survey%20monkey%20-%20chatboat%20integration/Screenshot%202021-02-28%20at%2013.45.33.png)

Low(er)-level diagram (ok I spend some time on it outside of planned 3h so feel free to not take into account, but I will be super curious on feedback!)
![](Survey%20monkey%20-%20chatboat%20integration/Screenshot%202021-02-28%20at%2016.00.34.png)

Suggested infra: public cloud with managed services (AWS Fanboy here):
1. Gateway - AWS API GW
2. Compute - AWS lambda (not really heavy on compute here, easy to add new stuff)
3. MQ - Amazon Managed Streaming for Apache Kafka or SNS+SQS
4. Storage - Dynamo DB

Managed services will reduce headache of maintenance of infra so we can focus on what we do the best - helping people and companies listen better.
As well, Managed services will allow to scale as we go, so we pay less when we use less, as well performance is less of an issue.

## Estimated cost:
1k chats per minute, 15 per sec. each chat is 5msg back and forth, so 50 incoming and outgoing api calls. ±100-200 events and same lambda invocations and db reads/writes.
Total for just that infra -  6k/month. Adding 30$% on various things like cloudwatch,IAM etc let say 8k / month. Sounds reasonable [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=a450ffcd8b33103bca7f539ddbdbae0de34ff1ba) 
For time to build solution - 1w design sprint + 2w prototype + 4w alpha +8w public beta + 12w GA = 31w work of team of 4 engineers. Or ±2 dev-years. Avg salary in NL for developer is 45k(show me that guy!) [Software Developer Salary in Netherlands | PayScale](https://www.payscale.com/research/NL/Job=Software_Developer/Salary) so it cost ±100k to build
Cost of maintenance - 0.5 dev + 1sales + 1customer support, so around 100k per year
**Total first year ±300k and 100k afterwards**

Lets see how much we can generate with that.
1k chat  per minute = 43M per month. Let say, average survey got 1000 feedbacks so it’s 43k of surveys. Let say each client have 10 surveys, so it’s additional 5k of clients per month. Let’s take middle between business plans, so it’s 50$ per client. So estimated additional revenue = 250k per month. 
Lets take whole first year after GA to reach this point, then we will be break-even on month 14 after GA or 1.5y years after we started working on solution. Estimating 3yeas for product to leave before major redesign or cancelation or godzilla, so we will make  **5M of profit** with this solution. Time to buy SVMK before it goes to the moon :D 

That’s it. Hope you had fun reading my story, I’m sure I did while writing!
