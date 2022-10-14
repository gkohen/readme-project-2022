# Autonomy vs Governance - The Delicate Balance of Power

In larger organization there seems to be a constant jealousy amongst the developer community of the nimbleness and autonomy we find, many times in smaller organizations allowing to move fast. However, as you want to align over two thousands contributors on best practices as well as to spur innovation throughout the organization you realize that being too top down will result in squashing experimentation and being too ground up will result in many local optimization missing the chance to build upon success of others and not having to reinvent the wheel.
In addition we wanted to make sure we keep the overhead of managing such an aligned organization as low as possible. 

## The Origin Story
Ay Blue Yonder we strive, like in many other organizations, to grow by while making sure we minimize these growth bottlenecks. That means we want to adopt the best in breed technologies while allowing us not to be experts in these technologies and instead, focus our best and and brightest efforts on Blue Yonder's raison d'etre: Provide the our customers AI-driven Supply Chain Platform.
That means that we aim for any staff providing support the the product/services teams, to manage as little services and tools as possible. Let me pause to clarify a point of invisible toil here. Many times there a misconception that companies can easily run their own open source deployments or hosted Docker/Kubernetes services. Although that is most times true where you can run these with relatively cheap cloud resources, we forget the operational overhead of running such services such as have staff available to manage, tune and support these now critical system. Moreover you need to do it in a follow-the-sun capacity. 
For many years we managed many of these systems internally. From code management, SAST(Static code scanning), DAST(Dynamic Code Scanning) to CI/CD workers and more. 
As we were evaluating the transition these support systems to cloud vendor managed we were looking at criteria such as staff familiarity(productivity is king), security, extensibility and scalability. The choice was pretty clear for us as all the tea leaves of our business goals were pointing us to GitHub.  

## Trouble in Paradise
Everyone knows GitHub right? Problem solved!?
Not so quick. Although knowing how to utilize GitHub for open source project or small organizations with private repositories, it becomes more complex as you're trying to drive best practices throughout the organization of thousands of people with many of the following aspects:
1.  Permissions over thousands over repos (multi-repo) or really complex mono repos.
1.  Drive the adoption of [DevOps methodologies](https://medium.com/@raycad.seedotech/devops-methodology-and-process-dde388eb65bd), [Trunk Based Development](https://trunkbaseddevelopment.com/) and [Clean Code](https://www.freecodecamp.org/news/clean-coding-for-beginners/)
1. Adhere to regulations such as SOC 2 and SUX.
1. Drive innovation
1. Make it easy/familiar for team members to move from one team to another to re-balance needed.
1. Improve the metrics of TTFMPR(just made that acronym). Meaning: time to first meaningful pull request of a new engineer in the team. Never underestimate the feeling of accomplishment by newbies.
1. Achieve all of the above with a lean management overhead to prevent some of the operational management overhead described above.

Although, GitHub does provide organizational administration, it somewhat relies on the fact that you will have, for such a size of an organizations. Let's have a look at an illustration below that resembles the organizational structure of Blue Yonder's engineering groups. Note: There are many more product segments and product teams, but you get the idea.

```mermaid
graph TD
	DG[Blue Yonder] ---|BY Admins| 0[ ]
    style 0 width:0
    style DPA0 width:0
    0 --- DPA[Platform]
    DPA ---  |Platform Administrators| DPA0[ ]
	DPA0 --- DPAA[Pillar A]
    DPAA --- DPAAA[Team Bat]
    DPAA --- DPAAB[Team Antelope]
    DPAA --- DPAAC[Team Cheetah]

	DPA0 --- DPAB[Pillar B] 
    DPAB --- DPABA[Team Eagle]
    DPAB --- DPABB[Team Alligator]
    DPAB --- DPABD[Team Beetle]

	DPA0 --- DPAC0[Pillar C]
    DPAC0 ---  |Pillar C Administrators| DPAC[ ]
    DPAC --- DPACA[Team Eagle]
    DPAC --- DPACB[Team Alligator]
    DPAC --- DPACC[Team Raven]
    DPAC --- DPACD[Team Beetle]

    0 --- DPB[Product Segment A]
    DPB --- |Segment A Admins| DPB0[ ]
	DPB0 --- DPBA0[Product 1]
    DPBA0 ---  |Product 1 Administrators| DPBA[ ]
    DPBA --- DPBAA[Team Koala]
    DPBA --- DPBAB[Team Kiwi]
    DPBA --- DPBAC[Team Dingo]
    DPBA --- DPBAD[Team Dodo]
    DPB0 --- DPBP[Product 2]
	DPB0 --- DPBV[Product 3]
```

Imagine you want to organize some GitHub secrets that will be available to some groups but not others and every team might have many repos that will map to modules/services/apps. You also want to create reflect the hierarchy of the organization and make sure you can delegate to various parts of the the organization, admins that in turn can manage these specific parts or group of repos. We did look at concepts in GitHub such as projects, but these where not providing the hierarchy we needed but rather serve as a feature source of truth and project planning(For which we use Jira at Blue Yonder). The way teams are defined at GitHub did not give us the structure we needed and instead reflected inhabitance of permissions. 
Finally, we wanted to make sure that when someone creates a repository at Blue Yonder, it not only has the needed files(As GitHub template repos do), but they also reflect our best practices such forcing our flavors of branch protection, checks scans and designated teams for one or more repos. In out case we called it a team set: Admins/Collaborators/Contributors.
These abilities to tweak the administration to be low-touch and adhere to our best practices, does not come out of the box. Lucky enough, GitHub supports plethoras of APIs which allow you to re-imagine how your organization will look like and the secret weapon of GitOps.

## What is GitOps?

As some of you may have guessed, It's the combination of Git practices and Operations. More specifically the ability to manage your operations by defining the structures, configuration, infrastructure and interactions as Git operations with repositories. It heavily stole practices from the DevOps world([Thank you Picasso](https://www.creativethinkinghub.com/creative-thinking-and-stealing-like-an-artist/)).
That allows you to have define almost any structure you'd like, to define your organization as long as you can use the underlying API to reflect it (such as in the case of GitHub APIs). 
For instance, if you have one(or many) JSON/YAML files in such a management dedicated repository, the folks that want to contribute a change to which secret can be read by whom, which teams-sets should be created or assignment of lower levels admins, can be done through the same processes of code changes with pull request reviews and full auditing of such changes. Further, we can use mechanisms such as CI/CD to accept requests to create new repositories and we can generate repositories that adhere to out best practices, naming and the correct ownership. Further, anyone can see hwo these are implemented and suggest improvement to the process which goes to one of the [five ideals of DevOps](https://itrevolution.com/five-ideals-of-devops/): Improvement of daily work.

## How it's done at Blue Yonder?
