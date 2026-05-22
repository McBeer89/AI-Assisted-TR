# Reference: Threat Detection Engineering Methodology

Bundled articles from Andrew VanVleet's Threat Detection Engineering
series - the methodology this workflow is built on. Read them for the
reasoning behind every rule in the project prompt. Each article below is
separated by a horizontal rule and a source-file marker.


---

<!-- source: 1 Threat Detection Engineering The Series.md -->

# Threat Detection Engineering: The Series

**Author:** VanVleet  
**Published:** January 23, 2024  
**Reading Time:** 2 min read

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

This series is an attempt to step back from the day-to-day efforts and think about the core concepts and principles of Threat Detection and Hunting, then identify a winning strategy for keeping attackers out of our networks. I'll then offer some practical applications to help refine how we do those day-to-day Detection Engineering efforts.

The series will present threat detection as a game of probability and create a visual model for thinking about how we can maximize the chances of reliably winning that game. It will offer a loose mathematical approach to evaluating the value of a given detection and discuss incremental detection costs to help you determine whether a detection (or collection of them) are helping you win the game. It will look at the relative strengths of threat detection and hunting (and how they are different from one another). It will provide an analytic tool to help you identify attack techniques as accurately as possible with your available telemetry.

Hopefully it will be useful in helping you define, refine, or clarify your own thoughts! If you have questions or topics you'd like me to address in future posts, let me know in the comments!

This post will serve as the index to tie together the rest of the articles in the series. I'll categorize the articles as primarily Strategy or Application, though I'll include a bit of both in each article. The application articles will draw heavily on the concepts and terminology established in the strategy ones, so I'd recommend reading those first. Here we go!

## Strategy

1. [Plotting a Winning Threat Detection Strategy: A Visual Model](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441)
2. [Identifying and Classifying Attack Techniques](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595)
3. [The Relative Strengths of Threat (Detection|Hunting)](https://medium.com/@vanvleet/the-relative-strengths-of-threat-detection-hunting-777b03a89d15)
4. [Compound Probability: You Don't Need 100% Coverage to Win](https://medium.com/@vanvleet/compound-probability-you-dont-need-100-coverage-to-win-a2e650da21a4)
5. [TTPI's: Extending the Classic Model](https://medium.com/@vanvleet/ttpis-extending-the-classic-model-058c572b76f3)

## Application

1. [Threat Detection Cost vs. Coverage](https://medium.com/@vanvleet/the-threat-detection-balancing-act-coverage-vs-cost-cdb71d21412f)
2. [Improving Threat Identification with Detection Data Models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051)
3. [DDM Use Case: What ATT&CK Gets Wrong about Process Injection](https://medium.com/@vanvleet/ddm-use-case-what-att-ck-gets-wrong-about-process-injection-7c15b6764bfe)
4. [Mistaken Identification: When an Attack Technique isn't a Technique](https://medium.com/me/stats/post/8cd9dae6e390)
5. [Creating Resilient Detections](https://medium.com/@vanvleet/creating-resilient-detections-62f9eb5318eb)
6. [Technique Analysis and Modeling](https://medium.com/@vanvleet/technique-analysis-and-modeling-ffef1f0a595a)
7. [Technique Analysis and Modeling (Podcast)](https://www.youtube.com/watch?v=5DAQkvOyqME)
8. [Technique Research Reports: Capturing and Sharing Threat Research](https://medium.com/@vanvleet/technique-research-reports-capturing-and-sharing-threat-research-003c80ac9a4d)

---

**Tags:** Threat Detection, Threat Hunting, Detection Engineering, Information Security

**About the Author:**  
VanVleet - A Cyber Security professional with just shy of 20 years experience in the public and private sectors. I have a particular passion for Threat Detection and Hunt.

---

<!-- source: A1 The Threat Detection Balancing Act Coverage vs Cost.md -->

# The Threat Detection Balancing Act: Coverage vs Cost

**Author:** VanVleet  
**Published:** January 23, 2024  
**Reading Time:** 7 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62), if you haven't read the earlier articles, you might want to do that first.

As discussed in my [article on strategy](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441), threat detection is like a game of probability: we try to build preventative mechanisms and detections that cover enough of the attack surface that it's unlikely an attacker will find a path through our network without triggering one of our alarms and alerting us to their presence. In this post, I'm going to explore some of the practical realities and constraints involved in maximizing our attack surface coverage and the implications for threat detection efforts.

At first blush, the best approach seems to be to deploy as many detections as possible, maximizing our coverage through overwhelming numbers. This approach is facilitated by the many available collections of public or commercially-provided detection content. For example, Elastic Security [advertises](https://www.elastic.co/blog/elastic-detection-rules-open-visibility-data-quality) 800 SIEM rules plus 380 endpoint rules in their version 8.8 release. [Anvilogic](https://www.anvilogic.com/) and [SOCPrime](https://socprime.com/) and claim thousands of ready-to-go rules. Sentinel offers [50+ rules](https://github.com/Azure/Azure-Sentinel/tree/master/Detections/AuditLogs) for protecting Azure AD alone. It is relatively easy to deploy detections for many thousands of use cases, especially when you're trying to protect a wide range of enterprise systems: endpoints running any of the top 3 operating systems, network devices, orchestration platforms, cloud platforms, Active Directory, and so on.

## Detections: A Limited Resource

While an automation platform might support a huge number of detections, **in practice there are constraints on the number of detections that a given organization can maintain.** Every deployed detection is going to generate a certain number of false positive (FP) alerts. Some detections might identify rare or easily distinguished malicious activity and almost never generate FPs, while others may be trying to spot malicious use of common resources and generate frequent FPs. But EVERY deployed detection will generate a least a few.

For the sake of discussion, let's assume that on average a given body of detections will generate only one FP per month per alert. At that rate, a body of 500 alerts will generate 500 FP's each month. (For this calculation, we won't count true positive alerts: if you have more true positive alerts than resources to handle them, your problems aren't with Threat Detection!) Now, let's assume that our incident responders can investigate those false positives at a rate of 1 every 30 minutes, all day every day without burning out and quitting. That means one responder can handle 16 alerts per day, and 320 alerts per month. So, our body of 500 alerts will require a team of 2 responders just to manage the false positives (to say nothing of handling true positives!). This simplistic example scales linearly: 1000 deployed detections will need a team of 4 responders, 1500 will need 6, etc.

In addition to the cost in incident response time, each detection also requires care and feeding from the Detection Engineering team. For every alert deployed, there is some amount of time that must be spent on maintenance: creating filters for recurring false positives, updating and fixing queries that break due to things like log source changes. There should also be a validation process to ensure that all of those hundreds or thousands of detections are still working as designed, otherwise your real detection capability will atrophy over time.

Each detection also carries costs in automation platform resources: the processing and memory required to run a detection query every X minutes, 365 days a year, in order to alert as quickly as possible when the malicious activity it's looking for actually happens. We're not going to explore those costs here, since they vary drastically from configuration to configuration.

**All of this means that for any given organization, there is an upper ceiling on the number of detections that can be deployed and maintained.** They are a limited resource. While our example is simplistic, [industry surveys](https://swimlane.com/blog/top-soc-analyst-challenges/) suggest the assumptions we used to inform it probably aren't too far off. I'd guess from my own experience that for most organizations, the limit is likely between 1500–4000 deployed detections.

## Incremental Coverage vs. Incremental Cost

The cost of one new detection, in terms of response and investigation time, care and feeding, and query platform resources, can be considered the detection's **incremental cost**.

In the [strategy article](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441), I explored various detection patterns and offered a model to think through how much total attack surface a given detection covers. I'm going to use the term **incremental coverage** to describe how much additional attack surface coverage one detection provides. Here are a few details from that previous post that you'll need to know now: we're going to suppose that there are only 500 procedures in our total attack surface. (Mitre's ATT&CK matrix v13 defines 196 attack techniques and 411 sub-techniques, so our theoretical is definitely simpler than real life, but it makes the math easier without compromising the argument. I'd guess there are closer to 1000 distinct procedures in the real world.) Using this estimate, we can do some rough incremental coverage calculations:

* A detection that comprehensively covers one procedure would provide incremental coverage of about 1/500th.
* A 'tuple' detection (one that looks for a combination of two or more procedures) yields incremental coverage of 1/124,750th (there are 124,750 possible 2-tuples of 500 procedures).
* A 'tangential' detection (one that looks for an attacker-controlled element, like a command line) adds 1/500,000th of incremental coverage at best (an attacker can modify the attack to evade the detection endlessly, but we're going to cap the possible modifications at 1000 per procedure so we can assign it an incremental coverage value).

When we compare incremental cost to incremental coverage, it becomes evident that some detections probably aren't worth it. For example, let's imagine our organization's maximum detection limit is 6000 (I'm high-balling the estimate to emphasize the point) and we onboard 3000 off-the-shelf detections from our favorite public or commercial repository. If those 3000 are all entirely 'tangential' detections, then we have improved our attack surface coverage by about 3000/500000ths, or 3/500ths. But those detections have consumed 1/2 of our total available detection capacity! That leaves only 1/2 our capacity to cover the other 497/500ths of the attack surface! Clearly, this is a losing strategy. Under this scenario, there's a high probability that an attacker can find a path through our network that we don't have covered.

## From Theoretical to Reality

We've used a lot of estimates to create our model and simplify the math. The real numbers are going to vary per organization. Your actual false positive rate, and therefore the incremental cost per detection, will depend on your environmental noise, quality of your telemetry, and the skill of your detection engineering team. The number of alerts that your incident response team can handle depends on their tooling, automation, skill, and experience. But the real-world attack surface is also much larger than our theoretical case, so the constraints and trade-offs highlighted here are definitely still in force in real life, if not more so. **The attack surface is large enough (especially considering all the different platforms a typical organization has to defend!) and the real-world limit on detections low enough that it is important to maximize the incremental coverage and minimize the incremental cost of each detection we deploy.** A big body of detections with low incremental coverage won't help us win the game of probability. We're much better off with a smaller body of detections with high incremental coverage. **From a theoretical standpoint, 3 detections that each comprehensively cover 1 procedure provide the same attack surface coverage (3/500ths) as those 3000 tangential detections, and at significantly lower incremental cost.**

But reality is more nuanced than our theoretical model, naturally. Those 3000 'tangential' off-the-shelf detections, assuming they are well distributed amongst the various techniques, might actually provide very good detection coverage against low-skilled attackers like 'script kiddies' or threat groups who mostly pursue low-hanging fruit. If the attackers targeting your organization are all likely to use off-the-shelf attack tools and techniques without obfuscation or modification, then those 3000 off-the-shelf detections could be exactly the right strategy. On the other hand, they'll be next to worthless against an attacker who is careful NOT to use known command line parameters and obfuscates their tooling. State-sponsored attackers and skilled criminal actors will have no difficulty evading those 3000 'tangential' detections, **leaving your organization *feeling* protected but effectively defenseless.** In that case, you're much better off with those 3 comprehensive detections.

## Summary

While it might appear that detection capacity is infinite and therefore any additional detection is worth deploying, in reality detections are a limited resource. Detection Engineers should maximize the incremental coverage and minimize the increment cost of each detection they deploy to ensure that they cover as much of the total attack surface as comprehensively as possible. Large collections of detections that offer little incremental coverage may give an organization the illusion of protection, but leave them effectively defenseless.

## One Final Thought: An Ounce of Prevention….

While not the main topic of this article, this discussion highlights the value of policies and mechanisms that prevent attack techniques outright. For example, if you're working to defend a Kubernetes cluster and you can put a policy in place to completely prevent the creation privileged containers, then you have reduced the total attack surface with very little incremental cost. Same for features like Windows Credential Guard to prevent credential-theft techniques targeting LSASS. Those are big wins! Preventative measures are the only thing that can improve the fundamentally disproportionate relationship between attack surface (huge) and detection capacity (too small). **While we don't often think of prevention as part of Threat Detection, it ought to be the first thing a Detection Engineer considers when evaluating how to deal with an attack technique.** (Note, you shouldn't disable a detection because you are preventing the attack! There is value in knowing something was attempted, even if it was prevented. But by preventing something outright, you significantly reduce the incremental cost of that detection: it shouldn't fire unless something goes REALLY wrong.)

## Thoughts?

I'd love to hear if your experience is consistent with or different from my own. If you have any thoughts to add, post a comment or let me know on [Twitter](https://twitter.com/_vanvleet)!

*Originally published at [https://www.linkedin.com](https://www.linkedin.com/pulse/threat-detection-balancing-act-coverage-vs-cost-andrew-vanvleet).*

---

**Tags:** Threat Detection, Detection Engineering, Information Security

---

<!-- source: A2 Improving Threat Identification with Detection Modeling.md -->

# Improving Threat Identification with Detection Modeling

**Author:** VanVleet  
**Published:** February 26, 2024  
**Reading Time:** 13 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this post, I'll present a simple approach to detection modeling and demonstrate how a Detection Data Model (DDM) they can be used as an analytic technique to help with the task of identification.

In this article I'm building on previous topics, so you'll find it easier to follow along if you've already read my articles on [Identifying and Classifying Techniques](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595) and overall [Threat Detection strategy.](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441)

## Structured Analytic Techniques: What and Why?

Let's start with a story.

In March 2023, CVE-2023–23397 was released. The vulnerability involved sending an Outlook calendar invite with the *PidLidReminderFileParameter* set to an attacker-hosted external file. This caused Windows to attempt to authenticate to the external share with the NTLM password hash, exposing the user's hash. Crowdstrike quickly responded by publishing a [hunting query](https://www.reddit.com/r/crowdstrike/comments/11sda83/situational_awareness_hunting_microsoft_outlook/) to find the activity.

Except… the published query didn't actually detect the attack technique. The query was looking for outlook.exe making outbound SMB connections, but it was the SYSTEM process (PID 4) that makes the outbound SMB connection. The detection had excellent theoretical classification (outlook.exe really shouldn't be making outbound SMB connections) but achieved 0% identification, resulting in a 0% probability of finding the technique.

So what went wrong? Crowdstrike's analyst didn't fully understand how the technique worked (none of us did because it was new!) and he made a reasonable but incorrect assumption. Now, please don't misunderstand. The purpose of this story is not to insult the excellent work that Crowdstrike — and the particular analyst who made the post — does to help us all keep our networks safe. **The purpose of this story is to demonstrate that even the most experienced detection engineers can make identification mistakes that sabotage their threat detection efforts.** These mistakes happen because attack techniques are abusing complex technical systems. When dealing with complex information, the human mind is excellent at making assumptions to fill in gaps.

In the world of intelligence analysis (where I started my career), analysts are taught to use [structured analytic techniques](https://www.cia.gov/static/Tradecraft-Primer-apr09.pdf) to make their analysis resistant to mental mistakes. These techniques help reveal where our information or understanding is incomplete and we're making assumptions to fill in gaps. Once we can identify our knowledge gaps, we know what details we need to find for a more accurate understanding. Even if we can't fill those gaps, we can at least be deliberate and explicit about the assumptions behind our thinking.

Returning to the world of detection engineering, identification is a difficult task fraught with possible mistakes about complex technical details. It is further complicated by limited visibility into those systems. In order to help us accurately understand an attack technique and determine the best way to identify it, we can employ a structured analytic technique: a detection data model.

## Detection Data Models

**A detection data model (DDM) is an analytic tool that facilitates the process of modeling an attack technique, i.e. mapping out the operations required to implement an attack technique and uncovering any gaps in your understanding of that technique.** It also maps in potential telemetry sources to assist with finding the best available approach to identifying an attack technique. To borrow a term from Jared Atkinson, the DDM helps you [find the base condition(s)](https://posts.specterops.io/thoughts-on-detection-3c5cab66f511) for an attack technique (definitely recommend reading his post!)

I'm honestly not sure who first came up with the term "detection data model," but there is a great deal of prior work on mapping out attack techniques. Jose Luis Rodriguez and Roberto Rodriguez have [done](https://posts.specterops.io/defining-attack-data-sources-part-i-4c39e581454f) a [great](https://www.youtube.com/watch?v=eM0c_Gil-38) [deal](https://www.youtube.com/watch?v=QCDBjFJ_C3g) with detection models in the [OSSEM project](https://ossemproject.com/dm/intro.html). Jared Atkinson has an excellent [series](https://posts.specterops.io/understanding-the-function-call-stack-f08b5341efa4) that demonstrates how to map Windows techniques via '[operation graphs](https://posts.specterops.io/on-detection-tactical-to-functional-a3a0a5c4d566)' and previously worked on mapping via '[capability abstractions](https://posts.specterops.io/capability-abstraction-fbeaeeb26384).' I have borrowed elements from each of these modeling approaches in crafting my version of a DDM.

My goal was to create a detection model and process that would be lightweight and flexible enough to map any attack technique on any platform (various OSes, network, cloud, etc). **Keep in mind that this is an analytic technique: there is no single correct model.**

The purpose of a detection data model is to:

1. Map out the **specific**, **essential**, **immutable**, and theoretically **observable** elements of an attack technique to ensure you have a detailed and complete understanding of how it works. (And to make sure you're "[playing with a full deck](https://posts.specterops.io/thoughts-on-detection-3c5cab66f511)!")
2. Distinguish between unique procedures that implement the same technique.
3. Find potential telemetry to identify the technique with the highest possible confidence.

Here is an example of an incomplete DDM for the attack technique identified in CVE-2023–23397. (I'll talk later about why I left it incomplete.) Following the style of Jared's operation graphs, I use a circle to represent each 'operation' that takes place. Arrows indicate the flow of operations, and labels and tags are used to include potential telemetry sources and important details like processes, APIs, filenames, etc.

<!-- Image: DDM diagram for CVE-2023-23397 showing operations with a question mark indicating incomplete understanding -->

## Creating a DDM, Step by Step

Now that we've talked about why to use a DDM and given an initial example, let's walk through the process of creating a DDM. I'm going to use the technique "[Create or Modify System Process: Windows Service](https://attack.mitre.org/techniques/T1543/003/)" (T1543.003) as my example here. I'll specifically focus on creating a remote service, so I can demonstrate how to map out a procedure that involves two machines. I use the [Arrows](https://arrows.app/) app in this demo, but any drawing application works.

> Please note: I am not going to go into how to deep dive into a technique here because my focus is on mapping the technique, not analyzing it. Trying to do both would make a VERY long post. Jared's posts cited above (and more articles [here](https://posts.specterops.io/understanding-the-function-call-stack-f08b5341efa4) and [here](https://specterops.io/wp-content/uploads/sites/3/2022/06/RPC_for_Detection_Engineers.pdf)) walk through a lot of that process for various techniques. They would be an excellent place to start if you need to learn those skills.

The first step is to map out your understanding of the technique as best you can. Try to define operations that are as granular and specific as possible, but it's not critical to get it right initially. We'll iteratively work to expand operations. A few principles to keep in mind:

* Each operation will be a circle and should use an "Action Object" pattern for naming.
* Use arrows to indicate the progression from operation to operation
* If the procedure involves two machines, make operations on the source machine a green circle and the target machine blue.

So, here is my first attempt at mapping out the operations in creating a remote service, just based on some initial googling.

<!-- Image: Initial DDM showing "Open SCM" (green) → "Call CreateServiceW" (green) → "magic happens" → "Create Registry Key" (blue) -->

First you have to open a handle to the Service Control Manager on the target machine, and then you call CreateServiceW with the details of the service you want created. Then magic happens, and on the remote machine a registry key is created.

Then you ask some questions about each operation you've mapped:

* Do I understand what's happening here? Do I know what processes, APIs, network connections, and securable objects are involved in each step? Is it clear how one operation causes or is followed by the next operation?
* Is this a specific, granular operation or a summary of multiple other operations? Is this the [right level of abstraction](https://posts.specterops.io/capability-abstraction-fbeaeeb26384), or do I need to delve lower?
* Is this operation essential to accomplishing the technique? If an operation is optional (like unmapping the existing section in Process Hollowing) then it should **not** be included on the DDM. This will make more sense when we talk about how to use it.

For the Open SCM operation, I have to admit that I know almost nothing of what's involved there, so that's the first place to go deeper. A review of the documentation for [CreateServiceW](https://learn.microsoft.com/en-us/windows/win32/api/winsvc/nf-winsvc-createservicew) refers me to the API [OpenSCManager](https://learn.microsoft.com/en-us/windows/win32/api/winsvc/nf-winsvc-openscmanagera) for this step. I can see that "opening SCM" means I need to call this API, so I'm going to rename this step "Call OpenSCManager." Other than that, this operation seems pretty specific and straight-forward: I call it, provide the remote machine I want, and I get back a handle that I can use with CreateService. So, I'm happy with where this is at and feel comfortable that I understand this operation.

The next operation is "Call CreateServiceW." The relationship between this operation and the one that follows it is not clear: my best description of the next step is "magic happens and a remote key is created." Clearly there is a lot I don't understand here. For example, how is the remote machine being informed that it needs to create a registry key? What process is doing that? As mentioned, I'm going to skip over the technical analysis right now, but at this point you'd go and do that research and discover that CreateServiceW calls a Remote Procedure Call (RPC) named "[RCreateServiceW](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-scmr/6a8ca926-9477-4dd4-b766-692fab07227e)" on the target machine. This RPC service is hosted in the Windows binary services.exe. Let's add that to our DDM. We use a downward arrow to indicate that the operation below is part of the implementation of the operation above, not a new step. In other words, it's a lower layer of abstraction. For "Receive RPC" we're using an upward arrow to show that the "Create Registry Key" operation is completed by the RPC code on the target machine (you can see this by looking at the implementation of RCreateServiceW in your favorite disassembler).

<!-- Image: Refined DDM showing "Call OpenSCManager" → "Call API" (with tag: API: CreateServiceW, Process: any) → downward arrow to "Call RPC" (RPC: RCreateServiceW) → "Receive RPC" (Process: services.exe) → upward arrow to "Create Registry Key" -->

We want to keep the operations granular but general, so at this point I'm also going to change the "Call CreateServiceW" operation to "Call API" and added a tag to note the specific API that's being called. I've also added in a tag for the process responsible for each operation. You'll use tags for any specific details, like file names, registry keys, network ports or protocols, API calls, etc. Make sure you only include the essential and immutable details: if you have an operation of "Write File" and the attacker can choose the filename, tag it "File: any" or don't tag it at all.

Now we go through the iterative process and ask the same questions of our new operations. In this case, I still don't know the specifics of how this data is transiting the network. "Remote Procedure Call" is a known entity, yes, but each RPC can use different network transports. This tells me that I need to delve deeper to understand what's happening during this operation. Skipping the technical details (but you can read about them [here](https://specterops.io/wp-content/uploads/sites/3/2022/06/RPC_for_Detection_Engineers.pdf)), we learn that the [Service Control Manager Remote Protocol [MS-SCMR]](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-scmr/705b624a-13de-43cc-b8a2-99573da3635f) has [two possible network transports](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-scmr/4c8b7701-b043-400c-9350-dc29cfaa5e7a): SMB using the named piped "\PIPE\svcctl" or TCP. Let's add these to our map.

<!-- Image: Further refined DDM showing "Call RPC" branching downward to two options: "Connect SMB" (Pipe: \PIPE\svcctl, Process: ?) and "Connect TCP" (Port: 49152-65535, Process: ?) -->

I don't actually know which process would make these TCP or SMB connections, so I'm tagging them with "Process: ?" Again, I used a downward arrow from "Call RPC" to the new operations, because they are a lower abstraction layer: they are how the "Call RPC" operation is implemented.

Going through the iterative process again, I'm now feeling like I have a pretty solid understanding of how this technique works. Each operation is granular and specific, in that it doesn't appear to summarize multiple operations. At this point, I feel like I have a solid understanding of the operations required to implement this technique.

Now it's time to add in the telemetry for each operation. If you're not certain that a source will record the operation, add it and then you can test it out later. In this use case, I know that [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) event 18 records connections to named pipe, so that might be an option. I also could potentially see network traffic in firewall logs or with network sensors. Creating a new service can generate events 4697 and 7045, and analysis of the code for RCreateServiceW (in our "Receive RPC" operation) shows that they are generated there, so we'll add those to that operation. And Sysmon 12 records the creation and deletion of registry keys, so I'll add that. Finally, if I can [set a SACL](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/registry-global-object-access-auditing) on the [SCM database in the registry](https://learn.microsoft.com/en-us/windows/win32/services/database-of-installed-services), event 4663 would notify me of changes. You should also add in any telemetry your EDR or other security solutions might provide. Here's our DDM with telemetry sources:

<!-- Image: Complete DDM with telemetry annotations on each operation showing available log sources like Sysmon 18, events 4697/7045, Sysmon 12, event 4663, etc. -->

The final task is to ask yourself "is there another way to do any of these operations?" For example, is there another way to open a handle to a remote SCM besides calling OpenSCManager? Can we call an RPC on the target machine directly instead of using CreateService? Could we create a registry key on the remote system without involving SCM at all?

If these questions lead you to other possible paths or operations, add them to your DDM.

## Using a DDM

Now that we've completed the process and have our DDM, it's time to use it. We already accomplished purpose #1: mapping out the operations to ensure we have a detailed and complete understanding of how the technique works. Now we accomplish purposes #2 and #3.

### Distinguish between Unique Procedures

It's time to use our DDM to determine if it includes distinct procedures. I'm a believer in Jared Atkinson's definition of a procedure (see "[What is a Procedure](https://posts.specterops.io/on-detection-tactical-to-function-810c14798f63)" for details): a procedure represents a unique series of operations that accomplish the technique. This is part of why it's important that a DDM only includes **essential** operations! Including optional operations makes it harder to distinguish procedures.

If you have two very different paths through your DDM, you probably have two different procedures. For example, if we determine that an attacker can create a service remotely by connecting directly to the remote registry, our DDM looks like this:

<!-- Image: DDM showing two separate paths - one through SCM and one directly through "Create Remote Registry Key" to "Create Registry Key" -->

It's visually very obvious that the path from "Create Remote Registry Key" to "Create Registry Key" is entirely separate from any of the other operations. That tells us we have two unique procedures: creating a service via SCM, and creating a service via the registry. (Now, if you've been paying attention, you're probably thinking that new "Create Remote Registry Key" operation above is very likely a summary of other operations, and you're correct. We need to do more iterations until we understand what's really happening there, but I'll leave that as an exercise for the reader.)

Generalizing this, any time we have 2 or more distinct paths through a technique, we're probably looking at 2 procedures. The following operation map has two distinct procedures: one via B and one via C.

<!-- Image: Simple diagram showing A branching to B and C, both leading to D - illustrating two distinct procedures -->

At this point, you may want to fork the DDM into two — one for each procedure identified — and then dig deeper into the new procedure until you really understand it. On the other hand, if your telemetry allows you identify both procedures with a single source, you might want to keep them in a single DDM. More on telemetry next….

### Choose the Telemetry Source with the Most Accurate Identification

Decide which data source offers the best bet at accurate identification of the technique. That will often be the source closest to the end of the operation chain. With our example technique, it seems like our best source would be Sysmon 12 or a SACL on the SCM database in the registry. That would allow us to identify new services with near 100% accuracy, even if an attacker created a registry key directly without using RPC. However, if neither of those is an option in your environment, the next best choice would be events 4697 or 7045. At that point, we're best forking our DDM into two different procedures. We would have very good identification for one of procedures (the one using SCM), but we would also know we have an identification gap on the other procedure (using the registry). This is often the case with threat detection, so at this point we'll just document the gap and move on. The goal is to [cover as many dots as possible](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441), but we know we can't cover **all** of them. At some later point, if we gain the ability to set SACLs or deploy Sysmon, we can circle back to this procedure.

## Wrapping it Up

I have explained why we need DDMs and how to make them. Now I'm going to return back to where I started, with the DDM for CVE-2023–23397. Remember that operation that was simply a question mark? I left that there to make a point. Sometimes, we may find that a DDM has already given us the information we need to identify the procedure with high accuracy. For example, maybe our email security solution allows us to flag every incoming message that contains "*PidLidReminderFileParameter"* and we know from our research that the field is absolutely necessary for the attack technique to work. At that point, you might decide to call the DDM **good enough** for your purposes. Remember this is a tool, not a end to itself.

However, if you haven't already identified an excellent source for accurate identification, that question mark might represent the perfect identification opportunity that you just don't know about yet. Imagine if in our example we stopped at the first layer and our DDM looked like this:

<!-- Image: Simple DDM showing only top-level operations with question marks for unknown details -->

We would be entirely ignorant of the opportunity provided by events 4697 and 7045 and might conclude that without Sysmon or a SACL, we can't identify this technique at all and put it on the shelf. Or, in the case of CVE-2023–23397, perhaps we cobble together a low fidelity detection based on SMB and WebDav traffic because it's the best we have available. It may be, and it may not be… it all depends on what's behind the question mark. **One important benefit of a DDM is to help us realize that there IS a question mark in our understanding of a technique, so we can make an informed decision about what our best option is.**

## Thoughts?

Hopefully this article has provided you with a tool that can help with your next attempt to identify an attack technique in your environment! If you have any questions or thoughts to add, post a comment below.

---

**Tags:** Threat Detection, Detection Engineering, Detection Data Model, Detection Modelling

---

<!-- source: A3 DDM Use Case What ATT&CK Gets Wrong about Process Injection.md -->

# DDM Use Case: What ATT&CK Gets Wrong about Process Injection

**Author:** VanVleet  
**Published:** March 7, 2024  
**Reading Time:** 13 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this post, I'll demonstrate the value of detection data models (DDMs) using [Process Injection (T1055)](https://attack.mitre.org/techniques/T1055/) as a use case. In the process of building the DDMs, it'll become pretty clear that Mitre's ATT&CK framework gets some sub-techniques of process injection wrong, and how a DDM makes it easy to find a better way to define them.

If you haven't already read my post on [Detection Data Models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051), you might want to start there so it's easier to follow along.

## Process Injection — Mapping the Sub-Techniques

Process Injection is one of my favorite attack techniques, because it's fun to dive into the internal details of how an attacker can get code to execute in another process. To get us started, let's quickly review the various sub-techniques defined by ATT&CK and make a DDM for each of them. We're going to focus only on Windows techniques in this post .This is going to be a simplified, quick-and-dirty DDM for the sake of being succinct.

### T1055.003 Thread Execution Hijacking

This technique is defined by executing malicious code by hijacking one of the process' existing threads. First off, an attacker needs a suspended target process. (You could, in theory, try hot patching an unsuspended process by just modifying the instruction pointer, but I doubt it'll work reliably.) This can be done by creating a new process or suspending an existing one. Then you need memory in the target process to write to; this can be done by allocating it directly or by mapping in a new section. Next you write your payload to the new memory space. Finally, hijack your chosen thread. There are a couple of ways to do that: if it's a new thread that hasn't started execution, you can write a JMP operation at the existing entry point (in what was the .text section), or you can modify the register that will be loaded into the instruction pointer when the process resumes (RCX or EAX). Once this is done, you resume the thread to start execution.

There's one more alternative procedure here, where we simply overwrite the existing code at the existing entry point. This doesn't require allocating new memory, just changing the memory protection state to make it writable.

All told, this sub-technique's DDM looks like this:

<!-- Image: DDM diagram for T1055.003 Thread Execution Hijacking showing operation flow from "Get Target Process" through "Allocate/Map Memory" or "Change Memory Protection", "Write Memory", "Hijack Thread" (with options for JMP or Modify Register), and "Resume Thread" -->

### T1055.004 Asynchronous Procedure Call

This sub-technique uses an Asynchronous Procedure Call (APC) to gain execution for the malicious code. An APC is a function that runs in the context of another thread. You run it by queuing it up for the target thread, and then when the thread enters an 'alertable' state, it'll see that the APC is queued for it and execute it. An attacker has to get their malicious payload into the target process first, and they can use any of the methods we discussed for thread hijacking. Then, they call "QueueUserAPC" and provide the address of their payload and it's off to the races. There are a couple variations on the approach that hinge on when the target thread becomes "alertable" and runs the payload, but the DDM looks the same:

<!-- Image: DDM diagram for T1055.004 Asynchronous Procedure Call showing operation flow with memory allocation/mapping, writing payload, and QueueUserAPC call -->

### T1055.005 Thread Local Storage

This sub-technique is very similar to the first two, except that the attacker uses Thread Local Storage (TLS) to get the malicious payload executing in the target process. As with APCs, the attacker can use any method to get the malicious payload into the target process. I'm not honestly sure if you MUST suspend the target process for this one, but we'll say you do for the sake of a stable solution (you don't want your memory space changing underneath you!). To execute the payload, an attacker will modify the TLS data structures in the binary's PE header to point to the address of their payload. There are a few variations on how to do it, based on whether the target process already has a TLS index. If there's already an index, you just add your payload's address to the end of the array. If there isn't one, you'll have to create the entire structure somewhere in memory, then update the header's TLS data directory to point to it. Every time a new thread is created, the Windows loader will run all TLS functions BEFORE the thread is directed to it's intended entry point. So, once the new TLS function is installed, you just wait for a thread to come along to execute it. The payload needs to be designed for this particular execution case, but that's outside the scope of our brief discussion. This gives us a DDM like this:

<!-- Image: DDM diagram for T1055.005 Thread Local Storage showing memory allocation, payload writing, and TLS modification operations -->

### T1055.015 ListPlanting

This technique is a little unique, because it can only be used to target a process that has a 'list-view control,' which is a Windows UI element that shows the user a drop-down list where they select an item. The sub-technique abuses this control's ability to execute a custom sort function by providing a malicious function instead. An attacker can target an existing process but is most likely to launch their own sacrificial process that they know meets the requirements. Like all sub-techniques previously discussed, they can get their payload into the target process' memory with any already noted technique, or there is a way to use SendMessage with the LVM\_SETITEMPOSITION and LVM\_GETITEMPOSITION flags to write the payload a painful 2 bytes at a time. The payload is executed by a call to PostMessage with the message "LVM\_SORTITEMS," causing the malicious "sort" function to execute in the target process. The DDM looks like this:

<!-- Image: DDM diagram for T1055.015 ListPlanting showing memory operations and PostMessage with LVM_SORTITEMS -->

## And Pause…..

Thus far all of our DDMs have looked very similar. Let's combine them and take a look.

<!-- Image: Combined DDM showing all four sub-techniques (Thread Execution Hijacking, APC, TLS, ListPlanting) with their shared and distinct operation paths -->

Looking at the model of all of these sub-techniques, it's very clear that they are all similar yet distinct operation chains that accomplish the same attack technique. The major distinguishing factor is that they all have a distinct approach to executing the malicious payload. This is what sub-techniques should look like!

However, the astute reader will have noticed that I have not moved sequentially through the list of sub-techniques. That's because I've started with the sub-techniques that ATT&CK got **right**. We are unfortunately at the end of that list. As we start looking at the next sub-techniques, we'll start seeing what ATT&CK gets **wrong**.

### What Defines a Sub-Technique?

Before we move on, let's briefly discuss what makes a good sub-technique. I assert that **a sub-technique should be a unique implementation of a technique. It should not be possible to execute multiple sub-techniques of the same technique simultaneously.** Otherwise, you're really just giving two names to a single technique. Putting this into the DDM context, if they both traverse the same operational path, they are the same sub-technique. (There's a whole other conversation to be had about what distinguishes a procedure from a sub-technique, but we'll save that for later.)

In the context of this use case, that means that I should not be able to inject a single malicious payload into a single target process and have it qualify as 2 or 3 or 4 different Process Injection sub-techniques.

### T1055.002 PE Injection

This sub-technique is a great place to start on the "wrong" list. ATT&CK describes this sub-technique thus: "PE injection is commonly performed by copying code (perhaps without a file on disk) into the virtual address space of the target process before invoking it via a new thread." The definition and title seem at odds: one seems to be focused on creating a new thread to execute the code, while the other focuses on the payload. So, is the sub-technique distinguished by the payload you're writing (a portable executable [PE] file), or by the way you execute it?

If the former, it is impossible to distinguish this sub-technique from any other sub-technique on our DDM because it comes down to a distinction in the "Write memory" operation. You have three options for what to write: shellcode, a PE, or a DLL (which IS also a PE, but we'll list it separately now so we can deal with T1055.001 next).

<!-- Image: DDM showing "Write Memory" operation with three payload options: shellcode, PE, or DLL -->

We can take **any** path through the model, so long as the payload we write is in PE format. If the sub-technique is defined by the payload, **it is impossible for it to happen independent of another sub-technique.** You still have to execute the payload and thereby use another sub-technique. In my opinion, that's a strong indication that it's not a sub-technique at all.

On the other hand, if the sub-technique is defined by using a new thread to execute the code, regardless of the format of the payload, then it's poorly named but does fill a gap in our existing model. The DDM would look like this:

<!-- Image: DDM for "New Thread Injection" showing memory allocation, payload writing, CreateThread operation -->

This looks like it really belongs! It's a unique operation path that focuses on a distinct method for executing the malicious payload. In fact, if you know process injection well, you were probably already wondering where the "New Thread" sub-technique was on our original list!

So, looking at our model, T1055.002 does actually belong as a sub-technique, but it needs to shed the baggage of a specific payload and take its rightful place as "T1055.002 New Thread Injection."

### T1055.001 DLL Injection

Buckle up, this sub-technique is even worse. What makes it worse is that it's a sub-technique that combines two distinct things:

1. Injecting a payload that is a DLL (reflective DLL injection, for example)
2. Causing a process to load a DLL via LoadLibrary

The first item is just another variant of a payload-defined sub-technique and thus has the exact same problem as PE Injection: if we're focusing on the format of the payload being injected, then this sub-technique also can't happen independently and shouldn't exist at all.

But, there's the second item still. So, maybe this is a similar case where the name is poorly chosen, but the sub-technique is still unique? Let's make a DDM for it. In order to inject a DLL via LoadLibrary, you have to allocate memory into the target process and write in the path of the DLL you want it to load. Next, you create a new thread that will execute the LoadLibrary API, and you pass the address of your 'payload' (the path of the target DLL) in as a parameter. Windows then goes and loads the DLL into the process' memory for you. The DDM would look like this:

<!-- Image: DDM for DLL Injection via LoadLibrary showing memory allocation, writing DLL path, CreateThread with LoadLibrary as start address -->

Look familiar? When it's in a DDM, it becomes pretty obvious that this is just a different procedure for our renamed "New Thread" injection. The payload isn't actual code, like with the other sub-techniques, but it IS a payload that, when passed to the right execution method, results in code being executed in the target process. There's a difference in what that CreateThread call looks like, so we could distinguish that by adding a flag: 'Thread Start Address: Any' or 'Thread Start Address: LoadLibrary,' depending on the procedure.

But ultimately, this sub-technique shouldn't exist. It's not a unique path at all. The newly renamed "T1055.002 New Thread" sub-technique should address both approaches as two different procedures, and T1055.001 should be retired completely.

### T1055.012 Process Hollowing

In order to see how this sub-technique goes wrong, let's recall an important element of a DDM: you only map the essential operations of a technique. This is because an attacker doesn't **have** to perform optional steps, so they have no value in helping to identify the technique.

The process hollowing sub-technique is distinguished from other sub-techniques by unmapping the original file ('hollowing' the process) before mapping in the malicious payload. However, it is completely unnecessary to unmap the original section. An attacker can simply ignore it and write their payload using any available method. Unmapping the original section has no impact on how the new payload will be executed. An attacker still has to choose an execution method from the list we've already mapped out. So, similar to T1055.001 and T1055.002, this sub-technique cannot be performed independent of another sub-technique. The DDM looks like this:

<!-- Image: DDM for Process Hollowing showing optional "Unmap section" operation that doesn't create a unique execution path -->

I've included the 'Unmap section' operation — even though optional operations should not be included in a DDM — because without it, there isn't anything left to distinguish that this sub-technique exists. This is a clear indication that this sub-technique **shouldn't** exist. It doesn't provide a new, unique operation path and it can't be performed independent of another sub-technique. Adieu, T1055.012.

### T1055.013 Process Doppelganging

And that brings us to our final Windows injection sub-technique for today: Process Doppelganging. This one is gives us a different lesson on the value of DDMs. To implement this sub-technique, you create a "transaction," which is a little-used feature of the NTFS file system that effectively allows you to treat file I/O operations as a single transaction that isn't 'committed' (made permanent) until everything is completed successfully. Should something go wrong, a developer can simply 'roll back' the entire transaction and all changes will be automatically reverted. This sub-technique takes advantage of that by creating a transaction, overwriting a benign file with a malicious payload, using that file to create a new section in memory, then rolling back the transaction. This leaves the attacker with the malicious file in memory but the original benign file on disk. This can confuse EDR and defenders, because the operating system will report that the process is running "svchost.exe" yet the image that was actually loaded is entirely different. So, the DDM for this sub-technique looks like this:

<!-- Image: DDM for Process Doppelganging showing transaction-based file manipulation: Create Transaction, Overwrite File, Create Section, Rollback Transaction, Create Process, Create Thread -->

You will immediately notice that this looks NOTHING like the rest of the sub-technique DDMs we've seen so far! Here they are for comparison:

<!-- Image: Side-by-side comparison of all injection DDMs showing Process Doppelganging's distinctly different operation flow -->

Now, some sub-techniques are drastically different from one another, and that's OK so long as they're implementing the same technique. (Look at accessing the NTDS.dit file via raw disk access or volume shadow copy for an example of two very different sub-techniques that clearly implement the same technique.) But when we get a DDM that looks nothing like its fellow sub-techniques, we should ask if we're really dealing with sub-techniques and not distinct techniques. In the case of Process Doppelganging, the technique isn't **really** injecting code into another process. It would be better described as tricking Windows into loading one image while making it appear like another image was loaded. The sneakiness all happens during the image loading phase, whereas with process injection the sneakiness all comes after the image is loaded. In fact, in our DDMs the operation chain for doppelganging ends where our operation chains for process injection begin! It really feels like these are different techniques entirely.

On the other hand, if we look at another technique that is similar to doppelganging, called [Herpaderping](https://github.com/jxy-s/herpaderping), we'll see some strong parallels! In herpaderping, you create a file, write your malicious payload into it, and then create a new section and process with that image. But before you create a thread to start executing it, you overwrite the image on disk to something benign. THEN you create your thread to start execution, again causing a mismatch between the image that the OS reports it loaded and the image it actually loaded. Let's make a combined DDM for herpaderping and doppelganging. Green is doppleganging, gray is herpaderping, shared operations are black:

<!-- Image: Combined DDM for Process Doppelganging (green) and Herpaderping (gray) showing their similar operation flows and shared steps -->

You can see that there are some strong similarities between these two techniques! The model shows us that these would be best cataloged as sub-techniques for a new, different technique, maybe "Process Image Tampering."

## Summary

In this post, I've used detection data models (DDMs) to map out the various sub-techniques of T1055 Process Injection. I've demonstrated how DDMs can be helpful in guiding you through the process of understanding how techniques, sub-techniques, and procedures relate to one another. Along the way, we've also shown a number of sub-techniques that ATT&CK gets wrong. To wrap things up, here's a look at what T1055 Process Injection (and T???? Process Image Tampering) **should** look like:

<!-- Image: Proposed reorganization of Process Injection sub-techniques showing correctly categorized techniques with proper sub-technique relationships, plus new "Process Image Tampering" technique -->

Hopefully this post helps gives you a better idea of how to create and use a detection data model in your technique identification efforts! If you have any questions or thoughts to add, post a comment below!

(If you were keeping score, I don't cover T1055.011 Extra Window Memory Injection in this post. Pretty sure the DDM would look similar to ListPlanting, but I haven't implemented it yet so I don't want to speak out of turn. Perhaps I'll update the post later and add it in.)

## Further Reading

I was light on details for a lot of these techniques because we had a lot of ground to cover. For those of you who really want more detail, here's some further reading on each technique:

**TLS Injection**

* https://www.mandiant.com/resources/blog/newly-observed-ursnif-variant-employs-malicious-tls-callback-technique-achieve-process-injection
* https://github.com/MahmoudZohdy/Process-Injection-Techniques

**APC Injection**

* https://www.ired.team/offensive-security/code-injection-process-injection/apc-queue-code-injection
* https://www.ired.team/offensive-security/code-injection-process-injection/early-bird-apc-queue-code-injection

**Thread Execution Hijacking**

* https://www.ired.team/offensive-security/code-injection-process-injection/addressofentrypoint-code-injection-without-virtualallocex-rwx
* https://www.ired.team/offensive-security/code-injection-process-injection/injecting-to-remote-process-via-thread-hijacking

**Process Hollowing**

* https://github.com/m0n0ph1/Process-Hollowing

**ListPlanting**

* https://web-assets.esetstatic.com/wls/2020/06/ESET_InvisiMole.pdf
* https://cocomelonc.github.io/malware/2022/11/27/malware-tricks-24.html

**Doppelganging**

* https://www.ired.team/offensive-security/code-injection-process-injection/process-doppelganging
* https://www.blackhat.com/docs/eu-17/materials/eu-17-Liberman-Lost-In-Transaction-Process-Doppelganging.pdf

**Herpaderping**

* https://github.com/jxy-s/herpaderping

---

**Tags:** Process Injection, Detection Engineering, Threat Detection, Mitre Attck, Information Security

---

<!-- source: A4 Mistaken Identification When an Attack Technique isn't a Technique.md -->

# Mistaken Identification: When an Attack Technique isn't a Technique

**Author:** VanVleet  
**Published:** July 1, 2024  
**Reading Time:** 6 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this post, I'm going to talk about one of the challenges Detection Engineers face: sometimes a Mitre ATT&CK technique isn't really a technique at all, which really complicates trying to detect it! I'm going to use T1059.001 PowerShell as my example in this article, but the concept applies to a lot of other techniques (too many!). Ultimately, we'll demonstrate why T1059.001 (and many others) shouldn't even exist.

If you haven't already read my articles on [Identifying and Classifying](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595) and [Detection Data Models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051), you might want to start there so it's easier to follow along.

## Let's Get Started

Let's imagine that you were looking over the top attack techniques used in 2023. Red Canary has an excellent site called ["Threat Detection Report"](https://redcanary.com/threat-detection-report/) that gives lots of great insights into the top attack techniques they're seeing. Directly from their site, "The following chart represents the most prevalent MITRE ATT&CK® techniques observed in confirmed threats across the Red Canary customer base in 2022."

<!-- Image: Red Canary Threat Detection Report chart showing top MITRE ATT&CK techniques, with Windows Command Interpreter and PowerShell at the top positions -->

The top two techniques they saw in 2022 were Windows Command Interpreter and PowerShell. If your leadership team saw this page, there's a fair chance you're going to get a question on what your company's detection posture is for these techniques! So, being proactive, you jump into the task of figuring out how to defend against it. You decide to start with T1059.001 PowerShell, and having recently read my article on DDMs, you're a devoted convert to using them to understand and identify a technique. :) So, you get started building your DDM.

## Building a DDM for T1059.001 PowerShell

You pull up your drawing app and start to plot out the operations for T1059.001 PowerShell. The first operation is…. Um….

<!-- Image: Empty DDM diagram showing confusion - no clear starting operation for "PowerShell" as a technique -->

You have no idea what to put down here, because "PowerShell" tells you absolutely nothing. It's like if someone said "The bad guys are going to use a computer to rob the bank, now go stop them!" There are any number of ways they could be using the computer, from throwing it through a window to an *Oceans 11-*style takeover of the entire system.

## Tools, not Techniques

The problem with an attack technique of "PowerShell" (or Windows Command Interpreter or WMI) is that we haven't actually defined an **attack**. We've only defined a tool, one used by attackers and admins alike. An attacker uses PowerShell to **do** something, like establish command and control, exfiltrate your data, move laterally, or elevate privileges. The sentence "the attackers used PowerShell" is worthless unless it's followed by "to accomplish X." What's really happening here is that PowerShell is the tool and X is the actual attack technique they used it for.

The InfoSec industry doesn't have any difficulty identifying something like MimiKatz or CobaltStrike as a tool, but for some reason ATT&CK really struggles with general-purpose tools like PowerShell, Python, WMI, DLLs and Shared Modules, operating system APIs, cloud APIs, etc.

***A Side Note:*** *I think the case could be made that the entire Execution Tactic shouldn't exist. The problem is similar: execution isn't an attacker objective, it's the way an attacker achieves the other objectives. "Execution" doesn't stand alone. You execute code to do something, and the something is the objective. By gaining initial access, it is implied that you have gained execution somewhere in the network. Additional executions all have additional objectives: move laterally, elevate privileges, exfiltrate data. Most, if not all, of the techniques listed under the Execution tactic are actually tools. The problem of defining tools as techniques probably has its origin in the mistake of defining "Execution" as an objective.*

## Trying to Detect Tools

We confuse ourselves when we define tools as techniques. It makes the ATT&CK framework a bit difficult to work with for detection engineers. When we try to take on the job of detecting a tool instead of a technique, we have one of two problems:

1. If the tool is narrowly focused, like Mimikatz, then we're detecting something tangential and attacker-controlled. As discussed in previous articles, our detection provides little incremental coverage because an attacker can use any tool they want, with an infinite list of options.
2. If the tool is broadly capable, like PowerShell, then we're trying to detect EVERYTHING, as implemented in one specific tool. That is not only a daunting task, it's also very inefficient. If we're going to try and detect every technique, we're better off focusing on detecting the technique itself, including but not limited to implementations that use one specific tool.

**Either way, a focus on the tool over the technique leads to bad detection engineering.** It has a strong likelihood of pushing us towards tangential detections with poor incremental coverage. Focusing on detecting tools, even very capable and frequently-used tools like PowerShell, shifts the game of probability to the attacker's favor. They can, after all, always use a new tool, rendering all our tool-focused detections worthless.

## How to Distinguish Tools from Techniques

For detection engineers, we need to be able to determine when a technique is really a tool so we don't expend valuable time and effort trying to detect it. I would offer this as my litmus test: **an attack technique must be focused on an actual attack (something that achieves a specific attacker objective) and can be defined as an operation chain (even if it's a chain of one!).** Think about it: if it can't be mapped, it can't be identified. And if it can't be identified, it can't be detected!

I think a good example of this distinction can be seen with T1546.003 WMI Event Subscription (for Persistence or Priv Esc) compared with T1047 WMI. The first is a specific action that accomplishes a specific attacker objective: to gain persistence or to elevate their privileges. We can map out the operation chain:

<!-- Image: DDM for T1546.003 WMI Event Subscription showing clear operation chain: Create Event Filter → Create Event Consumer → Create Binding -->

The second is as exactly the same as PowerShell: we don't have any idea what an attacker actually did when we say "they used WMI." It could be anything, it could be nothing. It could be malicious or benign. It's not focused on an actual attack and it cannot be defined as an operation chain. It's a tool, not a technique.

## The Case for Detecting SOME Tools

While I've argued against spending time on tool-based detections, there are some tool-based detections that might be worth the cost of developing and maintaining. Some tools are very frequently abused by real-world attackers (CobaltStrike, for example) and sometimes attackers use them in ways that are really suspicious in your particular environment (most LOLBins and maybe highly obfuscated or frequently abused PowerShell commands, for example). In those cases, even though an attacker can theoretically use any tool and render your detections worthless, the frequency with which those are actually being abused makes them a fair candidate for a tool-specific detection, so long as their incremental cost isn't too high.

## Summary

The ATT&CK framework is useful to detection engineers because it helps us catalog known attack techniques and then methodically research and detect them. However, ATT&CK mistakenly identifies numerous tools as techniques, making a detection engineer's life harder. Focusing on tools instead of techniques makes defending more difficult, because ultimately the attacker gets to choose the tool. You can distinguish techniques that are really tools by determining if they are focused on achieving a specific attacker objective and can be defined in an operational chain. Techniques that are really tools are best left alone, focusing instead on comprehensively detecting the real techniques, regardless of the tool used to implement it.

Hope this was useful to you! If you have any questions or thoughts to add, post a comment below.

---

**Tags:** Threat Detection, Mitre Attck, Powershell, Cybersecurity, Detection Engineering

---

<!-- source: A5 Creating Resilient Detections.md -->

# Creating Resilient Detections

**Author:** VanVleet  
**Published:** November 13, 2024  
**Reading Time:** 12 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

I've spent a lot of time focusing on higher-level [threat detection strategy](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62), but today I'm going to take a detour to talk at a more tactical level: how to create detections that are resilient to common SIEM problems like ingest lag and query failure. Today I'll talk about some common problems and share some best practices for making your detections more resilient, and hopefully save you some future grief!

It is very frustrating to have built the perfect detection, but when the moment comes for it to shine something goes wrong and the event goes by undetected. So let's talk about the kinds of failures we commonly see and what options we have for building in resilience.

## Ingest Delay Resilience

This is both the most common and easiest problem to solve, so it's a great place to start! As a side note, I am by no means the first [person](https://learn.microsoft.com/en-us/azure/sentinel/ingestion-delay) to [discuss](https://cybermsi.com/blog/security/implementing-ingestion-delay-correction-in-microsoft-sentinel/) [this](https://community.splunk.com/t5/Luxembourg-User-Group/Developing-reliable-searches-dealing-with-events-indexing-delay/m-p/665588) [problem](https://www.databricks.com/blog/cybersecurity-lakehouses-part-2-handling-ingestion-delays), but awareness of the issue in the Detection Engineering community seems low and most of the solutions I find online only partly solve the problem. So, I'll spill a little more ink in the hopes of raising awareness and offering a better solution.

Let's begin by defining some terms and concepts:

* **Ingest delay** is the time difference between when an event actually happens and when the log recording that event gets ingested into your SIEM.
* Every detection automation platform I've ever seen involves running a query on a recurring schedule with a defined **lookback window**, which is the window of time that each query execution will inspect. Typically, queries will run hourly or every 30 minutes and look back the same amount of time. This sliding window ensures that every log gets inspected in turn without overlap (which could cause duplicate alerts). Lookback windows can be **immediate** (the time window starts at query time and looks back X minutes) or **delayed** (the time window starts at query time minus some predetermined period and looks back X minutes).
* Whenever a record is ingested into a database or document store, there are two timestamps you should know about: the **event time** is the time that the event actually happened, and the **ingest time** is the time when the log recording that event was ingested into the database. In security, almost every query is time-bounded, and the event time is almost always what's used. (Most out-of-the-box detections and SIEMs I've seen use event time.)

Visualizing this, a 30 minute recurring query with a 30 minute immediate lookback window (using event time) would look like this:

<!-- Image: Timeline diagram showing query executing on 30 minute schedule with 30 minute lookback window -->

Query on 30 minute schedule with a 30 minute lookback window

The problem with ingest lag arises when logs aren't in the SIEM at the point that your detection query is looking for them. Imagine you have a query that is running every 30 minutes, and the log you need to detect a malicious event is delayed by 30 minutes. When your query runs and looks back 30 minutes, the log is not present in the SIEM, so you don't get an alert. But, when the next query runs in 30 minutes, the lookback window has adjusted forward and no longer includes the time when the event happened! Your detection might be 100% capable of detecting the malicious activity, but you miss it because of the ingest lag.

<!-- Image: Timeline diagram showing a missed event that falls into the gap caused by ingest lag -->

A missed event caused by ingest lag

I've never met a SIEM that is actually real-time, so some ingest lag is always present. **If you're using the event time as your query timestamp and an immediate lookback window, you probably have a blind spot.** There are events that would fall in your query window, but haven't been ingested yet. The size of your blind spot will depend on the log source's ingest delay and length of your lookback window, but it's almost certain there is at least a small one! If that critical event happens to occur in the blind spot, you're going to miss it.

Here's how to find out how big your blind spot is: run a query in your SIEM using your typical detection query lookback period, and count the number of records in each 2 minute window. Let's graph it to make it easier to see. You'll likely see a point where the graph begins to slide downhill. That downward slope, plus any totally empty bins, are your detection's ingest delay blind spot.

<!-- Image: Graph showing event count over one hour lookback window with average ingest delay of 30 minutes - shows declining count toward the present -->

Graph of a one hour lookback window on a log source with an average ingest delay of 30 minutes.

If a graph of your log source looks like this and you're using an event time as your detection query timestamp, you have a blind spot. **Remember that even though those logs in the blind spot will get ingested later, they'll never fall inside the query's sliding time window again! Each execution of the query is going to have its blind spot, reducing the chances that your detection will find the specific log(s) it's looking for.**

Even a log with an impressively low lag time will still have a blind spot.

<!-- Image: Graph showing event count over one hour lookback window with average ingest delay of 3 minutes - shows small decline toward the present -->

Graph of a one hour lookback window on a log source with an average ingest delay of 3 minutes.

And in a worst case scenario where the ingest lag is almost as long as your lookback window, your blind spot might be pretty close to a total eclipse!

<!-- Image: Graph showing event count over 30 minute lookback window with average ingest delay of 30 minutes - shows significant blind spot -->

Graph of a 30 minute lookback window on a log source with an average ingest delay of 30 minutes.

Hopefully you're already familiar with your SIEM's normal ingest lag time and you've planned for it. One solution is to use a delayed lookback window that is greater than your longest expected ingest lag (not the average lag!). This allows enough time that events **should** be ingested before you query a given time window.

<!-- Image: Timeline diagram showing query with 30 minute delayed lookback window -->

Query with a delayed lookback window of 30 minutes.

The trade-off with this approach is that your 'time to detect' (the length of time from a malicious event to when you know it happened) is also delayed. In our example above, the delayed lookback provides resilience to ingest delays up to 30 minutes, but your alert on a malicious event is also delayed 30 minutes (regardless of the current ingest delay). But at least you shouldn't have a blind spot, so you can be confident you'll actually get an alert!

**The real challenge with ingest lag is when there are unexpected surges.** An ingest lag that suddenly climbs to two or four hours (or worse!) will almost certainly exceed any delayed lookback, resulting in possible detection misses. And what happens if ingest completely stops for 6 hours while the engineering team finds and fixes the problem? Even if they eventually get all the delayed logs ingested, your query windows will have passed and you'll have missed any alerts that would have fired during the downtime.

**The best solution for resilience to ingest lag is to use the ingest time instead of the event time in your detection queries.** Using an ingestion-based timestamp means that all events are guaranteed to be inspected by your detection query **in the next query after they are ingested**, regardless of how much ingest lag you're experiencing. Even if the SIEM is down for a full day, as long as the delayed logs are eventually ingested, they'll be inspected in the very next query execution. No missed alerts, just delayed ones. Using an ingest timestamp also means you don't have to use a delayed lookback window, so there are no built-in detection delays. You have the lowest time-to-detect possible, whatever the current ingest delay. That's resilience! :)

> Note: In order to make testing for an ingest delay blind spot easier, I've included a simple PowerShell script at the end of this article.

### Using Ingest Time in your SIEM

In my experience, most SIEMs use event time as the primary timestamp (it would mess up other searching use cases to use the ingest time as the primary timestamp) and their automated queries use the primary timestamp by default. So, if you're using the defaults you probably have at least a small ingest delay blind spot. Hopefully, though, your SIEM makes it possible to use an ingest timestamp instead. Here are a few I've worked with:

* Azure Data eXplorer (ADX — which I'm becoming a big fan of!) makes it very easy to use ingest time. The ADX API doesn't define separate parameters for query times, they have to be provided in the query itself. So, you have full control of the time parameters and can just use the ingestion\_time() function, and you're done! Of course, ADX also doesn't provide automated queries, so you have to figure that part out yourself!
* Sentinel's Analytics Rules and Splunk's Alerts both automatically query against the event time, but there are a couple of workarounds available. Microsoft suggests [a limited one](https://learn.microsoft.com/en-us/azure/sentinel/ingestion-delay) (it is only resilient up to the anticipated ingest delay, any longer and you will miss events), and Splunk suggests a [better one](https://docs.splunk.com/Documentation/SCS/current/Search/Timemodifiers#Searching_based_on_index_time) (but it's still a hack that requires you to knowingly use it!). Splunk's solution works for both platforms, so I'll summarize it here. Add a clause to your query to select on ingestion time: in Splunk, use the \_index\_earliest and \_index\_latest fields, in Sentinel use the ingestion\_time() function. You can see examples of each in the links above. Make sure you define the time dynamically: -1h in Splunk and ago(1h) in Sentinel. Then set the query lookback (which uses event time) as far as the platform will allow ("All Time" for Splunk and the last 14 days for Sentinel).\* This means the event time-based clause will not play a meaningful role in selecting records, so your ingest time clause becomes the primary time-based filter.
* ElasticSearch, if you're using the API directly, allows you to specify any timestamp in your query's range statement. You do have to [add ingest time](https://discuss.elastic.co/t/how-to-add-time-of-ingestion-to-the-document/243042) to records in your ingest pipeline, since it's not added by default. In the Elastic Common Schema, the ingest time should be named event.ingested, so use that and you're set.
* I've never used Splunk Enterprise Security or Elastic Security (Splunk and Elastic's SIEM offerings), so if you know how to use ingest time in your automated queries on those platforms, post a comment below and share the knowledge! I'm guessing you could get Splunk's workaround to work in either platform.

You'll have to figure out the best solution for your environment, given your SIEM and ingest delays (don't forget to think about normal delays and potential surges!). If your SIEM doesn't support using ingest times for automated queries, it's time to put in a feature request! Also, at the end of this article is a script you can use to verify your solution is working.

\* Allow me to debunk something I saw in multiple articles talking about Microsoft's solution: the idea that setting the maximum lookback window means a less efficient query. This is a misunderstanding of what this query is doing. The ingest time criteria limits the records searched in the same way an event time criteria would limit it; if you're using the same lookback window length for both the efficiency should be equivalent. You can see this for yourself in Splunk by running a search for "index=whatever | stats count" with "last 60 minutes" in the time picker. Then inspect the job and note how many records it scanned. Now, search "index=whatever \_index\_earliest=-60m \_index\_latest=now | stats count" with the time picker at "All Time" and inspect the job. The number of records scanned and time taken won't be drastically different. By setting a shorter event time lookback window, like in Microsoft's solution, you're just setting limitations on your ingest delay resilience: as soon as ingest delay exceeds your event time lookback, you start missing events. **There is no trade-off between ingest delay resilience and query efficiency when using an ingest timestamp properly.**

## Query Failure Resilience

Another source of detection misses is query platform failures. What might fail is platform specific, but any platform can have failed runs due to things like API rate limits, network failures, or platform downtime. If bad luck causes a malicious event to coincide with a failed query window, the event will be missed.

<!-- Image: Timeline diagram showing event missed due to query failure -->

Event missed due to query failure.

There are three options for addressing query failure misses. Unfortunately, the best options require support from your detection automation platform, so if your platform doesn't support any of these, it's time to put in a feature request!

### Option 1: Health monitoring and manual/automated resubmission

For this option, you configure notifications for any failed queries, and then you re-run any failed queries to ensure no malicious events were missed. This can be done manually or automatically, but automatic resubmission is definitely superior. It could be difficult to re-run all detections manually if you have hundreds or thousands of them. Ideally, this solution would be automated so that a failed query is resubmitted to cover the missed timeframe. Even more ideally, this automated failure detection and resubmission would be implemented by your detection automation platform, so you don't have to worry about it at all!

### Option 2: Overlapping Lookback Windows

You can build in some failure resilience by configuring your detection queries to use overlapping lookback windows. Then, if one query fails, a subsequent query will still cover the failed query window and you won't miss any events. Your failure resilience will be equal to the number of overlapping windows, but consecutive query failures will result in missed alerts. For example, you could use a 30-minute recurring query with a 1 hour lookback window, and this would provide resilience for a single failure. Two consecutive failures would leave a missed query window.

<!-- Image: Timeline diagram showing query on 30 minute schedule with 1 hour overlapping lookback window -->

Query on a 30 minute schedule with a 1 hour overlapping lookback window.

This approach requires some method for deduplicating alerts because under normal circumstances a given malicious event will be inspected in multiple executions of the detection query. It also can cause some hiccups with event grouping: if you deduplicate AFTER you've grouped events, then a subsequent query execution might find a slightly different set of events (because some of them might fall outside the new lookback window) and generate a duplicate alert. All told, this solution can work, but it definitely can cause its share of headaches. I prefer options 1 and 3.

<!-- Image: Timeline diagram showing duplicated alert caused by grouping events on different windows -->

Duplicated alert caused by grouping events on different windows.

### Option 3: Adjusted Query Lookback

This option requires support from your detection automation platform, but it's a simple solution. In this solution the automation platform records the query window for each execution. When a query runs, the platform automatically adjusts the query window to cover the time between the last successful execution and the current time. No deduplication is required because each time window will be queried only once.

<!-- Image: Timeline diagram showing a failed query with adjusted lookback window for subsequent query execution -->

A failed query with an adjusted lookback window for the subsequent query execution.

## Conclusion

Hopefully this article has provided some useful ideas for how you can ensure your detections are as resilient as possible, so they'll work in the best of times and the worst of times (perhaps just a little delayed).

If you have any thoughts to add, please leave them in the comments below!

## Script for Measuring Your Ingest Delay Blind Spot

If you're concerned that you have an ingest delay blind spot, here is a simple PowerShell script to help quantify it. The script will spawn a cmd.exe process with a unique command line every 5 minutes for one hour, for a total of 12 instances. To use it, create a detection in your automation platform for a process command line containing the string "IngestDelayTestCommand." Set it to run at your normal recurrence frequency and with your typical lookback window. Then run this script on a machine that's logging to your SIEM and verify how many of the 12 possible alerts you actually get. Then ask yourself how many missed events is acceptable. (I know, I know… I'm leading the witness. 😁)
```powershell
#This script helps test for an ingest delay blind spot  
#To use it, create a detection in your automation platform that looks for "IngestDelayTestCommand" in a process command line.  
#Then run this script on an asset that is logging to your SIEM. The script will execute a cmd.exe process that echos the expected   
#string every 5 minutes for an hour. Your ingest delay blind spot is the number of alerts you DON'T get out of the 12 possible.  
  
for ($i=1; $i -le 12; $i++) {  
    $currenttime = Get-Date -Format "HH:mm:ss"  
    $command = 'echo {0} IngestDelayTestCommand - Test #{1}' -f $currenttime, $i  
    & cmd.exe /c $command  
    Start-Sleep -seconds 300  
}
```

Output will look like:
```
07:33:38 IngestDelayTestCommand - Test #1
```

And the command line will look like:
```
"C:\WINDOWS\system32\cmd.exe" /c "echo 09:13:06 IngestDelayTestCommand - Test #1"
```

---

**Tags:** Threat Detection, Resilience, Detection Engineering, Siem

---

<!-- source: A6 Technique Analysis and Modeling.md -->

# Technique Analysis and Modeling

**Author:** VanVleet  
**Published:** March 11, 2025  
**Reading Time:** 6 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

As I've built a framework for how to keep our networks safe through threat detection, I've established the overall [strategy](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62) and explained how we can shift the odds [strongly in our favor](https://medium.com/@vanvleet/compound-probability-you-dont-need-100-coverage-to-win-a2e650da21a4) using thorough (sub)technique-focused detections.

My goal with this article is to pull all of this strategy together and demonstrate a practical analytical process to apply it to real world attack techniques. In this article, I'll walk through how to analyze a technique to identify distinct procedures and create a strategy for building a thorough detection. I recently did a [podcast](https://youtu.be/5DAQkvOyqME?si=vutHl_lQKcGq_95g) where I go through this same analysis process using the Kerberoasting (T1558.003) technique, so there are multiple examples for all learning styles! :)

This analytic process is most effective when it's facilitated by a model. If this is the first time you've heard of technique modeling (also called detection modeling), pause and go read my article on [detection data models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051) (DDMs). If you just need a refresher, modeling is a tool that helps to avoid the thinking errors that are common when dealing with complex concepts like attack techniques. When you have to map out your knowledge in a model, you quickly discover the areas where your understanding is lacking and assumptions are filling in the gaps (or you're just missing things!). You can also explore the technique visually, helping you see things you might not think of otherwise.

The steps that we'll follow when analyzing a technique are:

1. Start your model by adding in the things you already know.
2. Choose an operation and go deeper. Expand your knowledge.
3. Add what you've learned to your model, adjusting it as needed.
4. Repeats steps 2 and 3 until you're reasonably confident you've gotten it all.
5. Add in available telemetry.
6. Identify any other possible paths through the model.
7. List the distinct procedures.
8. Document your results.
9. Use the model to create a detection strategy.
10. Implement the detection(s).

I covered steps 1 through 6 for the technique *Create or Modify System Process: Windows Service* (T1543.003) in my [article on DDMs](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051) (and in the podcast I go through steps 1–9). So, in order to keep this article short, we're going to pick up where we previously left off with T1543.003. Here was our completed DDM (with some additional detail, since we're taking it all the way through this time):

<!-- Image: Complete DDM for T1543.003 showing all operations, telemetry sources, and multiple procedure paths including Create Remote Registry Key, RPC via TCP, and RPC via Named Pipe -->

*A quick note: To save time in the last article, we did not expand on the "Create Remote Registry Key" operation. In real life, we should. That operation uses the* [*Windows Remote Registry Protocol*](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-rrp/0fa3191d-bb79-490a-81bd-54c2601b7a78) *[MS-RRP], another RPC interface. Because that procedure is so different form the rest, we're best covering it in proper detail in its own model (who knows, there may be a more paths to do it than RPC!). But, to save time in this article, we're going to skip analyzing that procedure. That does mean that our detection strategy could be missing opportunities: a good case study in the costs of not analyzing and modeling a procedure.*

## Step 7 — List the Distinct Procedures

We have identified 3 distinct paths through the model, so that's 3 procedures:

1. Create a registry key remotely.
2. Call the RCreateServiceW RPC call using a TCP connection.
3. Call the RCreateServiceW RPC call using a named pipe.

## Step 8 — Document your results.

The next step is to capture the results of your analysis, including your model, for your whole team and for future detection engineers. The goal is to document the distinct procedures along with all the background and technical information necessary to understand how and why those procedures work. This provides the context, information, and telemetry necessary to build a detection strategy to detect the technique as comprehensively as possible. This report can enable any detection engineer to quickly create a thorough detection for the technique in their own environment.

In a future post, I'll share the template that I use for documenting technique analysis, along with some examples. I call them Technique Research Reports (TRRs). But more on that later….

## Step 9 — Use the model to create a detection strategy.

The next step is to employ our model to create a detection strategy. Recall that detecting a technique has two tasks: [identifying and classifying.](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595) We can only classify events that we have identified, so if we base a detection on a log source that identifies only 50% of the procedures, the very best we can hope for is to cover 50% of the technique's attack surface. We need to detect as many of the procedures as possible, so we're ideally looking for a point in the model that has telemetry and is shared by many or all procedures. Otherwise, we'll need a group of detections that collectively cover all procedures.

On this model, there is one spot that definitely offers better identification than the rest:

<!-- Image: DDM highlighting the "Create Registry Key" operation as the ideal detection point covering 100% of procedures -->

If we can create a detection at the "Create Registry Key" operation, we can identify 100% of the events. Hopefully, we can also find a way to classify those events as malicious with high fidelity, too! (But remember, classification is an environment-specific task: some environments might have so little noise that any new services are suspicious, while others might see new services all the time.) If you can set a SACL or have Sysmon in your environment (or an EDR that provides an equivalent log), you're set!

However, if you don't have any of those then we need to keep looking. The next best option is at the "Receive RPC" operation.

<!-- Image: DDM highlighting the "Receive RPC" operation as the second-best detection point covering 2 of 3 procedures -->

At this point, we can identify 2 of the 3 procedures. The 3rd procedure is going to end up a known blind spot (but that's ok, remember [we don't have to get 100% to win!](https://medium.com/@vanvleet/compound-probability-you-dont-need-100-coverage-to-win-a2e650da21a4)). You have two possible logs to choose from at this operation, so hopefully you find one that works. *(Again, if we delve into that 3rd procedure — creating the remote registry key — we may find some detection opportunities and it wouldn't have to be a known blind spot.)*

But let's keep going a bit further. Imagine that for some reason, you can't get the RPC event logs. At that point, your best strategy is going to be to try and catch the network communications (it's the only option left!). At this spot in our model, there are two paths that we'll have to address separately, so our detection strategy will be pair of complementary detections.

<!-- Image: DDM highlighting network communication operations (SMB Named Pipe and TCP connections) as the final fallback detection points -->

## Step 10 — Implement the detection(s).

Once you've applied the model to your environment, you hopefully will have found a solid detection strategy that works for your telemetry and environmental noise. Now it's time to write and deploy your detection queries. At this point, don't anguish over what you can't detect. Remember that we're building a mesh of detections that, in the aggregate, has a high likelihood of detecting an adversary somewhere on their path from initial access to impact. Just document your known blind spots, and make note of anything that would allow you to build a better detection in the future: maybe the ability to set a SACL, or enabling a particular log in Windows.

## Repeat!

At this point, you've analyzed the target attack technique, documented your results, built a strategy, and implemented it with detections. Now you select a new technique and start over. Technique by technique, your mesh will fill in, your attack surface coverage will grow, and the probability that an attacker will slip by undetected will shrink!

## Thoughts?

Thanks for reading! I hope you found something useful. If you have any thoughts to add, post a comment below!

---

**Tags:** Threat Detection, Detection Engineering, Detection Data Model, Technique Modeling, Cybersecurity

---

<!-- source: A7 Technique Research Reports Capturing and Sharing Threat Research.md -->

# Technique Research Reports: Capturing and Sharing Threat Research

**Author:** VanVleet  
**Published:** November 14, 2025  
**Reading Time:** 8 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

In my article on [Technique Analysis and Modeling](https://medium.com/@vanvleet/technique-analysis-and-modeling-ffef1f0a595a), I talked about the need to document the results of your research. In this article, I'll share details about why and how to capture and share the output of technique analysis and modeling in a Technique Research Report (TRR). I'm also announcing a [public repository of TRRs](http://library.tired-labs.org) to enable the industry to share good technique analysis! Details on that at the end.

## The Detection Engineering Process

Before we get into the details of TRRs, let's first explore what we want to accomplish.

When creating a detection, a detection engineer (DE) will (hopefully!) follow a process similar to this:

1. **Research the technique** — some work must be done to understand the attack technique that will be detected. Ideally, this is robust and identifies all the procedures, but at a minimum there should be some effort to understand what the technique looks like.
2. **Identify possible telemetry** — determine what potential telemetry sources are available, so the DE knows what options they have to work with.
3. **Select a log source** — a log source(s) will be selected as the best option for detecting the technique in the target environment.
4. **Build the detection query** — write the query logic to actually implement the detection in a specific SIEM. The query also needs to be adapted to the target environment to minimize false positives.

Looking at these steps, some of them are universally applicable (those that involve understanding and identifying the technique) and some are environment specific (those that involve specific logs sources, SIEMs, and environment noise and tuning).

<!-- Image: Diagram showing detection engineering process steps with "Research Technique" and "Identify Telemetry" marked as universally applicable, while "Select Log Source" and "Build Detection Query" marked as environment specific -->

Identifying telemetry splits in the middle because some telemetry is commonly available (Windows event logs, for example), while some is environment specific (your particular EDR).

(As an aside, the universally applicable sections are generally those doing [identification](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595), while the environment specific ones are doing [classification](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595). This is unsurprising because classification is inherently environment specific.)

## Lossy and Lossless Information Capture

Let's take a quick side track. I'm going to borrow a concept from compression algorithms: lossy and lossless compression. Lossy compression is when you store information in a way that some of it is irrecoverably lost, while lossless compression keeps all of the information but attempts to pack it in as tightly as possible. We're all really familiar with this concept when dealing with pictures and videos. At some point, we all realize that we don't REALLY need to keep that 50MB, 200MP picture of us doing something stupid because we're unlikely to ever print a wall-sized poster of it. A lossy 3MB version is enough to keep the memory alive.

<!-- Image: Photo showing "Me, doing something stupid. But AWESOME." - appears to be someone in an unusual position or doing an athletic feat -->

The effort of capturing threat research has parallels. A technical write-up of an attack technique captures a lot of the information (lossless), while a detection query captures very little (lossy). Part of that is because a published detection, from a practical standpoint, can't come with 5 pages of documentation, including graphics and screen captures! The other part is that the act of creating a detection query involves environment specific decisions: selecting the best telemetry source available in that environment, determining which procedures actually apply there (and which are covered by other defensive layers), what noise to tune out (or not), what query capabilities the SIEM can support (or the given detection engineer was able to implement), etc. From the point where we take the universally applicable information and start to make decisions on a specific detection implementation, we begin losing some information. **The decisions that are best for one environment may NOT be the best for another, but the decisions made and details that informed them are not captured in the detection.** This information is lost. As a result, detection queries are a poor vehicle to capture and convey information about attack techniques!

## Measuring the Gap

Despite being a lossy vehicle, we share detection queries in the InfoSec industry all the time. There are numerous public repos and a dozen vendors offering thousands of pre-built detection queries for nearly every technique on the ATT&CK matrix. So here's the rub: when someone gets one (or thousands) of detection queries, they have to make a decision. Do they do the technique research to understand how the detections — and the procedures they are purported to detect — apply to their environment? Or do they just deploy them?

I like to compare this to the process of building a wall. When you're building a wall (imagine your favorite war strategy game here), you need to make an assessment of the gap that it's intended to block. How big is it, what shape is it, is actually a gap or just an alcove? Building without measuring the gap can result in a wall that gives the impression of safety but offers little real protection!

<!-- Image: Screenshot from CodeCombat.org game showing a poorly placed wall that doesn't properly block a gap -->

Source: CodeCombat.org, MY favorite war strategy game!

It looks ridiculous in the game, but I'd wager we have an awful lot of unmeasured detection walls in real life!

But detections aren't the only way we share technique research. There are many technique write-ups that have been published, but they take some time to sort. Some are excellent: thorough, accurate, and well presented. Others are boilerplate (and I swear half of them are AI-generated). And some are downright inaccurate! What's most problematic, though, is that the majority of them only cover a single, well-known procedure. The end result is that it can take quite a bit of time to do thorough technique analysis and modeling, even for well-documented procedures. If you're too hasty about it, you'll have an incomplete understanding of the technique, resulting in an incomplete detection strategy and a poorly measured wall.

## Technique Research Reports — Lossless Capture and Sharing

Coming back full circle, the portion of our technique analysis that is most useful to capture and share is the universal part.

<!-- Image: Diagram highlighting "Research Technique" and "Identify Telemetry" as the universally applicable portions worth capturing in TRRs -->

This is the purpose of a Technique Research Report (TRR). A TRR documents the distinct procedures that implement a technique, including the background and technical information necessary to understand how those procedures work. **TRRs provide the context, information, and potential telemetry needed to create a robust detection strategy tailored to your specific environment.**

Let's take the Kerberoasting detection data model we created in the [Detection Engineering Dispatch podcast](https://www.youtube.com/watch?v=5DAQkvOyqME&list=PLeaA8CQiZrWyodUEdNL2yBA4dM5TTBkrI&index=15):

<!-- Image: DDM for Kerberoasting (T1558.003) showing complete operation flow and telemetry sources -->

A [TRR on Kerberoasting](https://github.com/tired-labs/techniques/blob/main/reports/trr0018/ad/README.md) will contain this DDM, a list of the distinct procedures we've identified, and all the background information needed to understand them.

With this information, a detection engineer should have no difficulty determining which of the procedures work in their environment and what telemetry (either from the DDM or unique to their environment) they have available to build their detection strategy. Measuring the gap becomes easy, all that's left is to build the wall.

## The TRR Repository

To make it easier to share thorough attack technique research, I have created a public [TRR Library](http://library.tired-labs.org) on GitHub that the entire InfoSec industry can use and contribute to. I've already got some of my own work in there, and would love for you to contribute your work, too! With time, I'm hoping this can become a real force multiplier for the InfoSec community: a place to find and share high quality technique analysis and modeling that can speed up your efforts to deploy thorough detections.

Do you have some great technique analysis you'd like to share? Want a future employer to see what you can really do? Pick an attack technique, do some mind-blowing modeling and analysis, document it in a TRR, and submit a PR to have it included in the Library! Cred for you, excellent research for all of us. We all win! :) Here's the [contribution guide](https://github.com/tired-labs/techniques/blob/main/docs/CONTRIBUTING.md).

## TRR Format

While any format would work, I'll share the format the TRR library is using. For more details, the Library has a detailed [TRR Guide](https://github.com/tired-labs/techniques/blob/main/docs/TECHNIQUE-RESEARCH-REPORT.md).

### TRR Title

### **Categorization Section**

This section captures some of the metadata for the technique: the platforms it applies to, the MITRE technique number, etc.

* **Technique ID —** This is a unique ID to identify the technique in the Library. It's assigned when the TRR is accepted for publication.
* **External IDs —** The ID for the technique on all relevant threat matrices (doesn't have to just be ATT&CK)
* **Tactic —** The tactic(s) that this technique falls under.
* **Platforms** **—** The platforms that are covered in this TRR. Do not list platforms where the technique applies but will not be addressed in this TRR, those should be listed in the TRR that covers them.

**Scope Statement (optional)**

TRRs are focused on a single, specific attack technique or sub-technique for a specific platform. This is an optional statement of the scope of the TRR, like how it maps to techniques on other matrices and any rationale for the choice on scope.

### **Technique Overview Section**

This section is the executive summary of the attack technique.

### **Technical Background Section**

This section contains the real substance of the TRR. It should capture all the key technical details and background needed to understand how the technique works. The rule of thumb is to ensure that the reader doesn't need to go anywhere else for the details needed to understand the technique. Links can be provided to further details, but the key details should be included here. The structure of this section is not rigid, the author has discretion to determine the best way to present the information.

### **Procedures Section**

This section begins with the below table identifying each unique procedure that implements the technique:

<!-- Image: Example table showing procedure listing with columns for Procedure ID, Name, and Description -->

Following the table will be individual sections for each procedure, which will contain a summary of the procedure, a detection data model (DDM), and any additional technical details or background that are specific to the procedure (details relevant to all procedures should be in the Technical Background section above). It is possible that no additional detail is required for a procedure, in this case the section will simply contain a summary of the procedure and a DDM. This is where you might add ideas for how a procedure could be identified, any details about the telemetry identified in the DDM (specific field values, for example), and other useful but not environment-specific information about the procedure.

**Procedure #1 Details**

<Image of DDM>

Details of the procedure.

**Procedure #2 Details**

<Image of DDM>

Details of the procedure.

### **References Section**

This section is used to capture reference documents used in the TRR, good explainers, and other resources that would help someone dive deeper into the technique or platform.

## Conclusion

Thanks for reading today! Hopefully I've offered a compelling reason for why we as a detection engineering community should be doing thorough technique analysis and modeling and sharing our work with one another. I'll see you on GitHub!

---

**Tags:** Detection Engineering, Information Sharing, Cybersecurity, Threat Research

---

<!-- source: S1 Plotting a Winning Threat Detection Strategy A Visual Model.md -->

# Plotting a Winning Threat Detection Strategy: A Visual Model

**Author:** VanVleet  
**Published:** January 23, 2024  
**Reading Time:** 9 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this article, I'm going to set up a model for thinking about threat detection and then use it to answer two fundamental questions:

1. Where is the best place to focus detection engineering efforts to maximize impact?
2. How do we evaluate the quality of a detection? What makes one detection better or worse than another?

## The Model

First we'll build a visual model. we'll start with Mitre's ATT&CK framework for Windows.

<!-- Image: Mitre's ATT&CK Framework for Windows - diagram showing the ATT&CK matrix -->

We're going to represent each technique as a single dot.

<!-- Image: Diagram showing each technique represented as a single dot in the model -->

Then, if we've done a good job defining our attack techniques, an attacker's path through the network from initial access to objective could be represented as a path between dots. Obviously, an attacker doesn't have to use a technique from EVERY tactic, but they do have to use SOME.

<!-- Image: Diagram showing an attacker's path through dots from initial access to objective -->

Using this model, the goal of threat detection is to build mechanisms to prevent and/or detect as many techniques as possible, so an attacker can't get from initial access to objective without triggering alarms. At this point, it becomes a game of probability: how probable is it that an attacker will take a path through that doesn't alert you to their presence?

<!-- Image: Diagram showing coverage of techniques to prevent attacker paths -->

Let's make this simple model just a little more realistic. Many techniques have numerous sub-techniques. There are 193 techniques and 401 sub-techniques in ATT&CK v12. We're going to approximate that by turning some dots into clusters of dots, so that each dot now represents a sub-technique.

<!-- Image: Diagram showing dots clustered together representing sub-techniques -->

Then, let's consider that each sub-technique may have multiple distinct procedures for how it can be executed. Jared Atkinson provides a very compelling demonstration of this idea in an excellent [blog series](https://posts.specterops.io/on-detection-tactical-to-function-810c14798f63) where he refines the definition of a "procedure." He demonstrates with an example where he identifies 4 distinct procedures for the sub-technique "OS Credential Dumping: LSASS Memory" and graphs them into a single chart, which I'm going to call a "procedure map."

<!-- Image: T1003.001 OS Credential Dumping: LSASS Memory procedure map by Jared Atkinson -->

(Adding my own note to Jared's work here: a procedure map should only include the ESSENTIAL operations that MUST BE EXECUTED in order to implement the procedure. We'll talk more about this when we discuss detection data models.)

Adding in procedures, our model becomes a mass of dots and clusters of dots, with each dot representing a procedure and clusters representing procedures that implement the same sub-technique, or Jared's "sub-technical synonyms."

<!-- Image: Diagram showing mass of dots and clusters representing procedures and sub-techniques -->

## Answering the Questions

Using this model, let's discuss what makes the best detection. The natural answer is "the detection that comprehensively covers the most dots." The more dots a detection covers, the more likely an attacker's path through the network will traverse one of them. But from a logistical perspective, it's impractical to detect unrelated procedures in a single detection. (It's hard enough to maintain simple detections, who needs complex ones?) So, if we can find closely related procedures that can be covered in a single detection, or a set of related ones, that's where we get the most impact. Luckily, this grouping of related procedures is already done for us: sub-techniques are often a cluster of related procedures. So, the theoretically ideal detection would be the ***one that comprehensively covers all the procedures of a single sub-technique.***

<!-- Image: Diagram highlighting ideal detection coverage -->

The detection engineering task is to find the best detection possible given the sub-technique's procedure map, available telemetry, and environmental noise. We're going to borrow Jared's procedure map again to dive in a little deeper.

* A detection that can catch all four procedures (at one of the graph's bottlenecks, for example) is the theoretical **ideal** (yellow on the graph). (Whether or not that ideal is viable depends on telemetry and noise in the given environment.)
* A set of detections that catch all four, perhaps at different points, is the **second best** (green on the graph). You have the same coverage, and just a little more work to maintain.

<!-- Image: LSASS procedure map with yellow highlighting showing ideal detection points -->

These are the best case scenarios. In real life, we are often only able to detect some of the procedures, or even some portion of some procedures. The rest are a known gap.

Now let's discuss the worst case scenarios.

The first is one where all we can do is detect the **tangential** elements (brown on the graph) of a procedure, like the command line parameters used by a specific tool that implements one of the procedures. When we focus on tangential elements, there are almost infinite paths through the procedure map. (An attacker can create a new path by writing a new tool, scripting the procedure, using command-line obfuscation, or load and execute the tool in memory, just to name a few.)

<!-- Image: LSASS procedure map with brown highlighting showing tangential detection points -->

Detecting **tangentials** shifts the probability game to strongly favor the attacker. This shouldn't be done until all better options have been exhausted. (And yet, much of the publicly available threat detection content is of this nature!)

The next common detection pattern that falls in the worst case category is the one that looks for **tuples** of procedures. For example, the detection might look for an EXE, ISO, or ZIP file written by Outlook (T1566.001 Spearphishing Attachment) that executes a Powershell script (T1059.001 Powershell). The problem is that this detection covers a specific 2-tuple of dots.\* Any other combination, even using some of the same procedures, won't be caught. This turns our hundreds of dots into a hundred thousand 2-tuples! (If we had 500 procedures, there would be [124,750 possible 2-tuple combinations](https://www.calculatorsoup.com/calculators/discretemathematics/combinations.php?n=500&r=2&action=solve).) Covering that many combinations requires a lot of detections, so this clearly shifts the probability game in the attacker's favor.

<!-- Image: Diagram showing tuple detection pattern -->

*\* We give this hypothetical detection more credit than it deserves. This example is worse than just a procedure chain, because it doesn't comprehensively cover all the individual procedures in the chain. An attacker could traverse the exact path and still evade detection by using a different implementation of the procedure, like phishing with a different file type.*

There's one more observation to extract here. Let's explore our LSASS Memory example a little further. Let's suppose we have telemetry from the Process Access operation showing the target process and requested access rights (maybe from an EDR hook on NtOpenProcess, for example). We'll add the rights requested by each tool to our graphic. (Note that reading credentials from LSASS memory only actually needs the "PROCESS\_VM\_READ" permission, but at least two tools overshoot and request all possible permissions.)

* A detection (or set of detections) for LSASS with any flag combination that includes "PROCESS\_VM\_READ" or "PROCESS\_CREATE\_PROCESS" or "PROCESS\_DUP\_HANDLE" would be the theoretical **ideal** (yellow) because it covers all 4 procedures at an essential operation.
* A detection looking for LSASS and "PROCESS\_ALL\_ACCESS" is a worst case scenario, because it's looking for the specific implementation of a specific tool (or tools, in this case). Even though it looks very similar to the ideal, we're really back to detecting **tangentials** (brown).

<!-- Image: LSASS procedure map with process access rights annotations -->

## Doing the Math

Using this model, we can make a rough mathematical representation of the incremental coverage value of a given detection. Let's pretend that there are 500 total procedures in our cluster of dots (that's probably way too low, but it suffices for our purposes).

* A detection that comprehensively covers one procedure covers 1/500th of the total attack surface.
* A detection that covers 4 procedures (like our ideal detection in the LSASS Memory example) covers 4/500th of the attack surface.
* A detection that can only reliably cover half of a procedure is still covering 1/1000th of the attack surface.
* A detection that covers 1 2-tuple is covering 1/124,750th of the attack surface (remember that 500 items allows for 124,750 possible 2-tuples).
* A detection that covers one tangential element is effectively covering 1 of nearly infinite options, but for the sake of our math let's generously assume there are only 1,000 different tangential elements an attacker could introduce. Thus, it covers 1/1,000th of 1 of 500 procedures, so that's 1/500,000th of our attack surface.

Obviously, our math is rough, but it serves to illustrate a few important points:

1. Not all detections provide the same coverage value.
2. Our coverage impact is orders of magnitude greater when we focus detection efforts on covering a procedure as comprehensively as possible.
3. Detection strategies that use tangential elements or tuples shift the probability game in favor of the attacker. They shouldn't be pursued until more effective options have been exhausted. ([Some cover so little attack surface that they may not be worth the development and maintenance effort!](https://medium.com/@vanvleet/the-threat-detection-balancing-act-coverage-vs-cost-cdb71d21412f))
4. The best level to focus our detection engineering efforts is the sub-technique level, where we have clusters of similar procedures that we might be able to detect together.

## One Last Pattern

There is one last type of detection pattern that can be effective, which I'm going to call a "grouple" detection. This is similar to the tuple pattern, but differs in a critical way. Instead of looking at just tuples of single procedures, it's looking for tuples of entire groups or categories of procedures. For example, we might look for ANY alert under the "Initial Access" tactic, followed within a certain timeframe by ANY alert in the "Persistence" tactic.

<!-- Image: Diagram showing "grouple" detection pattern across tactic groups -->

This detection pattern has the potential to be effective, so long as our coverage of the individual procedures in each group is good. But where it particularly excels is in situations where telemetry for a given procedure is too noisy to alert on directly. We can't cover the procedure itself, but we can alert when it co-occurs with other (possibly also noisy) procedures, allowing us to build in some coverage where none is otherwise possible. That makes "grouple" detections an excellent option to cope with those inevitable cases where the environment noise is just too loud to permit a high-fidelity detection (scheduled tasks, I'm looking at you!).

## The Winning Strategy

Using this visual model, I hope I've offered some compelling answers to my two fundamental questions:

**Q**: Where is the best place to focus detection engineering efforts to maximize impact?

**A**: *At the sub-technique level, covering each procedure as comprehensively as possible.*

**Q**: How do we evaluate the quality of a detection? What makes one detection better or worse than another?

**A**: *The best detection is the one that covers the procedures of a sub-technique as comprehensively as possible. Be careful to focus on essential elements, not tangential ones! Detections that focus on tangential elements are not helping us win.*

## Summary

With these answers, we can see that a great strategy for winning this game of probability is to review each sub-technique one by one and implement rules to detect (or to prevent) each procedure. Where telemetry shortcomings or environmental noise prevent a detection, we can generate an event that can be bundled into "grouple" detections.

<!-- Image: Summary diagram showing the complete strategy -->

If you can cover enough of the threat space, and maybe with a little help from Lady Luck, you can keep attackers out of your network!

## Thoughts?

Hopefully this exploration of my visual model has helped clarify the challenges we detection engineers are constantly trying to tackle! If you have any thoughts to add, post a comment or let me know on [Twitter](https://twitter.com/_vanvleet)!

*Originally published at [https://www.linkedin.com](https://www.linkedin.com/pulse/threat-detection-visual-model-andrew-vanvleet).*

---

**Tags:** Threat Detection, Threat Hunting, Detection Engineering, Information Security

---

<!-- source: S2 Identifying and Classifying Attack Techniques.md -->

# Identifying and Classifying Attack Techniques

**Author:** VanVleet  
**Published:** February 14, 2024  
**Reading Time:** 7 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this post, we'll focus on the challenge of identifying and classifying events in order to detect a given attack technique.

*Update 8/29/2024: I know we're all standing on the shoulders of giants as we improve our understanding of and skills in detection engineering, but in the case of this article I've discovered that Jared Atkinson wrote about the idea of the two primary tasks of identification and classification a few* ***years*** *before me. Definitely recommend giving* [*his article*](https://posts.specterops.io/thoughts-on-detection-3c5cab66f511) *a read! Thanks, Jared, for letting me stand on your shoulders so often!*

## The Two Primary Tasks

When trying to detect an attack technique, there are two primary tasks that a Detection Engineer has to accomplish:

1. Identify when an event that is integral to an attack technique occurs in their environment.
2. Classify each identified event as malicious or benign.

Both tasks must be done successfully in order to detect the attack. For example, if we can identify with 100% accuracy that a scheduled task ([T1053.005](https://attack.mitre.org/techniques/T1053/005/)) is created, but we can't classify each new task as malicious or benign, we can't detect this technique. Alternately, perhaps we can classify a Golden Ticket ([T1558.001](https://attack.mitre.org/techniques/T1558/001/)) with 100% accuracy, but we have no telemetry to identify it. In either case, we cannot successfully detect attacks using that technique. In real life detection scenarios, we often end up with mixed results: perhaps we can identify 80% of the events, and of those we can classify 80%, giving us a 64% probability of detecting a specific malicious instance. **The more accurate we can get in either category, the better our probability of detecting a malicious instance of the target attack technique.**

## An Environment Specific Job

If you ever wondered why every company has to hire their own detection engineers, or why there isn't some universal solution to detecting known attack techniques, the reason is that identification and classification are both *very* environment specific. **Identification depends heavily on what sources of telemetry a given company has available. Classification requires filtering the malicious signal from the noise in a given environment.**

The Mitre ATT&CK matrix does a fair job of cataloging attack techniques, but it's up to detection engineers to figure out how to detect those techniques given the set of telemetry and noise in their enterprise. A detection that is highly effective for one enterprise might be unworkable for another because they don't have the same telemetry or they have completely different environmental noise.

## Identifying

When identifying techniques it is critical to focus our identification on essential and immutable elements. If we use tangential elements or those an attacker controls, then our identification accuracy plummets and the probability of detecting a given malicious instance drops with it. Elements that an attacker controls are likely to be changed precisely in the instances that we are most likely to be interested in.

By way of example, let's look at using a WMI Event Subscription (T1546.003) for persistence. Here's a model of how that technique works and some available telemetry (Sysmon). It's a simple technique: you create an event consumer and filter, and a binding to connect them together. The binding has to come last (you can't bind something that doesn't exist).

<!-- Image: Diagram showing WMI Event Subscription process flow with Event Consumer, Filter, and Binding components -->

Our goal is to identify 100% of the times that an Event Subscription is created. If we focus on the command line parameters an attacker might use (which are both tangential and attacker-controlled, so your Spidey-sense ought to be tingling already), it's going to be impossible get to 100% accuracy.

<!-- Image: Diagram comparing identification methods - command line parameters vs Sysmon Event 21, showing infinite command line variations vs single essential telemetry point -->

In fact, it would take an immense effort to approach even 1% identification accuracy. This is due to the fact that there are literally infinite command lines an attacker can use to create a WMI Event Subscription. They could use existing scripts or tools, write their own script or binary, obfuscate the command line parameters, etc.

On the other hand, we could get almost 100% accuracy if we use Sysmon event 21. It is an essential and immutable step in the technique.

This example is a detailed illustration of what hopefully was already obvious: it's really important that we choose the right telemetry to identify events for a detection. Even if we can classify with 100% accuracy, we'll end up with a 1% chance of identifying a given attack technique if we take the wrong approach to identifying. **Accurate identification is absolutely critical to good detection engineering.** It is also difficult, and one of the areas where I see the most mistakes in public or commercial detection repositories. In another article, I'm going offer an analytic tool that helps to do it well: [detection data models](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051).

## Classifying

Once we've identified instances of the technique it is time to classify them. Techniques will fall into one of 3 classification categories: **Inherently Suspicious**, **Suspicious Here**, or **Suspicious in Context**

**Inherently Suspicious** is easy to classify. The vast majority of instances will be malicious, so we can simply assume that any given instance is malicious. These lend themselves well to detections. Things like dumping LSASS, accessing the ntds.dit, and encrypting and replacing shared files are **inherently suspicious**. No legit user should be doing them.

**Suspicious Here** events are also easy to classify. Without legitimate use cases in your given environment, you can also assume that any given instance is malicious. You should note, however, that it's possible a future legit use case might be introduced, moving the technique into the *Suspicious in Context* category. Things like an ActiveScript WMI event consumer or a particular [LOLBin](https://github.com/LOLBAS-Project/LOLBAS) might be **suspicious here**, meaning that in *this* environment, they are highly likely to be malicious because we don't employ them in any legit use case.

**Suspicious in Context** is much harder to classify. Because there are legitimate instances of the technique in the environment, classification requires distinguishing between a malicious and benign instance. For some techniques, this may be impossible to do with adequate accuracy, and the technique is better handled as a warning signal (to be coupled with other signals), rather than as a detection. Things like creating a new service or startup key, running a file remotely, or scanning the network are **suspicious in context**: they might be suspicious, depending on the context in which they take place, but there are also regular benign instances of them here.

## Don't Compete with Your EDR

In my experience, *inherently suspicious* techniques are the bread and butter of EDR companies. Because they are almost always malicious, the identify/classify problem collapses down to mostly an identify problem. EDRs have a definite advantage in collecting telemetry, since they (hopefully!) already have an agent on every one of your enterprise's endpoints. If the EDR's engineers can find a way to collect the right telemetry, they can detect these kinds of techniques with high confidence.

On the other hand, *suspicious here* techniques get harder for them, because they will likely have a large, diverse customer set. What is suspicious in one customer's environment might be business critical in another's, so they can't detect these techniques without potentially creating a lot of noise for some customers. *Suspicious in context* techniques become almost impossible for an EDR provider to detect. They might be able to provide useful telemetry, but there's no way they can raise an alert on these techniques.

There is no point in competing with your EDR. Remember, the [winning strategy is to cover as many techniques as possible!](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441) As a result, **detection engineers will likely get the most impact by focusing on techniques that are *suspicious here* or *suspicious in context.*** A detection engineer knows their environment best and is in a great position to be able to classify these kinds of events and create coverage where an EDR cannot. (That said, don't assume your EDR has coverage for an *inherently suspicious* technique! Run some tests to verify their coverage, and build a detection to fill any gaps you find.)

## Summary

In order to detect an attack technique, you have to first identify that an event of interest happened, then classify that event as malicious or benign. Both tasks are critical to effective detection; poor outcomes in one can't be balanced by the other. These tasks depend on the available telemetry and noise in a given environment, so they have to be custom tailored to each environment. Detection Engineers shouldn't compete with their EDR solution: they should determine what existing protections they have and fill in gaps. Gaps are most likely to exist with techniques that are suspicious in the context of their own environment, rather than those that are inherently suspicious (which your EDR vendor should be good at detecting, if they're worth what you're paying for them).

## Thoughts?

If you have any questions or thoughts to add, post a comment or let me know on [Twitter](https://twitter.com/_vanvleet)!

---

**Tags:** Threat Detection, Detection Engineering, Information Security

---

<!-- source: S3 The Relative Strengths of Threat (DetectionHunting).md -->

# The Relative Strengths of Threat (Detection|Hunting)

**Author:** VanVleet  
**Published:** January 23, 2024  
**Reading Time:** 8 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

This article is one in a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). Here, I'll attempt to disambiguate the terms "threat hunting" and "threat detection" and explore the areas where each practice has relative strengths.

## **Threat Hunting, Threat Detection, Detection Engineering?!?**

There are a lot of terms in the industry for the processes we use to prevent threats from impacting our networks. For me personally, there are 3 that seem to involve a lot of confusion about where one ends and the next begins: threat hunting, threat detection, and detection engineering.

A few definitions from well-respected industry sources (added emphasis is mine):

* "Cyber threat hunting involves **proactively searching** organizational systems, networks, and infrastructure for advanced threats. **The objective is to track and disrupt cyber adversaries as early as possible** in the attack sequence and to measurably improve the speed and accuracy of organizational responses." — NIST SP 800–172
* "Detection engineering is the **process of identifying threats before they can do significant damage.**" -Crowdstrike
* "Threat detection is the practice of **analyzing the entirety of a security ecosystem to identify any malicious activity** that could compromise the network." -Rapid7
* "Threat hunting is an **active** IT security exercise with the intent of **finding and rooting out cyber attacks that have penetrated your environment without raising any alarms.**" -Cisco
* "Threat detection is the process of **identifying threats in an organization** that are actively trying to attack the endpoints, networks, devices and systems." -Splunk

See any similarities there? No wonder we can't figure out which one is which! All three terms essentially mean "an effort to find undetected threats in a network as soon as possible." One could argue that the terms are practically synonymous.

There IS a distinction between Threat Hunting and Threat Detection, but the massive overlap between them stymies efforts to define them in a way that clearly differentiates. There are a huge range of activities and approaches that fall under both domains. If you like Venn diagrams (and I do!), here's what this one might look like:

<!-- Image: Venn diagram showing overlap between Threat Hunting and Threat Detection -->

I think it would take an immense effort to get the entire InfoSec industry to agree on a single definition for each, so I'm not going to attempt to draw a defining boundary today. My goal is to suggest an approach that leverages their relative strengths. But, because some kind of distinction is needed to compare and contrast them, I'm going to loosely define Threat Detection as the process of **building** **automated** **alerting** for threats, and Threat Hunting simply as **actively searching** for threats.

It is import to note that while threat hunting and threat detection are both capable of covering a lot of the same space, **they are two different approaches with different constraints, relative advantages, and outcomes.** When you are intending to create a detection, you need to be confined to data that you can collect, parse, enrich, and search in an **automated** fashion. **But threat hunting should not be constrained by the bounds of automation.** You can do things that can't be automated, like pulling data you don't normally collect, enriching it in ways you can't do at SIEM ingest, and analyzing it with a script that runs for a full week, then manually reviewing to see if anything just doesn't feel right. Threat hunting can go anywhere it needs to and take as long as necessary, that's what makes it powerful. To bind threat hunting with the same constraints as threat detection ties its hands and makes it less capable of accomplishing the very thing it excels at: sorting through anomalous activity to find the things that might've slipped through.

AND, if you ARE bounding yourself to the constraints of threat detection in the hopes that your hunt might lead to an automated detection, what you are doing is probably more closely aligned with threat detection. To mangle Shakespeare, "Threat detection by any other name will still ideally produce an automated detection." You would likely get better value from your efforts by following an intentional threat detection methodology, then revisiting your threat hunting efforts to release them from the automation constraints.

As for how Detection Engineering fits into the picture, I think it's the process by which Threat Detection is accomplished. I like [Florian Roth's definition](https://cyb3rops.medium.com/about-detection-engineering-44d39e0755f0) of it:

*"Detection engineering transforms an idea of how to detect a specific condition or activity into a concrete description of how to detect it."*

Detection Engineering is concerned with the best practices of Threat Detection: how to do it and how to do it well. I will therefore use the term "Threat Detection" to encompass both.

## What Threat Hunting|Detection Are NOT

I know it's bold to declare something the "wrong way," but I'm going to go out on a limb and argue that there are a few things commonly referred to as threat hunting or threat detection that really are wrong applications of the terms.

* **IOC "Hunting" —** Many public articles on Threat Hunting describe the IOA/IOC/TTP approach: you take a known indicator (be that a hash, URL, or ATT&CK technique) and you hunt in your network to see if it's present. This is a valid way to "actively search for threats" and would thus qualify as Threat Hunting. However, while valid under the loose definition, this kind of hunting is **inefficient.** If you can define what you're looking for as concretely as a hash, URL, or technique, and you can find a way to confidently determine if it's present in your network, you really should be creating an automated detection to alert you whenever it turns up. But if you build an automated detection for it, at that point it's most accurate to describe the process as Threat Detection. And if you put in all the effort to determine if a given IOC/IOA/TTP is present or not at a given time without then creating an automated detection, that's best described as ineffective Threat Detection rather than Threat Hunting.
* **"Pulling the Thread" —** I cannot count the number of "Threat Hunting" vendor webinars and presentations I've attend that ended up being about responding to a suspicious event in your environment. I would argue that if you're starting with a known, suspicious event that ALREADY HAPPENED, you're not doing threat detection or threat hunting. You're doing incident response. Threat Hunting and Detection are about finding the things you DON'T already know about. Incident Response is the practice of dealing with those you DO.

## **Relative Advantage: Malicious vs. Anomalous**

Now let's focus in on Threat Detection and Hunting specifically. Given the overlap between the practices of Threat (Detection|Hunt), perhaps the best question is then "where are their relative advantages?"

* Threat Detection's relative advantage is for dealing with things (sub-techniques!) that we can define as likely **malicious** and build high-fidelity automated detections to alert us any time they happen. If we can't make a high-fidelity determination, we end up with many false positives threatening alert fatigue.
* Threat Hunting, on the other hand, doesn't require as much fidelity. It is perfectly comfortable with identifying **anomalous** activity, and then determining if that activity is malicious in nature. The process of investigating hunting leads doesn't have to be automatable, nor does it need to be high-fidelity. Threat Hunters, who should be experts in their own terrain, can explore anomalous findings and determine if they are caused by a network quirk, a misconfiguration or an attacker trying to stay out of sight. It is possible, for example, to hunt based on a hypothesis like "someone in our network is sending data to somewhere it doesn't belong." The hunter could pull network logs and begin to crunch data and figure out what is flowing where, and whether or not it belongs there. However, it would be VERY difficult to build an automated detection for that, because "where it doesn't belong" is subjective and requires a lot of home-turf expertise.

To put it another way, anything that can be determined with adequate confidence to be **malicious** in an automated way is most effectively dealt with through Threat Detection. Anything that can only be determined to be **anomalous** is best handled through Threat Hunting.

Another relative advantage is that Threat Detection provides continuous threat monitoring whereas Threat Hunting provides only a point-in-time check. Hunting might identify intruders or gaps that need to be fortified, but it might yield nothing. Either way, it is necessary to keep sending those scouting parties out, even to the same regions they previously scouted, to ensure nothing has changed since the last check.

Finally, Threat Detection lends itself to metrics and measurement better than Threat Hunting. It's much easier to demonstrate the value of increased coverage against known techniques than it is to demonstrate the value of a hunting foray that turns up nothing.

## **The Best of Both Worlds: The Relative Advantages Strategy**

We can get the best of both worlds by using each process where it has a relative advantage. Because of its strengths in continuous monitoring and easier metrics, the most effective approach is to handle everything we CAN through Threat Detection, and Threat Hunt the things that Detection isn't well suited to handle. Trying to represent this "relative advantages" strategy in our original Venn diagram would look something like this:

<!-- Image: Modified Venn diagram showing the relative advantages strategy - Threat Detection handling malicious activities, Threat Hunting handling anomalous activities -->

This strategy dictates a robust Threat Detection program working to identify and fill gaps for all known, definable **malicious** activities and providing continuous monitoring for them through automated detections. Alongside this Threat Detection program is a Threat Hunting program that actively searches for **anomalous** activities and determines whether the source is malicious in nature to make sure no one has managed to sneak through undetected.

## Summary

While both practices could be reasonably applied to a wide range of activities, Threat Detection holds a lot of advantages when a threat can be defined, declared malicious with reasonable confidence, and detected in an automated fashion. Threat Hunting excels in identifying anomalous activity that requires further investigation to classify as malicious or benign, and it is most powerful when freed from the constraints of automation. A Blue Team should apply both of them to the problems where they are most effective.

## Thoughts?

The world of Threat Hunting/Detection is pretty big! I'd love to hear thoughts or other ways of structuring things that you've found effective. If you have any thoughts to add, post a comment or let me know on [Twitter](https://twitter.com/_vanvleet)!

## **Credit Where It's Due**

Many of these ideas were clarified in my mind through numerous conversations with my exceptional colleagues Stephanie Copley, Steve Brawn, and Christopher Simpson.

## **Sources**

* https://www.crowdstrike.com/cybersecurity-101/threat-hunting/
* https://www.splunk.com/en_us/blog/learn/threat-hunting-vs-threat-detecting.html
* https://www.crowdstrike.com/cybersecurity-101/observability/detection-engineering/
* https://en.wikipedia.org/wiki/Cyber_threat_hunting
* https://cyb3rops.medium.com/about-detection-engineering-44d39e0755f0
* https://csrc.nist.gov/csrc/media/Publications/sp/800-172/final/documents/sp800-172-enhanced-security-reqs.xlsx
* https://sysdig.com/learn-cloud-native/detection-and-response/what-is-threat-detection-and-response-tdr/
* https://www.rapid7.com/fundamentals/threat-detection/
* https://www.cisco.com/c/en/us/products/security/endpoint-security/what-is-threat-hunting.html

*Originally published at [https://www.linkedin.com](https://www.linkedin.com/pulse/relative-strengths-threat-detectionhunting-andrew-vanvleet).*

---

**Tags:** Threat Hunting, Threat Detection, Information Security, Detection Engineering

---

<!-- source: S4 Compound Probability You Don't Need 100% Coverage to Win.md -->

# Compound Probability: You Don't Need 100% Coverage to Win

**Author:** VanVleet  
**Published:** September 5, 2024  
**Reading Time:** 6 min read  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

<!-- Image: Article header image - not accessible -->

This article is part of a [series on Threat Detection](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62). In this post, I'm going to use compound probability to show why you don't need to have 100% attack surface coverage to have a strong chance of detecting attackers in your environment.

In this article I'm building on previous topics, so you'll find it easier to follow along if you've already read the [other articles in the series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62), but here are some key points you'll need to know:

1. Defending a network can be described as a [game of probability](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441): how probable is it that an attacker will select a path from initial access to their objective that doesn't alert you to their presence?
2. Not all detections provide the same amount of attack surface coverage. The best detection is the one that provides the most incremental coverage.

## Compound Probability

First let's take a trip back to our years of high school math. Compound probability is the likelihood of two or more separate events happening. For example, the probability of rolling a one three times in a row. The formula for this is:

P(A and B) = P(A) * P(B)

So, the probability of **both** events happening is the probability of the first event times the probability of the second event. Using our example of rolling a one three times in a row:

P(rolling a 1) = 1/6

P(rolling a 1 three times) = 1/6 * 1/6 * 1/6 = 1/216

It's intuitive that as we add more events to the calculation, the likelihood of them all happening as desired decreases rapidly. If we want to get four ones in a row, the chances drop to 1 in 1296.

## Compound Probability in Threat Detection

Every time an attacker takes an action in your enterprise's environment, they are rolling the dice. They have an objective for each action: something they want to accomplish (persistence, lateral movement, privilege escalation, etc). They also have a finite number of techniques to choose from to accomplish that objective. When they pick a specific technique, they're taking a gamble that you don't have any detections in place to alert you to the action they're taking. If they are successful, they get to stay on the network undetected and continue towards their final objective. If they fail, you get an alert and start a response to kick them out. Putting this back into the [visual model](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441), **each attack technique they execute is a roll of the dice. The probability of their success is determined by the percentage of techniques that you have covered (for a given tactic).**

<!-- Image: Visual representation of attacker's path through techniques as dice rolls -->

They have to continue to roll the dice action by action as they forge a path through your network to their objective. While the odds might be in their favor for each individual roll, compound probability strongly favors the defender. The likelihood of guessing correctly over and over again gets smaller and smaller with every new roll, even if our attack surface coverage isn't impressive. For an example, let's assume an attack surface coverage of only 33% of the full attack surface, meaning we have detections or preventative policies in place for 33% of all the attack techniques in each tactic. That means an attacker has a 33% chance of failure and a 66% chance of success each roll. Further, let's imagine that an attacker can get from initial access to their final objective in just 5 actions (that would be an impressive feat in real life, but it serves well to demonstrate the point). The probability of an attacker choosing a technique we don't have covered 5 times in a row is:

P(choosing right 5 times w/ a 66% chance) = ⅔ * ⅔ * ⅔ * ⅔ * ⅔ = **13%**

Those odds are definitely in the defender's favor! And they just improve as our attack surface coverage increases. Let's explore how improving the attack surface coverage changes the odds. We'll use the above example of a 5 step attack.

* At 15% coverage, an attacker's odds are 44%.
* At 25% coverage, their odds drop to 24%.
* At 33% coverage, the odds go to 13%.
* At 40% coverage, the odds are just 7%.
* If we can reach 50% coverage, they have only a 3% chance of success.

The math also works in the defender's favor as we increase the number of steps an attacker needs to take in our environment, which is effectively the goal of many security practices like least privilege, network segmentation, zero trust, etc. Using a static 33% attack surface coverage:

* An attacker's odds of guessing right 5 times in a row is 13%.
* The odds of guessing right 6 times in a row is 9%.
* The odds of guessing right 7 times in a row is only 6%.
* The odds of guessing right 8 times is 3%.

Now, if we could get 50% coverage AND require the attacker to take 8 steps, their chance of success is a paltry .4%! Compound probability is a big ally to blue teams!

## Focusing on Attack Surface Coverage

This further illustrates the point that **detection engineers should focus their efforts on the detections that will increase their attack surface coverage the most.** In my [original article](https://medium.com/@vanvleet/threat-detection-strategy-a-visual-model-b8f4fa518441), I do some calculations on different types of detections and show how some provide considerably more detection coverage than others. Time and effort spent on producing as-comprehensive-as-possible detections will pay off as attack surface coverage increases. On the other hand, time and effort spent on low-coverage detections doesn't yield much when an attacker is rolling the dice.

It's probably a good time to note that estimates of attack surface coverage are always estimates. It would be a rare situation to be able to say you have 100% coverage of an attack technique (you'd need [100% identification and 100% classification](https://medium.com/@vanvleet/identifying-and-classifying-attack-techniques-002c0c4cd595), a high bar to reach!). However, as Luke Paine illustrated in a [recent article](https://posts.specterops.io/to-infinity-and-beyond-feab2d8ff93c), if we test enough of the known procedures, we can gain a good approximation of what our coverage likely is. Determining our real attack surface coverage requires understanding the possible procedures of an attack technique (for which a [detection data model](https://medium.com/@vanvleet/improving-threat-identification-with-detection-data-models-1cad2f8ce051) excels) and a robust testing program.

## Independent Events and Truly Random Choices?

For the sake of simplicity, we've treated the attacker's choices of techniques as independent events and a fully random choice (i.e. any of the available options are equally likely to be chosen). In reality, there are ways in which that isn't totally accurate. The most significant of these is the case of EDR detections.

A good EDR solution will provide significant attack surface coverage out of the box, which would appear to really tip the probability in your favor. However, due to the commercial nature of EDR solutions and their wide user base, attackers often have the opportunity to determine what techniques an EDR will detect before they use them on your network. So they can eliminate those techniques from their list of options before they make their selection. Thus, the decision isn't fully random: an attacker is way less likely to select a technique they expect your EDR to detect. In the case of attackers with VERY good OpSec (the [red team kind](https://www.blackhillsinfosec.com/wp-content/uploads/2021/03/SLIDES_OPSECFundamentalsRemoteRedTeams-1.pdf) of OpSec), this counter-balances the coverage provided by your EDR. The net effect is to narrow the range of options that they will choose from, and our probability game is played on a smaller field.

That doesn't mean your EDR's coverage is worthless, though. Not all attackers have good OpSec (some criminal actors don't seem to care much about OpSec at all). Also, a good EDR is always putting out new detections, so an attacker can't assume that it'll miss something today that it missed yesterday. Finally, a smaller field is still to the defender's advantage: it's easier to reach 50% coverage when there are less techniques to cover. This just means that, **in addition to having an EDR, blue teams need to create their own custom detections to fill gaps left by their EDR solutions. For attackers of a certain skill level, the real game of probability is played only in the space left by the EDR.**

## Thoughts?

If you have any thoughts to add, post a comment below!

---

**Tags:** Threat Detection, Detection Engineering, Infosec, Cybersecurity

---

<!-- source: S5 TTPI’s Extending the Classic Model.md -->

# TTPI's: Extending the Classic Model

**Author:** VanVleet  
**Part of:** [Threat Detection Engineering: The Series](https://medium.com/@vanvleet/threat-detection-engineering-the-series-7fe818fdfe62)

![VanVleet Profile](https://miro.medium.com/v2/resize:fill:64:64/1*dmbNkD5D-u45r44go_cf0g.png)

---

In 1926, Erwin Schrödinger introduced a new model of an atom. The previous "planetary" model, created by Niels Bohr in 1913, theorized that electrons moved around the nucleus in orbits of fixed size and energy. Schrödinger offered a more precise description of the movement of electrons, allowing it to model complex atoms that Bohr's model could not. Schrödinger's model became the foundation of modern quantum mechanics and is still widely accepted as the most accurate atomic model available. [Atomic Models — Compound Interest]

Models help us simplify, analyze, and explain complex real-life concepts. They are used everywhere and have been fundamental in enabling major advancements in our understanding of the world. But sometimes, models become an obstacle to advancement because they are too simple and limit our ability to account for the complexity of the thing they model.

In the InfoSec industry, we have some well-known models that help us to simplify, categorize, and analyze attacker behaviors. These include the taxonomy of "Tactics, Techniques, and Procedures (TTPs)" and David Bianco's ubiquitous "Pyramid of Pain." Like Bohr's atomic model, these models have enabled improvements in how we identify, categorize, and communicate attacker tradecraft. But, also like Bohr's model, they simplify too much and have become an obstacle to further progress. It's time for more precise models.

## Tactics, Techniques, and Fuzzy Procedures

Much ink has been spilt on defining TTPs. If you're unsure what they are, Robby Winchester wrote an excellent article that harks back to the DoD definitions, which is where the concept originated.

I think everyone is pretty clear on tactics and techniques, so I'm going to use simple definitions:

**Tactic** — A high-level grouping of actions that provide a specific benefit to an attacker.

**Technique** — A general method for achieving a tactic.

Note that both tactics and techniques are abstract concepts: high-level, without any specific implementation details. They are extremely useful for categorization and analysis, but you can't do or detect a tactic or technique. Nothing is concrete until you reach the procedure level.

Procedures are where the concept gets fuzzy, and that's because they do all the work that happens below "technique." All of the real-world usage of attacker tradecraft is encompassed in the P in TTP. In an article titled "What is a Procedure?," Jared Atkinson points out that attacker tradecraft can be described in at least six layers of abstraction, yet the TTP taxonomy offers only three. And all of the compression happens at the Procedure layer. In the TTP model, Procedure is doing a lot of heavy lifting!

## What IS a Procedure?

So, what IS a procedure? Going back to Robby's article, the DoD describes them as "standard, detailed steps that prescribe how to perform specific tasks." Jared offers an excellent clarification: "The procedures are the pattern of steps to execute, not the execution of the steps." He gives this example of a procedure for the technique of dumping credential from lsass.exe:

1. Determine the process identifier for lsass.exe.
2. Open a handle to lsass.exe with at least the PROCESS_VM_READ access right.
3. Read the memory of lsass.exe.

The procedure is the recipe for a specific method of dumping lsass.exe to obtain credentials. Running with this cooking metaphor, the procedure is the recipe, not the cake that it produces. And there might be other recipes that do it differently but produce the same kind of cake; these represent other procedures.

I am an adherent of Jared's definition of procedure, but in discussions with colleagues throughout the industry, I've realized that other interpretations of the term are in use and, under the current TTP model, are equally valid. For example, MITRE's ATT&CK framework lists "Procedure Examples" for each technique, all of which are a single, specific instance of attackers or tools using the technique. In other words, it's a list of cakes, not recipes. (In fact, there IS no list of recipes for any given technique currently in existence. That's one goal of the TRR Library). Shortly after embarking on a discussion of detection engineering with a new colleague, I find myself needing to clarify which definition of 'procedure' they are using: recipes, cakes, or a combination of both?

I believe that a big part of this problem is that our current model is too simple. The TTP taxonomy stuffs both cakes and recipes into a single layer. No wonder we find ourselves struggling to distinguish between them!

## TTPI — Adding the Instance Layer

To address this problem, I propose extending the TTP taxonomy to include a new "Instance" layer. This new layer separates recipes from cakes:

**Tactic** — The goal an attacker wants to achieve. (Feeding people)

**Technique** — A general method for achieving a tactic. (Baking a cake)

**Procedure** — A unique pattern of detailed steps to accomplish a technique. (The cake recipe)

**Instance** — A concrete implementation of a procedure. (The cake)

To illustrate this new taxonomy, I'm going to break down the technique for clearing Windows event logs (T1070.001). (I'm going to assume a lot of knowledge about this technique, if you need more background please see the TRR on it.)

### Clear Windows Event Logs (T1070.001)

**Tactic:** Defense Evasion

**Technique:** Clear Windows Event Logs

**Procedures:**

- An attacker can clear event logs using the MS-EVEN or MS-EVEN6 RPC methods.
- An attacker can clear event logs by redirecting them to attacker-controlled files via the registry.
- An attacker can clear event logs by killing the EventLog service and deleting the log files.

**Instances:**

- Executing 'wevtutil cl system' at the command line.
- Using the PowerShell 'Clear-EventLog' cmdlet at the command line.
- Using the PowerShell 'Clear-EventLog' cmdlet in a script that is downloaded from an Amazon S3 bucket and executed via the 'iex' cmdlet alias.
- Calling the EvtClearLog() Windows API in a binary named 'erase.exe.'
- Running a VB script in an Office document macro that calls WMI's ClearEventlog() method.
- Running a script named 'foo.ps1' to terminate the svchost process hosting the EventLog service and delete all files in '%SystemRoot%\System32\winevt\Logs\*.evtx.'
- …. (MITRE lists 41 instances, and that's just the tip of the iceberg)

## Of the Finite and the Infinite

The TTPI model yields some immediate benefits by allowing us to distinguish procedures from instances. One thing that becomes immediately clear is that procedures (again, the recipes) are finite. There are only so many ways to get the operating system to do something specific.

Procedures do change, but at a slow rate. Updates to operating systems and platforms render old procedures inoperable (Credential Guard killing LSASS dumping, for example), while new platforms or technologies introduce new ones (AI conveniently collecting and serving up Credentials from Password Stores [T1555], for example).

Instances, on the other hand, are categorically infinite and can change rapidly. Adversaries have moved to living-off-the-land, lighter weight tools and scripts, and malware-as-a-service to constantly refresh their instances and stay ahead of instance-focused detections and signatures. And the era of GenAI will make that even easier.

One other observation is that very instance maps to a single procedure, and many instances can implement the same procedure. For example, the wevutil.exe utility, PowerShell's Clear-EventLog cmdlet, and WMI's ClearEventLog() method are all different instances, but they all implement the exact same procedure: they call one of the MS-EVEN or MS-EVEN6 RPCs.

So, procedures are finite and slow to change, instances are infinite and fast changing, and all instances employ one procedure. Hold that thought.

## Revisiting the Pyramid of Pain

Let's take our new TTPI taxonomy and use it to refine David Bianco's "Pyramid of Pain."

<!-- Image: Original Pyramid of Pain diagram by David Bianco -->

The pinnacle of the pyramid is "TTPs." David explained this level by saying "When you detect and respond at this level, you are operating directly on adversary behaviors, not against their tools." The detection engineers I've talked with universally agree that the ideal is to detect these "TTPs" at the top of the pyramid.

The challenge is that tactics and techniques are abstract and undetectable, and procedures (the 'TTP' version) encompass both recipes and cakes. That leaves a lot of gray area: detecting both recipes and cakes can be considered detecting "TTPs".

We see this confusion in the many public detection repositories: detections for both recipes and cakes abound (the latter being more abundant than the former, in the author's opinion).

I'd like to offer a refined pyramid, which I'm going to call the **Pyramid of Permanence**.

<!-- Image: Pyramid of Permanence diagram - refined version with TTPI taxonomy -->

This pyramid stacks elements by ease of modification or replacement, just like Bianco's pyramid of pain, but our new TTPI taxonomy allows us to greatly simplify it: Procedures (using the TTPI definition) are the pinnacle. Everything below that is just an element of an Instance.

When it comes to detecting attacker tradecraft, we still focus on the pinnacle. When you take an instance away from an attacker, they just create a new one. But when you take away a procedure, they can't just create a new one. They have to operate in a smaller space.

MITRE's Summiting the Pyramid defines levels of analytic robustness for scoring a given detection that align neatly with this pyramid model. Levels 4 and 5 define detections that are focused on procedures, while levels 1–3 focus on instances. (Side note: whether a detection can reach level 5 will often come down to how different the procedures are. For cases where the procedures are drastically different, like credentials from the NTDS.dit, multiple level 4 detections are the highest you can achieve, and together they would comprise a level 5 detection strategy.)

## Don't Chase Instances

The new models help clarify a valuable lesson: don't chase instances. Instances are infinite. Attackers are constantly making new ones, and no matter how fast we move, we'll always be chasing: them deciding where to go next and us having to follow along. You can't get ahead in an infinite space.

This doesn't mean there is no value in detecting instances. Low cost, high fidelity detections for commonly-used instances are absolutely worthwhile. There is definitely value in identifying and responding to oft-abused tools like CobaltStrike and Rubeus, for example.

But, the most impact is gained by denying procedures. Returning to that earlier thought: procedures are finite and slow to change, instances are infinite and fast changing, and all instances employ one procedure. Attackers must use procedures, yet there is a finite number of them. If we focus on detecting them directly, we detect all instances that use the procedure and force the attacker to abandon it or get caught. That's a game we can win.

## Conclusion

New models can facilitate new understanding. By extending the TTP and Pyramid of Pain models, we can more clearly target our detection engineering efforts where they'll provide the most impact. Share your thoughts in the comments, and if you agree share the article with your colleagues. Let's get clarity between those recipes and cakes!

---

**Tags:** Threat Detection, Detection Engineering, TTPs, MITRE ATT&CK