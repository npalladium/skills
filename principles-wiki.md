# Principles Wiki

> Source: https://principles-wiki.net/
> Scraped: 2026-05-05

---

# Principles

## Abstractions Live Longer Than Details [see GP]

*See: Generalization Principle*

---

## Add More Classes

### Variants and Alternative Names

### Context
- **Object-Oriented Design**

### Principle Statement

> "When things get too complex, add more classes."

### Description

Complexity can often be reduced by adding further classes that better separate concerns.

### Rationale

### Strategies

- Divide larger classes into several smaller ones.
- Use polymorphism, dynamic binding and abstract couplings.

### Caveats

- Adding too many classes leads to ravioli code. Which may be hard to grasp and debug.
- Classes need to be separately understandable (see **PSU**).

See section **#Contrary Principles**.

### Origin

Grady Booch is sometimes cited like that. The precise origin is unknown. See **Wiki>Addmoreclasses**.

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **High Cohesion** (HC): Add more classes is a direct result of HC.

#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): Add More Classes decreases complexity with respect to the size of the classes on expense of increased complexity due to the number of classes. There is always a tradeoff to be made. Add More Classes tells that an increased number of classes often is the lesser evil while MIMC explains the tradeoff to be made.

#### Complementary Principles

- **Encapsulate The Concept That Varies** (ECV): ECV tells how to separate concerns when add more classes shall be used to lower complexity.
- **Model Principle** (MP): While adding classes MP should be considered in order to find appropriate concepts for encapsulation into a class.

#### Principle Collections

### Further Reading

- **Wiki>Addmoreclasses**
- **Wiki>Fearofaddingclasses**
- **Wiki>Howtoavoidfearofaddingclasses**
- **Wiki>Minimumnumberofclassesandmethods**
- **Wiki>Raviolicode**
- **Wp>Ravioli Code**

---

## Constantine's Law

### Variants and Alternative Names

- Low Coupling, High Cohesion

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**

### Principle Statement

> "A structure is stable if cohesion is strong and coupling is low."

### Description

This principle is a combination of the two principles **Low Coupling** and **High Cohesion**.

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence

- **Examined**
- **Accepted**

### Relations to Other Principles

#### Generalizations

- **Low Coupling**
- **High Cohesion**

#### Specializations

- **Information Expert**

#### Contrary Principles

- **Keep It Simple Stupid**: Following this principle often makes the design more complicated.

#### Complementary Principles

- **Model Principle**

#### Principle Collections

### Further Reading

- Albert Endres and Dieter Rombach: *A Handbook of Software and Systems Engineering*. p. 43pp.
- Glenford J. Myers: *Reliable Software through Composite Design*
- **Wp>Loose Coupling**, **Wp>Coupling (Computer Programming)**
- **Wp>Cohesion (Computer Science)**
- **Wiki>Couplingandcohesion**

---

## Crash Early [see FF]

*See: Fail Fast*

---

## Curly's Law [see SRP]

*See: Single Responsibility Principle*

---

## Dependency Inversion Principle (DIP)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Api Design**

### Principle Statement

Depend on abstractions.

### Description

A simplified description of DIP is that a variable declaration should always have the (static) type of an abstract class or `interface`. By doing so a module depends only on this abstraction. The concrete subclass realizing the details is referenced only once, namely when it is instantiated.

The more elaborate definition by Robert C. Martin reads as follows:

> a. High-level modules should not depend on low-level modules. Both should depend on abstractions.
> b. Abstractions should not depend on details. Details should depend on abstractions.

Following this rule leads to "inverted" dependencies compared to classical procedural approaches. The following diagram shows the classical approach. A high-level module `A` uses a low-level module `B`.

When applying DIP, both modules depend on the abstraction (note that in UML diagrams all arrows point into the direction of the dependency):

`B` is not depended upon anymore but it depends on another module. This is the inverted dependency.

### Rationale

When DIP is not applied, only the low-level modules can be reused independently. The higher-level modules depend on the others, so trying to reuse them makes it necessary to either also reuse the lower-level modules or to change the higher-level module. The former is often not wanted because reuse is often done in another context where the lower-level modules do not fit. And the latter is error-prone and requires additional work as it requires changes to already working modules.

### Strategies

- Apply the **Dependency Inversion** Pattern
- Use events, the observer pattern, etc. to remove dependencies
- Apply other forms of **Dependency Inversion**
- Have an `interface` type for every class
- Declare only `interface` types so that an object variable generally has an `interface` as static type and a concrete class as dynamic type
- Do not derive classes from concrete ones (i.\,e.\ non-abstract classes)
- Do not override already implemented methods in subclasses

### Caveats

It is normally not helpful to apply DIP to **Value Objects**.

Furthermore note that applying **Dependency Inversion** pattern (see **#Strategies**) introduces an abstraction. FIXME explain Super-A vs. ReqByB

See section **#Contrary Principles**.

### Origin

### Evidence

**Status: Accepted** — DIP is part of the well-known **Solid** principle collection.

### Relations to Other Principles

#### Generalizations

- **Low Coupling** (LC): LC aims at reducing the dependencies to other modules. One way to do so is to only depend on abstractions. DIP is about this aspect.
- **Dependency Abstraction** (DA) : While DIP is about inverting dependencies going from lower to higher architecture layers, DA also works on the same layer where an "inversion" is not helpful.
#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): DIP demands introducing abstractions, especially abstract classes or interfaces.

#### Complementary Principles

- **Model Principle** (MP): DIP demands having abstractions. MP tells how these abstractions can look like.

#### Principle Collections

### Examples

#### Example 1: Furnace

An example for a high-level module is a regulator module of a furnace. The classical approach would result in the regulator depending on a thermometer and a heater. in such a case it would not be possible to reuse the regulator module for regulating the fluid level of a reservoir or the speed of a car. A DIP-compliant solution would result in the regulator just depending on a sensor module and an actuator module and thermometer and header implementing these `interfaces`. By doing so thermometer, heater, and regulator can be reused independently.

This example is taken from  and slightly modified.

#### Example 2: Client Repository

Let's say the high-level module (your business logic), wants to be able to add or remove users to the database. Instead of it talking to the database directly, it defines an interface called ClientRepository which contains the methods the business logic needs. Then a MySQLClientRepository concretion, implements that interface and uses a database library to submit the queries. Since the interface is decided by the business logic, the high-level policy is protected from changes in the database library. More over, since the interface was defined by the business logic, it does not reveal anything about the underlying implementation, which allows different types of user repositories, such as a WebserviceClientRepository implementation (**OCP**). Finally the MySQLClientRepository and business logic can be built as well as deployed independently.
### Further Reading

-
-
-
- **Wiki>Dependencyinversionprinciple**
- **Wp>Dependency Inversion Principle**

---

## Direct Mapping [see MP]

*See: Model Principle*

---

## Do It Myself [see IE]

*See: Tell Don'T Ask/Information Expert*

---

## Do One Thing [see SRP]

*See: Single Responsibility Principle*

---

## Don't Repeat Yourself (DRY)

### Variants and Alternative Names

- Single Point of Truth (SPOT)
- Single Source of Truth (SSOT)

### Context

- **Object-Oriented Design**
- **Implementation**
### Principle Statement

> "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

### Description

DRY not only states that code duplication shall be avoided. Rather DRY is a general rule that states that if there is duplication, there shall be some "single source of truth". Also when one piece of information has several representations (like an object structure corresponding to a database schema) DRY demands one and only one representation being the definitive one. The other representations have to be generated automatically. The "one and only" representation can be one of the used representations or alternatively a third one.

### Rationale

If there are several representations of the same information (be it code or any other form of information), all of them have to be maintained separately while changing at the same time. There is the danger that at some point in time the different representations diverge which is a fault. But if there is a single source of truth, there is only one place where changes have to be applied. Then the representations cannot diverge.

### Strategies

- Add a new invokable module (a function, a method, a class, etc.) instead of duplicating code
- Factor out a common base class
- Use code generation when information has to be represented in multiple forms
- Use polymorphism to avoid repeatedly enumerating a set of possible solutions in if or switch statements

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence

- **Examined**: There is extensive research on code cloning, its reasons, automatic detection of code clones, their evolution, etc. In  Chanchal Kumar Roy and James R. Cordy present a 115 page survey on the state of research as of 2007. Many unanswered questions remain and research is still ongoing. In 2009 Juergens et al.  analyzed fife systems written in C#, Cobol and Java each between around 200 and 500 kLOC. They identified clones and intentionally and unintentionally inconsistent changes to them. They then related the faulty clones to them and came to the expected conclusion that clones are changed inconsistently and that this results in faults. So at least the part of DRY about code duplication is supported by research findings.
- **Accepted**: It is generally agreed upon that code duplication is to be avoided. On the other hand the broader meaning of DRY which results in the heavy use of code generators is often not considered. On the other hand *The Pragmatic Programmer* is a well known book which makes DRY a well-known and accepted principle.

### Relations to Other Principles

#### Generalizations

- **Murphy'S Law** (ML):Duplication is a typical example for error possibilities. In case of a change, all instances of a duplicated piece of information have to be changed accordingly. So there is always the possibility to forget to change one of the duplicates. DRY is the application of ML to duplication.

#### Specializations

- **Once And Only Once** (OAOO): This is the aspect of DRY which is concerned with the avoidance of (code) duplication.
- **Single Choice Principle** (SCP): Duplication can also be duplication of information about a set of possibilities. The SCP is about this aspect.
- **Write Code That Writes Code**: This is the code generation aspect of DRY.

#### Contrary Principles

- **Keep It Simple Stupid** (KISS): Especially code generators can be very complex.

#### Complementary Principles

- **Generalization Principle** (GP): A generalized solution avoids duplication.

#### Principle Collections

### Further Reading

- **Wiki>Dontrepeatyourself**
- **Wp>Don'T Repeat Yourself**
- [97things: Don't_Repeat_Yourself](http://programmer.97things.oreilly.com/wiki/index.php/Don't_Repeat_Yourself)

---

## Don't Talk To Strangers [see LoD]

*See: Law Of Demeter*

---

Why healthcare reform? Individuals claimed that the cost this would definately be less and would give better services. Can estimated how the healthcare bill will cost trillions. When you've got look in the overall picture you can see that involved with not financially sustainable. You will have to pay hundreds of dollars before it actually starts to pay for your healthcare. Government is inefficient in everything it manages. Private Enterprise does a much better job.

There are legion times when despite all of the precautions, you fall ill. It is therefore important folks have the ability to get treated at such times. Family healthcare is beans are known the most elementary needs of man. People decide to seek treatment as soon as 1st signs and symptoms associated with the illness make an appearance. It important that people are able to obtain the required medication and cure for their problem just as trapped lest 4 to 5 turn into something far worse. Often minor problems could generated major varieties. When family members try keep clear of going together with doctor, or getting treatment, it is principally because cost of of treatment too high for them to bear.

Nurse - Nurses can work in just about any local weather. They can work in doctor's offices, hospitals, and establishments. They can even work at schools. Nursing is a field with unusually high job security, good hours, and a pay that starts at $30,000 and upwards.

Physician Assistant - Being a PA is really a great to be able to get a high-paying job (think over $70,000 a year!) while getting great job stock. In order to be a PA, you'll need a Bachelor's degree, inside addition to other educational. Physician Assistants can help diagnose and prescribe medication to patients, making them one on the most necessary additions to the practice.

So be sure to vote those out who are in congress that do not care exactly what the American assume. So when it comes a person to vote in November make sure you do investigation and support those representatives who will vote for the common sense majority in this country. Remember to express you patriotic views and pick vote this November.

You: OK, well I am aware that sometimes people get yourself a bit nervous when they call for their first acupuncture treatment so this is what I'll do for that you. we have a free report means chose the nice acupuncturist your own own or entire family and before you can be I'll send that for you through the mail so you've an regarding what to seek for in an Acupuncture clinic, does that appear sensible?

U.S. food manufacturers know these complications, but care more about churning income than consumer health. As being a consequence, they search for your cheapest manufacturing techniques and apply the kids. They have found pumping high fructose corn syrup and other sweeteners in the food we eat cuts the associated with production. In turn, they zap it into foods not Osteopathic clinic related to sugar cheerful.

Don't ever lose sight of one fact. An individual have learn and perform the right type of marketing, can perform only call your business increase more and other over days. It may not happen right away, but these begin seeing things change quicker than you may realise. This will be a point as it may help you boost your chances of having a thriving practice.

There are times whenever a person provides the option to obtain treatment that he/she needs, but that you simply can to simultaneously they prevent the same. Operating further hassles. Since there is no means of predicting of cheap checks faced, one cannot tell the cost needed to handle with liquids. The cost of healthcare is ever increasing. As time passes by this costs are going to arrive up beyond any doubt. It is in order to stay shielded from such high costs, to ensure there is very little compromise on family click through the up coming page and at the same time no force on the earning members.

Heart disease presents a substantial drain on our economy. And, unfortunately, under our current [click through the up coming page](https://rentry.co/61918-a-comparison-and-some-national-healthcare-questions) system, treatments are governed with a lot of really bad science. But, there actually are a few simple dietary supplements and/or essential oils that, along with wise life-style choices, could all but eliminate it. Again, the cost would be a fraction of what we're spending now, and we wouldn't in order to be deal with the devastating problems of the medications.

If encounter mild or medium back pain the best way to relieve your back pain is to rest. This form of pain often due about exertion of the muscles and joints end up being easily be relived the rest. Greatest to lie down on cargo area or regarding carpet in your back to your floor and the pillow beneath your knees. You may also rest your back by placing your feet on a chair or stool. Need to know make sure your knees bend properly approximately on the 90-degree understanding. You can also place a rolled towel under your neck for support.

A healthcare plan is going to be passed. Obama's future rrs dependent upon it. Rrt'll be the end of his second term before knowing if operates or not considered. The promise of insurance on the 31 million people currently uninsured, will not be fulfilled for months and months. What if the plan does perform?

---

Now that healthcare reform is almost reality, you're ready identify details and nonwinners. Overall, I feel the nation can be a loser within the one. Big government programs usually result in massive corruption, mind-boggling inefficiencies, and shameful amounts of waste. And after this 宮崎市 整体院 we're it will our healthcare to arrange? Yikes! Drilling down into the specifics we find the real winners and losers. It is interesting that the uninsured are these!

If you're watching the Democratic primaries you probably know that each of the candidates have introduced their universal healthcare policies. Those who are in Christ Jesus need observe this very clearly. You will find there's much better healthcare system, because our leader isn't subject alter because belonging to the false accusation, or an exit poll change. Our leader has given us His policy where health is situation. The blessing of His redemptive name Jehovah Rapha (I Am the Lord theat Healeth Thee) was provided in the Atonement. Has been created His first promise to heal.

You can either become a doctor, nurses, surgeon and more. These are high level jobs that you intend to require a lot of education and also training. But there are jobs in do n't have to study so hard but still you can certainly be a part belonging to the pharmaceutical Acupuncture clinic industry.

I listen up to progressive talk television and radio. Most of the following people are smart then get a huge commitment to the disadvantaged. Would like healthcare passed at every cost. They won't consider, however, the possibility that a government program could lose money. That it might be a bad idea. Going without shoes will make a situation entire lot worse. That is, unless it's a conservative president that has taken us appropriate war. Only then will an exit strategy be demanded.

After enjoying the problems and seeing the eating habits study a broken system, he realized that business knowledge and experience is a good way Acupuncture clinic fully grasp the woes of the healthcare system and will lead method on how you can eventually remedy it.

After that, 宮崎市 整体院 cleaning needs to be focused along the different issues the individuals will handle or touch. Regarding cases, which means the plates, bowls, and flatware they will eat having. These all need to be washed as soon as an individual is done utilizing them. This is because the who used them last should have gotten their own germs onto the items, may then be passed to the next person generally if the items are dirty. This could spread sickness all the actual years facility.

Hospitals some other medical locations need always be able to see how well they do. Performance monitoring can be carried out using healthcare reporting systems, so that any areas that wish to be improved can be quickly located.

Our birthrate is now lower in comparison to previous Osteopathic clinic very long time. This means that there are fewer folks coming along to pay for the cost for people like us as we age. Often seen the figures about you'll likely folks paying into government programs shrinking. All the while, the associated with us receiving benefits is rising. Does the Baby Boomer Generation ring a bell? Yet we are better off than many other countries facing the same challenges.

I have observed over time that examination actually government program has begun it can not be reduced or absent. Once people already been hired to government deals, they won't be dismissed. Big buildings often be constructed to store thousands of bureaucrats who will have better pay and benefits than their counterparts in an individual can sector. [宮崎市 整体院](https://Seven.mixh.jp/answer/question/how-to-propose-healthcare-services-which-get-funded) represents 1/6th of our economy but it is going to be moved into the hands of government. Can you imagine it upward being an unhealthy idea? Decreased is for sure, there will be no turning back again.

Using these numbers, salvaging not unrealistic to place annual increase of outlays at a regular of 3%, but actuality is definately not that. For the argument until this is unrealistic, I submit the argument that the regular American in order to live that isn't real world factors from the CPU-I plus it doesn't is not asking considerably that our government, may funded by us, to be within those same numbers.

If are usually looking around your cat's eyes, you'll need to see that it is pink around his eyes. The cat that comes with an issue may have discharge coming from his Osteopathic clinic focus. The discharge become happening like a result of an allergic reaction or additional infection. If ever the kitty is keeping one eye closed, that might be an issue too.

So make sure to vote those out in which in congress that don't care what the American people want. So when it comes time for vote in November be certain to do investigation and support those representatives who will vote for your common sense majority in this country. Remember to express you patriotic views and pick vote this November.

---

## Explicit Is Better Than Implicit (EIBTI) [see RoE]

*See: Rule Of Explicitness*

---

## Fail Fast (FF)

### Variants and Alternative Names

- Rule of Repair
- Crash Early

### Context
- **Implementation** /* FF is more about implementation than design */
- **Api Design**

### Principle Statement

A design is better when fails fast, i.e. as soon as an unrepairable erroneous condition is encountered.

### Description

Check for erroneous conditions like wrong parameter values, unmet preconditions, violated invariants, etc. In case of methods this means that it checks for errors and reports them for example by means of throwing an exception.

### Rationale

When a failure remains undetected, it propagates through the system ultimately causing other modules to fail. This results in in a more complicated fault removal. Furthermore undesired side effects like corrupted files may occur. A crashed program clearly communicates that there is a problem and is often a better situation than a misbehaving program.

### Strategies

- Check input parameters for validity -- especially non-nullness.
- Throw an Exception.
- Use assertions.

### Caveats

FF reveals problems which are already present in the system. For a system with only a few problems, this is good as the remaining faults are identified and fixed more easily. But applying FF to a system that has many problems may decrease reliability further as problems which were hidden, show up, produce error messages and lead to system aborts.

See also section **#Contrary Principles**.

### Origin

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

#### Complementary Principles

- **Postel'S Law**: While FF is (amongst others) about checking for erroneous parameters, Postel's Law is about not being too strict with parameters. It says that the design should allow for uncommon or strangely arranged (yet meaningful) input data. This does not contradict FF as Postel's Law does not demand to process meaningless or erroneous data.
- **Principle Of Least Surprise** (PLS): FF is about what a module should do in the case of error. PLS on the other hand is about how the module should behave normally. Furthermore it normally is not a surprise that a module fails when there is an error but a module that doesn't fail when it should, behaves strangely.
- **Murphy'S Law** (ML): Even better than failing fast is to make errors logically impossible. ML is about this.

#### Principle Collections

/***Incomplete***7

### Further Reading

- Eric S. Raymond: *[The Art of Unix Programming: Rule of Repair](http:*www.catb.org/~esr/writings/taoup/html/ch01s06.html#id2878538)//
- Andrew Hund and David Thomas *[The Pragmatic Programmer](http:*pragprog.com/the-pragmatic-programmer/extracts/tips)//
- **Wiki>Failfast**
- **Wp>Fail-Fast**
- Jim Gray: *[Why Do Computers Stop And What Can Be Done About It?](http:*citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.110.9127)//
- Joshua Bloch: *[How to Design a Good API & Why it Matters](http:*www.infoq.com/presentations/effective-api-design)//

---

## Fallacies of Distributed Computing

### Variants and Alternative Names

### Context
- **Architecture**

### Principle Statement

> Essentially everyone, when they first build a distributed application, makes the following eight assumptions. All prove to be false in the long run and all cause big trouble and painful learning experiences.
> 1. 	The network is reliable
> 2. 	Latency is zero
> 3. 	Bandwidth is infinite
> 4. 	The network is secure
> 5. 	Topology doesn't change
> 6. 	There is one administrator
> 7. 	Transport cost is zero
> 8. 	The network is homogeneous

So a design is bad if one these aspects is neglected.

### Description

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

Peter Deutsch: *[The Eight Fallacies of Distributed Computing](https:*blogs.oracle.com/jag/resource/Fallacies.html)//

### Evidence

- **Accepted**

### Relations to Other Principles

#### Generalizations

- **Law Of Leaky Abstractions**: Essentially the eight fallacies are abstraction leaks.

#### Specializations

#### Contrary Principles

#### Complementary Principles

#### Principle Collections

### Further Reading

- **Wp>Fallacies Of Distributed Computing**
- **Wiki>Eightfallaciesofdistributedcomputing**
- Peter Deutsch: *[The Eight Fallacies of Distributed Computing](https:*blogs.oracle.com/jag/resource/Fallacies.html)//
- Arnon Rotem-Gal-Oz: *[Fallacies of Distributed Computing Explained](http:*www.rgoarchitects.com/Files/fallacies.pdf)//

---

Aerial Photography involves taking picture shots or videos from a height, from an elevated surface with the equipment fixed on aircraft, helicopters, balloons, rockets and parachutes.

You can find the form on their website.

Trending Questions
What does a focus ring do on an SLR camera? Who designed the first Air Recon Camera for Aircraft? What parts of a Polaroid camera are not used anymore? What is the importance of photography to science lab technology student? What medium did ansel Adams use? What were the first cameras called? What skillls was used to make the camera what was used to create the camera? What is the highest tvl in CCTV cameras? Why CCTV cameras are monochrome? Where can one find information about the Polaroid 600? Why will my computer not let me download pictures from my camera powerShot A470 computer is windows 98? How do you get a timer on your ipad camera? Can you charge a sony handycam DCR-SX63 by a usb cable? Can you look at someone else through your computer camera? What is the world's smallest camera? Are there night vision dvr security cameras? Why do my camera videos show up with the screen half green on YouTube? Which piece of information would most help a historian create an interpretation of this photograph of autoworkers in a factory? Can i use a webcam like a security camera? How many different types of cameras have been invented?

Trending Questions
Why has a xsara Picasso 2 lt hdi engine have 2 oil filler caps? How do you change gearbox oil on xsara picasso 1.6 hdi? How do you change the [Dingli battery charger](https://awp-na.pages.dev/resources.html) on the citroen Picasso? Why does the windscreen washer fuse keep blowing on your citreon xsara Picasso? How do you Reset air bag light on Picasso? What fuse is the brake lights wired into on a Citroen Xsara Picasso? How do you fit a fan belt to a citroen xsara Picasso? Where is the speed indicator bulb on citroen Picasso hdi? What are the two switches for on the dash-right hand side on a citroen Picasso for? How do you change the front sidelight bulb on a citroen c4? How do you get to the horn on a citroen Picasso? Can you make a citroen xsara Picasso display speed in kph rather than mph? Where is the 3rd piston cut off solenoid located on a citroen xsara Picasso? How do you remove service reminder on citroen Picasso? How do the door mirrors fold on a Citroen Picasso 2.0 HDi? How do you change a rear indicator bulb in a xsara Picasso? How do you remove drivers seat on Citroen Picasso? How much oil does 1.6 petrol citroen xsara take? How do you change gear linkage citroen c3? What damage can be caused to front suspension of car due to coil spring breakage?

The loss of water from the aerial parts of the plants specially through the stomata is known as the "Transpiration".

The material on this site can not be reproduced, distributed, transmitted, cached or otherwise used, except with prior written permission of Answers. Copyright ©2026 Infospace Holdings LLC, A System1 Company.

The material on this site can not be reproduced, distributed, transmitted, cached or otherwise used, except with prior written permission of Answers. Copyright ©2026 Infospace Holdings LLC, A System1 Company.

Cattail plants in freshwater swamps are being replaced by purple loosestrife plants The two species have very similar environmental requirements This observation best illustrates? Does liverworts have a stem? What is a hosta like plant with a small blue flower? What other function do the root serve aside from adsorbing nutrients and water? Where does a plant get the energy to make starch? What is the process Derives energy from organic moleculs in the presence of oxygen? Trending Questions
When a plant dies what happens to its carbon? What is the relative position of the pistil and the stamen? Why is red clover a dicot? What are plants called that has covered or enclosed seeds? Hydrangea plants of the same genotype are planted in a large flower garden some of the plants produce blue flowers and others pink this can be best explained by what? Is durian tree a flowering plant or non-flowing plant? What does pistil do in a plant? Write down the names of flowering plants with pictures? What are the endengered spicies plants? What is the name of the sex cell in plants? How do you root a money tree plant clipping? How does plants habitat affect how it grows? What kind of plants can you live without?

Trending Questions
Does the Chevrolet astro van have real time all wheel drive? Where is the blower motor located on a 2001 Chevy Astro Vans? How do you open the fuse box on a 2002 Chevrolet van? How do you set the clock time on a 1997 Chevy Astro van? Where is the chassis number on 1988 astro van? Why would a 1998 Lumina suddenly stall while driving? How do you set the stations on the radio on a 95 Astro van? What could be the cause of a hummming noise while driving coming from the rear of a 2000 astro? How do you adjust timing on an 88 astro? Where is location of the blower relay on a 2002 Durango? Where is the temp sensor on a 91 Chevy Astro? Removal of the oil pan on 2003 Chevy Astro van? How do you remomove and replace the fuel pump in a 1996 Dodge 1500 318 magnum Van? How do you remove the fuel pump on a 1989 Chevy Astro Van? Popping noise in your 2000 Chevy astro van around the front passenger tire what could the problem be? How do you replace the ignition switch on a 2002 Chevy Astro van? How do i fix a rear door handle on a 1987 Chevy Astro? How are the dashboard lamps replaced on a 1990 Chevy Astro van? What is the [fuel capacity](https://www.gameinformer.com/search?keyword=fuel%20capacity) of 1994 Chevy G20 conversion van? About how much does it cost to repair the head gasket on a 1993 Buick Skylark?

---

## General Principle Of Robustness [see Postel's Law]

*See: Postel'S Law*

---

## Generalization Principle (GP)

### Variants and Alternative Names

- Build Generality into Software
- Abstractions Live Longer than Details
- Rule of Power (RoP)

### Context

- **Object-Oriented Design**
- **Api Design**
- **Architecture**
- **User Interface Design**
- **Implementation**

### Principle Statement

A generalized solution, that solves not only one but many problems, is better than a specific one.

### Description

There are various ways to make a solution more generally applicable. In the simplest form this can be done by introducing a method with appropriate parameters. Other possibilities are classes, parametric types, callbacks, hook methods, etc.

A general solution abstracts from the specific tasks and solves a superset of them. Parameterization of some kind is used to specify what has to be done in a given situation.

A module can be more general than another one. But there are two aspects of this: First of all there is functionality. If module `A` can do the same as module `B` plus something more then `A` is more general. The second aspect is the one of what has to be done in order to exploit the generality. An ideal case would be that nothing has to be done and the module just does more. Other possibilities are that a configuration file has to be changed, an attribute has to be set, an invocation parameter has to be adjusted, etc. The least general possibility would be a module which can be changed easily. This is still better than a rigid module but less general than modules which do not need such changes. This form of generality is often rather called "flexibility" .
### Rationale

Specific solutions tend to be fragile. When requirements change, a specific solution might not fulfill them anymore. In contrast to that a more general solution is more stable so there will be less need to change it.

Moreover a generalized solution can be reused in a variety of other situations. A specific solution can only be reused when exactly the same requirements appear again. So a general solution is much more reusable.

### Strategies
- Make modules configurable at runtime or deployment time by using configuration files.
- Use parameterizable modules(method parameters, object attributes, parametric types, etc.)
- Use constants
- Find suitable abstractions

### Caveats

Making a **Module** (typically a layer, a subsystem or an API) too general leads to the **Inner-Platform Effect**. This means that the module is so general that it mirrors the functionality of the underlying platform without adding a benefit but only complexity.

Another problem is the **Turing Tarpit**. This means that the module is so general that arbitrarily complex tasks can be performed but those of interest, meaning the rather simple tasks that occur over and over again, are also difficult to do. This is a violation of the **EUHM** principle.

See also section **#Contrary Principles**.

### Origin

The term "generalization principle" is proposed here. Nevertheless the value of generalized solutions is well known at least since:

### Evidence
- **Proposed**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- ****Keep It Simple Stupid** (KISS)**: A generalized solution is typically not simple anymore. This is the typical conflict between generality and simplicity.
- **Easy To Use And Hard To Misuse** (EUHM): Too general solutions may lead to complicated usage of the module.
- **Rule Of Explicitness** (RoE): RoE often results in specific solutions. Generality often requires stating something implicitly.
#### Complementary Principles

- **Don'T Repeat Yourself** (DRY): A more general solution avoids duplication.
- **Encapsulate The Concept That Varies** (ECV): Encapsulating a varying concept typically results in a more generally applicable solution. This is especially true when an abstract concept is encapsulated by introducing an interface or an abstract class.

#### Principle Collections

---

## High Cohesion (HC)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**

### Principle Statement

**Cohesion** in a **Module** should be high.

### Description

The cohesion of a module is a measure for how well the internal parts of a module (e.g. the methods and attributes of a class) belong together. Having a high cohesion means, that a module should only comprise responsibilities which belong together.

Several kinds of cohesion can be distinguished some of which are strong and some of which are loose. So strong forms of coupling should be preferred.
### Rationale

Not adhering to this principle, i.e. having a low cohesion, means that one module has several unrelated or only loosely related responsibilities. A change in the requirements for one of these may thus also affect the others which would not be the case in a highly cohesive module.

### Caveats

See section **#Contrary Principles**.

### Origin

W. P. Stevens, G. van Niekerk, G. J. Myers, L. L. Constantine: *Structured design*

### Evidence

- **Examined**: There are metrics that try to measure cohesion and there are studies relating these cohesion measures to the number of errors found during testing . This correlation is evident. The limitation of these studies is that these cohesion metrics cannot represent the cohesion notion completely.
- **Accepted** The concept of high cohesion is widely known and described in several well-known books for example in .

### Relations to Other Principles

#### Generalizations

#### Specializations

- **Constantine'S Law**: Constantine's Law is just the combination of HC and LC.
- **Single Responsibility Principle** (SRP): SRP is a stronger version of HC.
- **Interface Segregation Principle** (ISP): ISP is the application of HC to interfaces.

#### Contrary Principles

- **More Is More Complex** (MIMC): Making a module highly cohesive often results in additional modules. Sometimes it is simpler to assign a minor unrelated responsibility to a module, which lowers the cohesion.
- **Model Principle** (MP): Adhering to HC sometimes means to split up a class into several smaller ones which might correspond to the model less well.
- **Low Coupling** (LC): A system consisting of one single module has a very low coupling as there are no dependencies on other modules. But such a system also has low cohesion. The other extreme, very many highly cohesive modules, naturally has a higher coupling between the modules. So here a compromise has to be found.

#### Complementary Principles

- **Tell Don'T Ask/Information Expert** (TdA/IE): IE may help finding solutions with high cohesion. On the other hand it may also be disadvantageous in some cases (see **Tell Don'T Ask/Information Expert#Caveats**).
- **Encapsulate The Concept That Varies** (ECV): Adhering to HC often results in modules to be split up into several more cohesive ones. ECV gives further advice on how to do that.

#### Principle Collections

### Examples
The donkey is good. The site is not working

#### Class level cohesion

Robert Cecil Martin (Uncle Bob) describes maximal cohesion at class level as "a class in which each variable is used by each method".

"In general the more variables a method manipulates the more cohesive that method is to its class."
(Clean Code: A Handbook of Agile Software Craftsmanship - Chapter 10)

### Further Reading

- Albert Endres and Dieter Rombach: ***A Handbook Of Software And Systems Engineering***. p. 43pp.
- **Wp>Cohesion (Computer Science)**
- **Wiki>Couplingandcohesion**
-

---

## Information Expert [see TdA/IE]

*See: Tell Don'T Ask/Information Expert*

---

## Information Hiding/Encapsulation (IH/E)

### Variants and Alternative Names

- Parnas' Law

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**

### Principle Statement

Modules should be **encapsulated**.

### Description

Information hiding and encapsulation are sometimes seen as one and sometimes as two separate but related notions. This varies through literature. There are three stages of information hiding/encapsulation which can be defined as having a capsule, making the capsule opaque, and making the capsule impenetrable.

Having a capsule means that an object has methods which enable the client of the module to use it without accessing its internal data structures. Making the capsule opaque means that the inner workings are hidden from the clients. This is typically done by using access modifiers (private, protected). Lastly making the capsule impenetrable means that no client should be able to get a direct reference to an internal data structure.

A properly encapsulated module with an impenetrable capsule is better than an module with just an opaque capsule. And this is better than a module with a non-opaque capsule. But at least having a capsule is better than not having one at all.

### Rationale

When the inner workings of a module are hidden from the outside, then they can be changed without any other module noticing it. If the interface of the module stays the same, the rest of the system is not affected by the change. So adhering to IH/E prevents **Ripple Effects**.

### Strategies

- Use the lowest possible visibility for a variable or method
  - Make all attributes private and use getter and setter methods to access them
  - Better also avoid getters and setters
  - Find suitable abstractions for data types and use appropriate methods instead of just getters and setters
- Avoid aliasing problems with value objects
  - If the programming language supports that use call-by-value objects (like stack objects in C++, structs in C#, records in Delphi, etc.) for value objects like `Date`, `Money`, `EMailAddress`, `TelephoneNumber`, etc.
  - Otherwise use immutable objects which are handled call-by-reference but needn't be copied
- Avoid aliasing problems with lists and similar data structures
  - Copy internal list objects before returning them or only return a read-only `interface` to them

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence

- **Accepted**: Virtually every book on object-orientation (e.g.  explains IH/E to some extend.

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **Keep It Simple Stupid** (KISS): Not adhering to IH/E is often easier.

#### Complementary Principles

- **Model Principle** (MP): IH/E demands having an interface for a module which hides the inner workings. MP tells how such an interface can look like.
- **Liskov Substitution Principle** (LSP): For subclasses you can waken encapsulation by having a wider `protected` interface which can be used by subclasses. For these cases LSP has to be considered, too.
- **Tell, Don'T Ask/Information Expert** (TdA/IE): Encapsulation is about not having getter methods returning constituent internal parts of a module. TdA can be another reason for that.
- **Low Coupling** (LC): Higher forms of couplings (especially content couplings) break encapsulation.
- **Principle Of Separate Understandability** (PSU): IH/E is about constructing a module in a way that hides the inner workings so it can be used without knowing them. PSU on the other hand is about constructing a module such that its inner workings (and its usage also) can be understood without knowledge about *other* modules.
- **Easy To Use And Hard To Misuse** (EUHM): A module should be properly encapsulated in order to make it easy to use and hard to misuse.

#### Principle Collections

### Examples

#### Example 1: Date and Time

In Delphi there is the data structure `TDateTime` which represents a specific date and time value . This is an alias name for a double value where the integer part represents the number of days since December 30, 1899 and the fractional part represents the time of day. This alone is a data structure but it is not encapsulated.

The Delphi runtime library (RTL) now specifies functions which operate on `TDateTime` structures. This is "having a capsule". But since it is still possible to access the internal representation directly, the inner workings are not hidden.

This is different in Java. Here the inner workings are hidden. It is not possible to access the private attributes of `java.util.Date`. Here the capsule is opaque (and impenetrable).

#### Example 2: Aliasing

A typical example for an opaque but penetrable capsule is the following:

<code java>
class SomeClass
{
	private SomethingDifferent innerObject;

	public SomethingDifferent getInnerObject()
	{
		return innerObject;
	}
}
</code>

In such a case the `innerObject` is private, which means it is hidden. But it is revealed by the getter method. In order to establish an impenetrable capsule, the object has to be copied:

<code java>
class SomeClass
{
	private SomethingDifferent innerObject;

	public SomethingDifferent getInnerObject()
	{
		return innerObject.clone();
	}
}
</code>

### Further Reading

- **Wiki>Encapsulationdefinition**
- **Wp>Encapsulation (Object-Oriented Programming)**

- **Wiki>Informationhiding**
- **Wp>Information Hiding**

-
- **Wiki>Encapsulationisnotinformationhiding**

- **Wiki>Ondecomposingsystems**

---

## Interface Segregation Principle
### Variants and Alternative Names

### Context
- ****

### Principle Statement
Don't expose methods to your client, methods that they don't use.

### Description

### Rationale
Exposing functionality that the client class does not need at most a security risk and at the very least noise for the client class.
You don't want your class's clients to recompile if some method they don't use changed.

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

#### Complementary Principles

#### Principle Collections

### Examples

#### Coffee Maker
Let's say your coffee maker logic receives input from various sensors which are external components to it. Eg: tankOutOfWater() and waterHeated() from the WaterTank and HeatSensor components respectively. You don't want the WaterTank component to be able to access the waterHeated() method. You can do this, by making your business logic implement the interfaces TankObserver and TemperatureObserver, each exposing the methods that can be accessed by each component. Then the component only knows how to talk to the interface, so the rest methods of the concretion are transparent to it.

---

## Invariant Avoidance Principle (IAP)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**

### Principle Statement

Avoid Invariants and Preconditions.

### Description

Methods typically have preconditions. Something that has to be true prior to invoking the method so it can work properly. Typical cases are parameters that may not be `null` or have to be in a certain range. A solution is better the fewer preconditions there are.

Furthermore there are (class) invariants, i.e. conditions that have to be true in all observable states during the whole lifetime of an object. Typical invariants are attributes that may not be `null` or have to be in a certain range, lists that have to contain certain objects with certain properties, etc. A solution is better the fewer invariants there are.

While preconditions and invariants are absolutely necessary, introducing further ones comes at a certain cost.

Not that this principle does not apply to loop invariants, control-flow invariants, etc. as there is normally no chance to avoid them. But there can be fewer or more class invariants depending on the solution.

### Rationale

A typical kind of defect is the violation of an invariant or a precondition. The more preconditions and invariants there are, the more possibilities there are to introduce defects. And according to **Murphy'S Law** these possibilities will sooner or later result in defects. So it is better to avoid preconditions and invariants as this reduces the number of potential faults in the software.

### Strategies

- If the language supports that, use references which cannot be `null`
  - In C++ use references instead of pointers (see ** C++ References**)
  - In Java use primitive types instead of their object wrappers (`int` instead of `Integer` but not `int` instead of `Customer`)
- Use **Value Objects** instead of primitive types (see ** String Preconditions**)
- Avoid duplication of information. If the same information is stored in different places (maybe in different formats), the values may get out of sync (see also **DRY**). This also applies to caching.

### Caveats

Keep in mind that preconditions and invariants are absolutely necessary for every software. So this principle is constantly violated. Introducing preconditions and invariants is often also done deliberately in order to simplify the code (see **KISS**). So the purpose of this principle is mainly to point out that there are drawbacks. By no means invariants are problematic themselves or should be entirely avoided. They just also have disadvantages.

See also section **#Contrary Principles**.

### Origin

### Evidence

- **Proposed**

- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **Murphy'S Law** (ML): ML states that an invariant will eventually be broken. So IAP is the application of ML to invariants.

#### Specializations

#### Contrary Principles

- ****Keep It Simple Stupid** (KISS)**: Adding an invariant typically makes the code easier, as it can be assumed that the invariant holds. In fact that is often the very purpose if introducing invariants: Either they make the design easier or they are inevitable. Otherwise they should be avoided.

#### Complementary Principles

- **Information Hiding/Encapsulation** (IH/E): When an invariant cannot be avoided, it should at least be encapsulated.
- **Liskov Substitution Principle** (LSP): Not only the pure number and strength of invariants is relevant. The question is also which types in an inheritance hierarchy should have which invariants. Deriving `Square` from `Rectangle` for example adds an invariant in the subclass. LSP adds another point of view to this problem (see also **Rectangle-Square Problem**).
- **Fail Fast** (FF): Breaking an invariant is a defect. And in such a case the software should fail fast.
- **Don'T Repeat Yourself** (DRY): Duplication of information, like having the same data in different representations or like caching values, creates invariants. So an invariant sometimes is a hidden DRY violation.
- **Low Coupling** (LC): One type of precondition is that a specific method has to be called prior to another one. This also results in a temporal coupling.

#### Principle Collections

### Examples

#### Example 1: Index Preconditions

<code java>
public void prettyPrintItem(List<Item> items, int index)
{
  ...
}
</code>

This method has the following preconditions:
- `items` may not be `null`
- `index` must be greater or equal `0`
- `index` must be lesser than `items.size()`

Compare the following solution:

<code java>
public void prettyPrintItem(Item item)
{
  ...
}
</code>

This is better as it just has one precondition: `item` may not be `null`

#### Example 2: String Preconditions

<code java>
public void downloadFile(String url)
{
  ...
}
</code>

This method has the following preconditions:
- `url` may not be `null`
- `url` must contain a valid URL (which is even a quite complicated precondition)

Compare the following method:

<code java>
public void downloadFile(URL url)
{
  ...
}
</code>

This is better since there is only one precondition: `url` may not be `null`

#### Example 3: C++ References

Compare the following two methods:

<code c++>
void prettyPrint(SomeClass * obj)
{
  ...
}
</code>

<code c++>
void prettyPrint(SomeClass& obj)
{
  ...
}
</code>

In the second version `obj` cannot be `NULL` as it is a reference and not a pointer. So there is one precondition less.

#### Example 4: DRY

A class for **Wp>Complex Numbers** should either store the real and the imaginary part or absolute value and argument but not both. If both are stored, there is the invariant that both representations result in the same complex number.

So it is better to store just one representation (e.g. the real and imaginary values) and if the other representation is needed (in this case the polar form), it can be computed. This can also be done transparently in the getter method.

#### Example 5: Caching

All forms of caching and redundancy are typical violations of IAP. They are done in order to increase performance. But there is always the disadvantage that all copies have to be kept in sync as there is the invariant that the data may not be inconsistent throughout the copies. There are forms of caching where temporary inconsistencies are tolerated. This is slightly better in terms of IAP but nevertheless there are these consistency constraints and there is the danger of violating them, so to some degree the disadvantage is always there.

### Further Reading

-

---

Working with a [roofing contractor Joliet](http:*sdgit.zfmgr.top/harrisziesemer) team can help catch damage early and plan repairs before water spreads.(Image: [https:*burst.shopifycdn.com/photos/red-rooftops-and-curved-streets-of-lisbon-portugal.jpg?width=746&format=pjpg&exif=0&iptc=0](https:*burst.shopifycdn.com/photos/red-rooftops-and-curved-streets-of-lisbon-portugal.jpg?width=746&format=pjpg&exif=0&iptc=0)) If you notice water stains, damp insulation, or recurring drips after storms, leak detection and repair should be scheduled sooner rather than later. Targeted repairs can help extend roof life when the overall system is still in good shape. When a roof is near the end of its lifespan, roof replacement Joliet planning can be a practical choice.(Image: [https://burst.shopifycdn.com/photos/red-rooftops-and-curved-streets-of-lisbon-portugal.jpg?width=746&format=pjpg&exif=0&iptc=0](https:*burst.shopifycdn.com/photos/red-rooftops-and-curved-streets-of-lisbon-portugal.jpg?width=746&format=pjpg&exif=0&iptc=0)) After severe weather, storm damage roof repair may involve replacing shingles, checking flashing, and inspecting for hidden impacts that can cause leaks later.

(Image: [https:*images.unsplash.com/photo-1652676176910-04de5edd5779?ixid=M3wxMjA3fDB8MXxzZWFyY2h8MXx8cm9vZmluZyUyMGNvbnRyYWN0b3IlMjBqb2xpZXR8ZW58MHx8fHwxNzcxNjEyMDU3fDA\u0026ixlib=rb-4.1.0](https:*images.unsplash.com/photo-1652676176910-04de5edd5779?ixid=M3wxMjA3fDB8MXxzZWFyY2h8MXx8cm9vZmluZyUyMGNvbnRyYWN0b3IlMjBqb2xpZXR8ZW58MHx8fHwxNzcxNjEyMDU3fDA\u0026ixlib=rb-4.1.0))

---

## Law of Demeter (LoD)

### Variants and Alternative Names

- Principle of Least Knowledge
- Don't Talk to Strangers

### Context
- **Object-Oriented Design**
- **Implementation**

### Principle Statement

"A method of an object should invoke only the methods of the following kinds of objects:
1. itself
1. its parameters
1. any objects it creates/instantiates
1. its direct component objects"

### Description

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **Low Coupling** (LC): The goal of LoD is to reduce coupling by reducing the knowledge of a class about other classes.
- **Tell, Don'T Ask/Information Expert** (TdA/IE): LoD is more specific than TdA/IE because TdA/IE can be applied in a wider context (e.g. for responsibility assignment). Applying TdA leads to solutions which are good according to LoD. Note that the reverse is not true: Accoring to LoD you may get and set values from an object passed as a parameter to a method.

#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): Adhering to the Law of Demeter often results in additional methods.
- **Low Coupling** (LC): Adhering to the Law of Demeter may create tramp couplings which are bad.
- **High Cohesion** (HC): Adhering to the Law of Demeter often results in additional methods that mirror methods of aggregated objects. As these objects have other responsibilities, the additional methods have fewer commonalities with the "real" methods of the class, which results in a lower cohesion.

#### Complementary Principles

- **Model Principle** (MP): Adhering to LoD may result in additional methods in aggregating classes. MP tells how they should look like.

#### Principle Collections

### Further Reading

- **Wp>Law Of Demeter**
- **Wiki>Lawofdemeter**
- Andrew Hunt and David Thomas: *[The Art of Enbugging](http:*www.ccs.neu.edu/research/demeter/related-work/pragmatic-programmer/jan_03_enbug.pdf)//
- Andrew Hunt and David Thomas: *The Pragmatic Programmer*
- Craig Larman: *Applying UML and Patterns*
- David Bock: *[The Paperboy, The Wallet, and The Law Of Demeter](http:*www.ccs.neu.edu/research/demeter/demeter-method/LawOfDemeter/paper-boy/demeter.pdf)//
- Karl J. Lieberherr and Ian M. Holland: *Assuring good style for object-oriented programs*, IEEE Software, p. 38–48
- Phil Haack: *[The Law of Demeter Is Not A Dot Counting Exercise](http:*haacked.com/archive/2009/07/14/law-of-demeter-dot-counting.aspx)//
- [Law of Demeter](http://www.ccs.neu.edu/home/lieber/LoD.html)

---

## Law Of Leaky Abstractions (LLA)

### Variants and Alternative Names

### Context
- **Architecture**
- **Api Design**

### Principle Statement

> All non-trivial abstractions, to some degree, are leaky.

A solution is bad if
- the leakiness of abstractions is ignored (bad usage of an abstraction) or
- the benefits of the abstraction cannot justify the disadvantages created by its leakiness (bad abstraction) or
- the abstraction is more leaky than necessary (suboptimal abstraction)

### Description

Abstractions are typically not perfect. Especially performance aspects are very hard to abstract away from. So there are cases when using the abstraction properly is not possible without knowing the basics underneath the abstraction. So developing and, even more, fixing defects in a system with leaky abstractions makes it necessary to know about all the details the abstraction is supposed to protect the developers from. Often abstractions reduce the effort to develop a feature while they increase the effort for fixing bugs.

A typical problem with leaky abstractions is that the leakiness is ignored. This does not directly mean that the abstraction itself is bad. Often the situation without the abstraction would be worse so it's good to have it. Nevertheless its unavoidable leaks have to be kept in mind. See **example 1**.

There are also situations where the abstraction is not crafted well. Sometimes there is a way to make the abstraction better (see **example 2**) but sometimes the whole abstraction is wrong and not having it would be better.

### Rationale

- Bad usages of abstractions are plain wrong. There is no doubt that they should be avoided.
- Suboptimal abstractions can be made better. There are unnecessary leaks and each leak is a possibility for a future usage fault (see **ML**). As the leaks are unnecessary, there are ways to improve the abstraction so this one s bad.
- The benefits of an abstraction are only reached at the cost of some liabilities so an abstraction is only good when the liabilities are small enough compared to the benefits. Bad abstractions are worse than no abstraction at all, so they should not be employed.

### Strategies

- *Make the abstraction less leaky.* There might be a way to reduce the leakiness of the abstraction.
- *Find a better abstraction.* There might be a better abstraction which is less leaky.
- *Remove the abstraction.* If the abstraction is so leaky that the leakiness creates more problems than the abstraction solves, it is better not to have it.
  - Especially refrain from stacking Frameworks on top each other (e.g. Spring in an EJB Container, etc.)

### Caveats

The law of leaky abstractions by no means says that abstractions are generally a bad thing. Abstractions are extremely helpful and software development without abstractions is virtually impossible. So abstractions are good and developers should strive for creating good abstractions. But abstractions are never perfect and this has to be kept in mind.

See also section **#Contrary Principles**.

### Origin

Joel Spolsky: *[The Law of Leaky Abstractions](http:*joelonsoftware.com/articles/LeakyAbstractions.html)//

The blog article has another focus as it rather explains LLA as an effect rather than an engineering advice. Because abstractions are leaky, developers should and have to know the details abstractions try to protect them from. The principle aspect is only a side aspect there but the main focus here.

### Evidence

- **Proposed**

### Relations to Other Principles

#### Generalizations

#### Specializations
- **Fallacies Of Distributed Computing**: The eight fallacies of distributed computing are abstraction leaks which are typically present in distributed systems. Normally it is not possibly to prevent them (so there might not be a better abstraction) but it is important not to ignore these abstraction leaks.

#### Contrary Principles
- **Keep It Simple Stupid** (KISS): Creating good abstractions is sometimes complicated and ignoring leaks is simple.

#### Complementary Principles
- **Murphy'S Law** (ML): According to ML, any abstraction leak will eventually result in a mistake.
- **Model Principle** (MP): ML might help finding the right abstraction, i.e. the one which is the least leaky.
- **Rule Of Explicitness** (RoE): RoE is another way to look at abstractions. Often abstractions create a level of implicitness. Abstraction leaks are one reason why explicit solutions can be considered preferable.
- **Easy To Use And Hard To Misuse** (EUHM): The more an abstraction leaks, the less it can be considered hard to misuse.

#### Principle Collections

### Examples

#### Example 1: Distributed Objects

There is plenty of middleware which centers around the notion of distributed objects: RMI, CORBA, DCOM, ... These technologies abstract away from the fact that the objects are not local but distributed over the network. They create the illusion that calling all objects are local. But all these technologies are leaky abstractions. There is no way to abstract from the fact that calling a remote object may fail. The network connection may break down, the remote machine may not be available, etc. Furthermore there are completely different performance characteristics of remote calls. There is some unavoidable latency and no abstraction what so ever can change this.

This does not mean that these technologies are generally bad. There is a value in these abstractions but the leaks have to be kept in mind. Ignoring the fact that remote calls may fail will result in fragile systems. If the distributed system to develop should be robust, there has to be code handling failing remote calls. And for performance reasons, remote interfaces have to be crafted in a way that remote calls are minimized. So for example **Data Transfer Objects** are employed in order to transfer larger chunks of data instead of making a remote call for every access to a getter method.

#### Example 2: String Classes In C++

In C++ string literals such as `"test"` are of type `char*` which means pointer to a character. There is some range in the memory where a sequence of characters is stored and there is a pointer pointing to the first character. These strings are unhandy to use so there are string classes in C++ which abstract from these low-level strings which are simply a heritage of the C programming language C++ is built upon. These string classes are much more convenient to use and create the illusion of a built-in string type.

<code c++>
#include <iostream>

int main()
{
	std::string s = "Hello";
	s = s + " World";
	std::cout << s << std::endl;
}
</code>

In this code it looks like there was a native string type in C++. This is the abstraction. But the abstraction is leaky. The following code does not work:
<code c++>
#include <iostream>

int main()
{
	std::string s;
	s = "Hello " + "World";
	std::cout << s << std::endl;
}

</code>

So it would be better if C++ had a real native string type.

(this example is taken from )

#### Example 3: List of further examples

- The virtual address space is leaky. Paging leads to a loss of performance. And page faults show up for example while iterating over a two-dimensional array.
- Platform details are leaky. The Java Virtual Machine does a pretty good job abstracting from the underlying operating system. But at least  when it comes to accessing files based on file paths, the underlying file system structure leaks through. On Windows you have "C:\" whereas on Unixoid systems you have "/usr", "/etc" and stuff.
- SQL is leaky. Some queries have a considerably lower performance than others despite being semantically equivalent.
- Network file systems (SMB, NFS, etc) are leaky because they are remote. The connection may break down and the access is much slower than local file access.
- The finiteness of real-world machines leaks. Programming languages are said to be Turing complete. They theoretically are as powerful as a Turing machine. In fact they are not. Turing machines are conceptual automata with infinite memory. But in the real world memory is finite. There can be OutOfMemoryErrors: RAM and hard disks and any other kind of storage can and do get full.

### Further Reading
- Joel Spolsky: *[The Law of Leaky Abstractions](http:*joelonsoftware.com/articles/LeakyAbstractions.html)//
- Joel Spolsky: *[Lord Palmerston on Programming](http:*joelonsoftware.com/articles/LordPalmerston.html)// About some of the consequences of the Law of Leaky Abstractions.
- Thomas Drakengren: *[The Law of Leaky Abstractions Refined](http:*blog.drakengren.com/2005/09/05/the-law-of-leaky-abstractions-refined/)//
- *[Two Kinds of Leaky Abstractions!](http:*discuss.joelonsoftware.com/default.asp?joel.3.199142.10)//

---

## Liskov Substitution Principle (LSP)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Api Design**

### Principle Statement

> "Subtypes must be substitutable for their base types."

### Description

Object-oriented programming languages permit the derivation of subtypes from base types, and subtype polymorphism allows the passing of an object of a subtype where ever an object of the supertype is specified. Suppose `P` and `Q` are types (i.e. classes or `interface`s) and `Q` is derived from `P` (so `Q` is the subtype and `P` is the base type or supertype). A method `m` requiring a parameter of type `P` can be called with objects of type `Q` because every object of type `Q` is also an object of type `P`. This is always true as typically object-oriented programming languages are constructed in that way.

The programming language does not enforce that the subtype behaves like the supertype. Method `m` may work with an object of type `P`, but not with an object of type `Q`. LSP demands that a subtype (`Q` in the example) has to be constructed in a way that it behaves like the supertype if it is called through the supertype interface. `Q` may have further methods and it may do additional things not observable by `m` but `m` shall be able to safely assume that its parameter behaves like an object of type `P` with respect to all observable state.

### Rationale

Let `P` and `Q` be types and `Q` a subtype of `P`. If LSP is not adhered to, there is an operation accessible through the interface of `P` which behaves differently when called on `Q`. So code which is written in terms of `P` will not expect the behavior and will not work as desired.
### Strategies

- Only strengthen invariants in subclasses; never weaken them
- Only weaken preconditions when overriding methods
- Only strengthen postconditions when overriding methods
- Use Delegation instead of Inheritance
- Figure out better abstractions

### Caveats

See section **#Contrary Principles**.

### Origin

Barbara Liskov: *[Data abstraction and hierarchy](http:*citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.12.819)//

### Evidence

- **Examined** LSP describes an effect created by object-oriented type systems. There is no human factor in there, so experiments are not needed. The effect was described and thoroughly examined by Barbara Liskov and Jeanette Wing. Their reasoning is presented in section **#Rationale** in a simplified form.
- **Accepted** LSP is widely known in practice, mainly because it is part of Robert C. Martin's **Solid** principle collection.

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **Keep It Simple Stupid** (KISS): Not adhering to the LSP can be easier.

#### Complementary Principles

- **Model Principle** (MP): MP demands inheritance relations to resemble an "is-a" relationship. This means that an object of the subclass is also an object of the superclass. This is always true in a technical sense as this is how object-oriented programming languages handle inheritance hierarchies. However MP demands that is shall be true in the model, too. This is slightly different from LSP which rather is about a "is-substitutable-for" relationship.
- **Principle Of Separate Understandability** (PSU): When building inheritance hierarchies, LSP constrains how subclasses are constructed. Namely they should comply with the superclass contract. PSU on the other hand demands that the superclass shall be separately understandable, which means that knowledge of concrete subclasses and their needs should not be necessary to understand the superclass. So a superclass should not have a specific functionality, etc. just because a particular subclass needs this. In contrast to that the superclass of course may provide `protected` features for subclasses in general. But it should be inherently clear that subclasses in general may need this functionality without looking at a particular one.

#### Principle Collections

### Further Reading

- Robert C. Martin: *Agile Software Development, Principles, Patterns, and Practices*, p. 111--125
- [ButUncleBob: Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)
- **Wiki>Liskovsubstitutionprinciple**
- **Wp>Liskov Substitution Principle**
- Barbara H. Liskov , Jeanette M. Wing: *[A Behavioral Notion of Subtyping](http:*citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.39.1223)//
- Barbara H. Liskov , Jeanette M. Wing: *[Behavioral Subtyping Using Invariants and Constraints](http:*citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.28.2615)//

---

The lawyers may because the biggest those who win. This bill is a dream come true for all ambulance chasers. There aren't limits indicates. The millions they get for malpractice suits will keep their butts in plush offices and fancy cars forever. I have a doctor friend that pays over $100,000 a year in malpractice insurance on your own. Keep that in mind the next occasion you complain about my doctor bill.
Looking in the air of a [宮崎市 肩こり](https:*10.Xg4Ken.com/media/redir.php?prof=806&cid=133758944&url=https:*Goelancer.com/question/lying-with-a-healthcare-provider-can-have-dire-results-so-fess-up-2/) cleaning direction can be very difficult. How do you are about cleaning something that you simply cannot view? The real trick is that everything else needs always be cleaned thoroughly so how the germs cannot get up into atmosphere. The floor, the tops of the tables, the chairs, and anything else that people use in order to be be concentrated by healthcare cleaning specialists usually. These are places where germs can gather; disinfectants or anti-bacterial cleaners should be employed to kill these germs before they take to the air and be inhaled. This could mean cleaning a surface that does not even appear to be like it is dirty as being the germs are microscopic.

A new patient called today and asked me if I do acupuncture for back problems. His sciatic pain had gotten worse and was upsetting his golf game and sitting down. Getting this man in right away was important to helping him get better and take it easy. The answer to the question. I think every acupuncturist has treated sciatica at least once their own careers a great number other regarding lower discomfort. Denver acupuncture clinic estimates every single person will have some form of pain their own back once in their lives it is therefore no wonder that Oriental medicine can be a major treatment in helping with chronic issues.

However, you might be only peoples. Maybe you messed up and also got a low GPA or a low MCAT score. If that is the case, there certainly a little chance that you will get into an US medical school, but may get acceptance a new Caribbean school of medicine. If you are interested in studying medicine abroad, you can also apply since there is a pretty good possibility you is definite to get in. But, if you wish to study medicine in north america (which I propose you over studying in foreign medical schools), read to do with.

Knowing how well the department is functioning, can reduce patient waiting times, as there'll be sufficient beds and staff. In busy departments like A and E, the wait time could be caused by a Osteopathic clinic lack of staff and also a deficit of cubicles. Reporting systems can identify location the busy times and places will be, certain no department is understaffed.

I am burdened for anyone who haven't got access to quality healthcare. But I'm scared to death in regard to the government taking it over. I still believe the free market would have better healthcare faster, and lower costs if given possibility. State insurance boards should be eliminated and the insurance industry opened doing competition, for starters. Oops, I forgot, you can't eliminate government departments.
Health care may be in a Acupuncture clinic state of flux, but it is alive and better. The one sure thing is people are going to require health care. There is also fantastic of emphasis put on preventive medicine. Therefore, investors have to ask which solution to use. Will pharmaceuticals win over health care supplies or physical therapeutics? Which corner is most beneficial to opt for? There are no real answers as a result of questions. Is actually about what the investors for you to focus to do with. It is also relating to flexibility. Each healthcare ETF has advantages and disadvantages.

When you speak any prospect, really can 宮崎市 肩こり on cell phone or in person you be compelled to steer them away from the price question they've in the forefront of minds and drive them into why they are really calling.

If your MCAT score is low, you might still take the MCAT extra. Just be aware that schools cane easily see all your MCAT numbers. So if you do better, they'll see which will. If you do worse, they notice too.
It sense just yesterday that the Obama administration and the liberal wing of congress was singing from the tree tops at exactly how much the healthcare bill would save several the while providing us better care and insuring more Within. Wouldn't it be great to take a very simplistic path and almost by magic insure all the nations uninsured and give your lower price healthcare all while not cutting any services and giving everyone a higher quality of healthcare? Does this sound too good to be true? Not in the minds among the elitist liberals that populate the administration and our lawmakers. This is just another case of liberal favorite anecdotes. The common sense majority is aware that "if legitimate too good to be true then".well you know the rest.

When the clients calling you furthermore turn into appointments, you'll be able to Osteopathic clinic are within your way to increases the amount of clients you posses. Every time help to make an appointment, you decide to money. How cool actuality. By starting the variety of of marketing, you are going to a witness to these changes because implement the group.

---

## Miller's Law

### Variants and Alternative Names

### Context
- **User Interface Design**
### Principle Statement

Any list should not contain more than 7±2 items.

### Description

Lists such as items in the main menu of a program or bullets on a presentation slide should be limited to 7±2 items.

### Rationale

The human short-term memory is limited. Research shows that one can remember 7±2 chunks of information.

### Strategies

- Divide a long list into sub-lists adhering to the rule.

### Caveats

Note that this principle is questioned.

See also section **#Contrary Principles**.

### Origin

- The original source for 7±2 is this: George A. Miller: *[The Magical Number Seven, Plus or Minus Two: Some Limits on Our Capacity for Processing Information](http:*www.psych.utoronto.ca/users/peterson/psy430s2001/Miller%20GA%20Magical%20Seven%20Psych%20Review%201955.pdf)//
- How this became a design rule is unknown.

### Evidence

- **Examined**: There is scientific research concerning the short-term memory. But this does not apply to any aspect of design directly.
- **Accepted**: The principle is widely stated as a (user interface) design rule.
- :!: **Questioned** :!:: It is highly doubtful to directly making a limit for memorizing digits to a generally applicable design rule. There is no evidence suggesting a rule that a list should not be longer then 7±2 items.

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

#### Complementary Principles

- **More Is More Complex**: This is a proposed design rule, which is not directly affected by the criticism of Miller's Law.

#### Principle Collections

### Further Reading

- **Wp>The Magical Number Seven, Plus Or Minus Two**
- **Wiki>Sevenplusorminustwo**
- **Wiki>Sevenplusorminustwodiscussion**
- Derek M. Jones: *[The 7±2 Urban Legend](http:*www.knosof.co.uk/cbook/misart.pdf)//

---

## Minimize Coupling Between Modules [see LC]

*See: Low Coupling*

---

## Model Principle (MP)

### Variants and Alternative Names

- Direct Mapping
- Low Representational Gap (LRG)

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**
- **User Interface Design**

### Principle Statement

The object structure of the software should model and mirror those concepts and actions of the real world, that the software supports.

### Description

The software should model and mirror the "real world". This first of all means, that the structure of the software---to some extent---models the structure of the problem. When the "real world action" that the software should support comprises certain entities like e.g. customers, products, and orders, then there should be one object for each customer, product and order. Furthermore there should be one class for each concept. And if there is a certain relationship between customers, orders, and products, there should also be an association between the corresponding classes and references between the objects. So the object structure models the structure of the real world concepts.

Real world actions are then *mirrored* in the software system. This means that each action in the real world triggers a corresponding action in the model world which ensures that the model stays consistent with the real world. So the software is a kind of a simulation of what actually happens. If customer orders some product, the software reacts by creating an order object, which is connected to the customer and the product objects corresponding to the customer and the product involved in the real world action.

Be precise with semantics. An operation `cancelOrder()` should get an `Order` or `OrderId` as a parameter. In some cases you might be inclined to supply a `Product` instead, maybe because you already have a variable with that object at hand. This might work totally fine in the concrete situation but in fact it only works "by accident". Similarly you might be inclined to invoke `cancelOrder()` in a situation when you want to delete the order from the repository. This might work as expected but actually `cancelOrder` is semantically slightly different from `deleteOrder`. So even if `cancelOrder` currently does nothing but to call `deleteOrder` you should call the method on the correct abstraction level that has precisely the right semantics.

Keep sure that your software models the reality by invoking the method that has the correct semantics and supplying it with the parameters that are needed from a requirements perspective.

### Rationale

When the structures in the software roughly correspond to the structures of the problem domain, a developer doesn't have to learn both of them. Knowing the problem domain is inevitably necessary. Any further structure of the software has to be learned and understood in addition. So creating a direct mapping between them, makes understanding the software easier, which improves maintainability. In such a system for most functionality there is a "natural", i.e. an intuitively clear place to implement it. This makes structuring the software easier and helps finding the implementation for a given functionality.

Moreover if something works accidentally, it breaks accidentally. Many many bugs are created because you are not precise with semantics.

In the example above supplying a `Product` to the `cancelOrder` method only works because by some circumstance it is made sure that  when `cancelOrder` is called, there is only one order for that particular product. It's a hidden precondition. As the software is changed, this hidden precondition may not be guaranteed anymore. This results in a non-obvious bug in a part of the system you haven't directly touched. Similarly in the `deleteOrder` example: The `cancelOrder` operation might get enhanced by creating a reverse invoice, a credit note, or a message to the customer. But an order might have to be deleted for purely technical reasons (migration to a new order system, etc.). So if you call `cancelOrder` instead of `deleteOrder` this will produce nasty bugs if `cancelOrder` gets enhanced as described.
### Strategies

- Create a class for each relevant real-world concept ("natural classes")
- Create methods corresponding to real-world actions
- Map additionally necessary behavior to natural classes instead of creating artificial classes
- For artificial behavior that cannot be mapped to a natural class at least create a metaphor or an artificial model (like a state machine)
- Be precise with semantics. If you have an operation that currently does what you need but for slightly different reasons because it's an operation on the wrong abstraction level, create a new operation with the correct semantics. Have that new operation call the existing one as an implementation detail (e.g. have a `cancelOrder` method call the `deleteOrder` method).

### Caveats

This principle may lead to the problem of modeling the real world in too great detail. This complicates the design without giving any further benefits. Especially a wrong understanding of inheritance may lead to **Taxomania**, where an inheritance relation and the respective classes are only introduced because there seems to be such a taxonomy in the "real world". But inheritance should only be used on purpose, not just because it is possible. A special form of this problem is called **Vapor Classes**, which are useless abstractions which are never used.

See also section **#Contrary Principles**.

### Origin

The root of this principle is the very beginning of object-orientation itself. The idea behind **Wp>Simula**, the first object-oriented programming language, was to view program executions as simulations. Kristen Nygaard, one of the creators of Simula defines object orientation as follows:
> "**Object-oriented programming.** A program execution is regarded as a physical model, simulating the behavior of either a real or imaginary part of the world.".

Although this view is disputed as a definition for object-oriented programming, it became the key idea of object-oriented analysis. In  Grady Booch clearly states that objects "directly reflect our model of reality".

### Evidence

- **Accepted**: Virtually every introduction to object-oriented analysis roughly explains this but mostly without stating it as a principle. Bertrand Meyer explains this principle in his book *Object-Oriented Software Construction*
- **Questioned**: The value of this principle is disputed. It is questioned whether objects in the OOP sense nicely map to real-world objects. Furthermore there is the typical object-relational impedance mismatch and the observation that business rules are sometimes cross-cutting. There also is not one single obvious model for the "real world". A model is subjective to the one creating the model. So it is not enough to model the "real world" but it is important to think about how to model it.

### Relations to Other Principles

#### Generalizations

- **Minimize Intellectual Distance**

#### Specializations

#### Contrary Principles

- ****Encapsulate The Concept That Varies** (ECV)**: Sometimes there are "concepts that vary" which are not directly related to a real-world concept. So ECV demands having an artificial class.
- **Keep It Simple Stupid** (KISS): There are often simpler ways to build a software system than to model and mirror the real world behavior, which frequently means having more objects and more complicated structures.
- **Single Responsibility Principle** (SRP): Following the Model Principle sometimes results in classes having more than just one responsibility.
- **High Cohesion** (HC): MP sometimes creates classes with suboptimal cohesion. See also SRP.

#### Complementary Principles

- ****Tell, Don'T Ask/Information Expert** (TdA/IE)**: TdA/IE tells how to distribute functionality among the natural classes which are created according to the Model Principle.
- **Low Coupling** (LC): When designing a model for a software, it has to be borne in mind that structures with low coupling are desirable.
- **Law Of Leaky Abstractions** (LLA): When building abstractions according to MP, keep in mind that there will most likely be abstraction leaks. A good abstraction minimizes those leaks.

#### Principle Collections

### Examples

#### Example 1: Object Structure (Library)

In a software system for a library, there will be a classes like `Book`, `Reader`, and `Lending`. A reader has a name, a book has a title and the reader must return the book after some date of expiry. So the corresponding classes will have attributes describing these properties. The reader may borrow and return a book, so the `Reader` class will have methods `borrow()` and `return()`. Classes, attributes, and methods are directly inferred from the problem domain.

#### Example 2: Swing

GUI frameworks like Java Swing typically have classes corresponding to the types of controls that can be used to build graphical user interfaces. So Swing for example has classes like `JButton`, `JCheckBox`, and `JTextField`.

Furthermore buttons, check boxes, text fields, and the like are also models of concepts in the real world. Buttons are typical controls of machines and check boxes and text fields are parts of a typical (paper-based) form. So the class `JCheckBox` is a model for a check box on the screen which itself is a model for a check box on a paper-based form.

#### Example 3: Dependencies

MP also tells which modules may depend on which others. Suppose we have a software comprising a parser for mathematical functions. Obviously there will be classes `Parser` and `Function`. MP tells that dependencies between these classes shall be according to the model. Logically a parser parses a string and creates `Function` objects. It is impossible to think about a `Parser` without `Function`s. So `Parser` may naturally depend on `Function`.

On the other hand our intuitive model of parsers and functions tells us that a `Function` does not need a `Parser` to be a meaningful entity. One can easily think of `Function`s created by using builder functions instead of a parser. And even if that wasn't true and there would only be the possibility to create functions by using parsers, a `Function` object logically can work without knowing that there are parsers which have created it. In an imaginary hierarchy of modules Parser would be a module on a higher scale than `Function`. So MP forbids that `Function` depends on `Parser`.

#### Example 4: Brake and Air Conditioning

Suppose a car has an air conditioning and a hill start assistant. The air conditioning needs to make sure that the engine provides enough power on sunny days. So it measures its power-consumption and pushes down the gas pedal just enough to the engine isn't stalled. The hill start assistant automatically releases the hand brake if you start driving. Now the following situation can happen: A car waits in front of a boom barrier of an underground garage. It's a hot day and the driver opens the window to get the ticket. hot air flows into the car and the A/C powers up. The revolution speed is low because the car stands still so the A/C hits the gas pedal in order not to stall the engine. Now the hill start assistant realizes that the gas pedal was pressed and releases the hand brake because pressing the gas pedal is the trigger that the diver want to drive away. As a result the car crashes into the boom barrier.

The problem here is with the A/C. Semantically it wanted to increase the motor power but actually it called an operation that hit the gas pedal. This is almost the same but not exactly the same. It's an operation on the wrong level of abstraction. If the A/C had called an operation `increaseMotorPower` instead of an operation `hitGasPedal` the problem would have been prevented.

#### Example 5: Inferring Information

Suppose there is a smartphone app and its backend. The app lets you buy several products. One day the app developers demand that the backend adds a field describing the product size (small, medium, large). The backend developers add the field and everyone is happy. The app developers use this field so they can decide whether gift wrapping is available (large products cannot be gift-wrapped). Half a year later the company decides to enhance their product portfolio by reselling products of company B. The backend will redirect orders of B-products directly to company B so they are in charge of delivery.

Unfortunately B is not able to provide gift-wrappings at all. So the smartphone app has to be changed such that it not only removes the gift-wrapping option for large products but also for B-products. But smartphone apps need to be updated by the customer (and some of them never do). So the only way to ensure that gift-wrapping is handled correctly even if the customer hasn't updated yet, is that the backend returns `size:large` for every B product (even for small ones).

The `size` field gets deprecated and a new field `giftWrappingAvailable` is added. In fact that's what they should have done in the first place. The app developers needed to know if gift wrapping is available. But instead they inferred this information based on the size. This worked but it worked accidentally---and it broke accidentally.

### Further Reading

- **Wiki>Programsrepresentmentalmodels** (general discussion on mental models)
- **Wiki>Systemmetaphor** (metaphors in extreme programming are a specific kind of model as discussed here)
- **Wiki>Thevalueofdomainmodels** (general discussion on domain models)
- [Martin Fowler: AnemicDomainModel](http://www.martinfowler.com/bliki/AnemicDomainModel.html) (the result of not properly adhering to the principle)
- [Martin Fowler: Domain Logic and SQL](http://martinfowler.com/articles/dblogic.html) (other possible to implement a system and their difference to domain models)

---

## More Is More Complex (MIMC)

### Variants and Alternative Names

- Less is more

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**
- **User Interface Design**
- **Implementation**
- **Documentation**

### Principle Statement

More is more complex.

### Description

Having more lines of code, methods, classes, packages, executables, libraries etc. always means also to have more complexity (which is bad). This means that given the complexity of the problem is fixed, a suitable compromise for the number of methods, classes, etc. has to be found. Reducing the number of statements per method typically results in the introduction of further methods. Reducing the number of methods per class can be achieved by dividing the class into several smaller classes, etc.

There is both: too large modules (i.e. undermodularization) and too small modules (i.e. overmodularization). Either there is too much complexity in a module (MIMC applied to one module) or there is too much complexity between the modules (MIMC applied to the number of modules).

Note that it is actually not the number of lines, methods, classes, etc. that is relevant but the effective number of items that have to be kept in mind for the purpose of understanding. So reducing the number of lines by placing several statements in one line does not help. Neither the introduction of an additional obvious private method exceeding the limit will do any harm. MIMC is just a rule of thumb stating that the introduction of further modules (and the like) usually has a higher complexity as a drawback.

For documentation it simply states that fewer documentation is better.

### Rationale

The capabilities of the human mind are certainly limited. If it is necessary to keep a large amount of modules or lines of code in mind, it is difficult to understand. Furthermore if a module is large, it takes a long time to read (and thus to comprehend). And if there are many modules, looking for a particular module takes a long time. And the longer the searching process takes, the more one will have forgotten what has been read previously. This results in worse readability, understandability and thus maintainability.

Regarding documentation it is evident that smaller amounts of documentation are read faster.

### Strategies

- Avoid many modules
  - Merge several modules into one
  - Don't introduce a new module but put the functionality into another module
- Avoid big modules
  - Divide large modules into several smaller ones
  - Introduce new modules to group related functionality. A **Parameter Object** is a typical example for this.

### Caveats

- Note that **Miller'S Law** is often cited in this context but it is doubtful if and to what extend it applies.
- Note that this principle is contrary to itself. Given a desired functionality a certain level of complexity in inevitable. This leads in the extremes either to a large amount of small classes or a large amount of code in a fewer class. The same applies on other levels like number and size of methods, etc. So there is always a tradeoff between MIMC and itself applied to different aspects of the software system.
- Furthermore note that having more classes can be regarded better than having too large classes. See **Add More Classes**.
- Having no documentation is best with respect to MIMC. But of course there are contrary principles.

See also section **#Contrary Principles**.

### Origin

The phrase "more is more complex" is new but can be regarded trivially intuitive to every developer. There is also some research concerning certain aspects of MIMC. See section **#Evidence**.

### Evidence

**Status: Examined** — There is some research relating module size to certain quality attributes like maintenance cost, error density, etc. Basili and Perricone studied maintenance data of Fortran programs for aerospace applications . They found that the smaller modules had a higher error density than the larger ones. At first this seems to contradict MIMC. But assuming there is a certain essential complexity of the problem, this complexity has to be implemented somehow. Either this leads to a few large modules or many smaller ones. In the latter case the complexity is in the relationships and interactions between the modules instead of the modules themselves. So too small modules result in more modules and more complex communication among them. Other studies seem to confirm this.

This phenomenon that the defect density is high for small modules but also rises for large modules is called the "Goldilocks Conjecture". As a result there is an optimal module size which is neither too small, nor too big. Several publications claim to have found this optimal module size. Depending on the programming language used, these values typically are claimed to be a few hundred lines of code. Note that most of these studies are in the context of procedural programming.

This sounds intuitive but the Goldilocks Conjecture is disputed. Some point out that the negative correlation between defect density and size is just a mathematical artifact and that there are also other methodological problems with these studies. There is also data which is not explainable by defect models based on the Goldilocks Conjecture.

The relationship between module size and defect proneness is complex and not clear. Furthermore modularization is not only a task in terms of module size. The more interesting aspect is how to assign responsibilities to modules. So apart from module size there are many other aspects influencing modularization (see especially **MP**, **LC**, and **HC**) which makes it hard to isolate the pure effect of size.

This is an important research question but as MIMC is just a qualitative rule of thumb (just as the other principles are). So the principle can be deemed helpful despite the Goldilocks Conjecture being disputed.

As a specific aspect of MIMC, complexity through deep inheritance relations is known to reduce effectiveness and efficiency of maintenance. There are controlled experiments showing this. On the other hand these results are limited as there may be many factors which are neglected by the experiment. Most notably in these experiments maintenance tasks where carried out on systems with artificially constructed inheritance hierarchies. It is undisputed hat there are good ways and bad ways of using inheritance. And it is doubtful that there are several equally good solutions for the same problem only differing in the depth of inheritance. So there is some evidence but no "proof" that deep inheritance hampers maintenance.

**Status: Questioned** — The Goldilocks Conjecture, which can be seen as an aspect of MIMC, is disputed. See above.

### Relations to Other Principles

#### Generalizations

- **Keep It Simple Stupid** (KISS): MIMC states that having more modules, etc. leads to more complexity. KISS on the other hand is about the avoidance of every form of complexity.

#### Specializations

#### Contrary Principles

Note that many principles are contrary to MIMC as they favor the introduction of additional modules. This means that it is worthwhile to consider MIMC when considering one of those. Nevertheless this does not mean that this is true the other way around. When considering MIMC, one wouldn't want to consider all principles that have complexity as a disadvantage. So here are those needing consideration:

- **More Is More Complex (MIMC)**: Changing a design to adhere to the MIMC principle may always lead to more complexity concerning another aspect of the system. For example reducing the amount of code in a large method is typically achieved by the introduction of further methods. So there is always a tradeoff between this principle and itself.
- ****High Cohesion** (HC)**: Not introducing further modules typically leads to a lower cohesion.
- **Add More Classes**: While MIMC is a very general principle that applies to virtually everything, it may be regarded better to have more classes than bigger classes.
- **More Stakeholders, More Details** (MSMD): The more stakeholders there are, the more documentation is needed.
- **Navigation Avoidance Principle** (NAP): When trying to minimize documentation try not to create the need for navigation.

#### Complementary Principles

- **Miller'S Law**: This is the law about a conceptual limit often cited as a (user interface) design rule.
- **Document The Hard Stuff** (DHS): When trying to minimize documentation DHS tells you what you should document and what you can leave out.
- **Don'T Repeat Yourself** (DRY): Eliminating duplication is a way to reduce complexity.

#### Principle Collections

### Examples

FIXME

---

## Murphy's Law (ML)

### Variants and Alternative Names

- Design for Errors

### Context
- **Object-Oriented Design**
- **Api Design**
- **User Interface Design**
- **Implementation**

### Principle Statement

Whatever can go wrong, will go wrong. So a solution is better the less possibilities there are for something to go wrong.

### Description

Although often cited like that, Murphy's Law actually is not a fatalistic comment stating "that life is unfair". Rather it is (or at least can be seen as) engineering advice to design everything in a way that avoids wrong usage. This applies to everything that is engineered in some way and in particular also to all kinds of **Modules**, (user) interfaces and systems.

Ideally, incorrect usage is strictly impossible. For example, this is the case when the compiler will stop with an error if a certain mistake is made. And in the case of user interface design, a design is better when the user cannot make incorrect inputs as the given controls won't let him.

It is not always possible to design a system in such a way. But as systems are built and used by humans, one should strive for such "fool-proof" designs.

There are different kinds of possible errors that can and according to ML eventually will occur in some way: Replicated data can get out of sync, invariants can be broken, preconditions can be violated, interfaces can be misunderstood, parameters can be given in the wrong order, typos can occur, values can be mixed up, etc.

Note that Murphy's law also applies to every chunk of code. According to the law the programmer will make mistakes while implementing the system. So it is better to implement a simple design, as this will have fewer possibilities to make implementation mistakes. Furthermore code is maintained. Bugfixes will be necessary, present functionality will be changed and enhanced, so every piece of code will potentially be touched in the future. So a design is better the fewer possibilities there are to introduce faults while doing maintenance work.

### Rationale

Systems are built and used by humans. And as humans always will make mistakes, there always will be some possibilities for a certain mistake. So if some mistake is possible, eventually there will be someone who makes this mistake. This applies likewise to system design, implementation, verification, maintenance and use as all these tasks are (partly) carried out by humans.

This means the fewer possibilities there are that a mistake is made, the fewer there will be. As mistakes are generally undesirable, a design is better when there are fewer possibilities for something to go wrong.

Note that ML does *not* claim that everything constantly fails unless there is no possibility to do so. It simply says that statistically in the long run a system will fail if it can.

### Strategies

This is a very general principle so there is a large variety of possible strategies to adhere more to this principle largely depending on the given design problem:

- Make use of static typing, so the compiler will report faults
- Make the design simple, so there will be fewer implementation defects (see **KISS**)
- Use automatic testing to find defects
- Avoid duplication and manual tasks, so necessary changes are not forgotten (see **DRY**)
- Use polymorphism instead of repeated switch statements
- Use the same mechanisms wherever reasonably possible (see **UP**)
- Use consistent naming and models throughout the design (see **MP**)
- Avoid Preconditions and Invariants (see **IAP**)
- Use assertions to detect problems early.
- ...

### Caveats

See section **#Contrary Principles**.

### Origin

The exact wording and who exactly coined the term, remains unknown. Nevertheless, it can be stated that its origin is an experiment with a rocket sled conducted by Edward A. Murphy and John Paul Stapp. During this experiment, some sensors had been wired incorrectly. A more accurate quote might read something like this: "If there's more than one possible outcome of a job or task, and one of those outcomes will result in disaster or an undesirable consequence, then somebody will do it that way." A more detailed version of the history of the experiment and the law can be found in  and Wikipedia.

### Evidence

- **Accepted** The principle is widely known and its validity is assumed. Nevertheless sometimes it is rather used as a kind of joke instead of as design advice. See for example Jargon File: *[Murphy's Law](http:*www.catb.org/jargon/html/M/Murphys-Law.html)//

Furthermore every defect in any system is a manifestation of ML. If there is a fault then obviously something went wrong. The correlation between the number of possibilities for introducing defects and the actual defect count can be regarded trivially intuitive.

### Relations to Other Principles

#### Generalizations

#### Specializations

- **Don'T Repeat Yourself** (DRY): Duplication is a typical example of error possibilities. In case of a change, all instances of a duplicated piece of information have to be changed accordingly. So there is always the possibility to forget to change one of the duplicates. DRY is the application of ML to duplication.
- **Easy To Use And Hard To Misuse** (EUHM): Because of ML an interface should be crafted so it is easy to use and hard to misuse. EUHM is the application of ML to interfaces.
- **Uniformity Principle** (UP): A typical source of mistakes are differences. If similar things work similarly, they are more understandable. But if there are subtle differences in how things work, it is likely that someone will make the mistake to mix this up.
- **Invariant Avoidance Principle** (IAP): Invariants are statements that have to be true in order to keep a module in a consistent state. ML states that eventually an invariant will be broken resulting in a hard to detect defect. IAP states that invariants should therefore be avoided. So IAP is the application of ML to invariants.

#### Contrary Principles

- ****Keep It Simple Stupid** (KISS)**: On the one hand a simpler design is less prone to implementation errors. In this aspect, KISS is similar to ML. On the other hand, it is sometimes more complicated to make a design "fool-proof" so usage and maintenance mistakes are prevented. In this aspect KISS is rather a contrary principle. Both apply at the same time so a tradeoff has to be made whether correct implementation or correct usage and maintenance are more important in the given case. This means, it is necessary to consider KISS in addition to ML in order to find a suitable compromise. See ** Parameters**.

#### Complementary Principles

- ****Fail Fast** (FF)**: Sometimes it is impossible to actually prevent an error. In such a case it is advisable to fail fast so the error is recognized early.

#### Principle Collections

### Examples

#### Example 1: Parameters

Suppose there are two methods of a string class `replaceFirst()` and `replaceAll()` which replace the first or all occurrences of a certain substring, respectively.

The following method signatures are a bad choice:
<code java>
replaceFirst(String pattern, String replacement)
replaceAll(String replacement, String pattern)
</code>
Eventually someone will mix up the order of the parameters leading to a fault in the software which is not detectable by the compiler.

So it is better to make parameter lists consistent:
<code java>
replaceFirst(String pattern, String replacement)
replaceAll(String pattern, String replacement)
</code>
This is less error prone. When for example a call to `replaceFirst()` is replaced by a call to `replaceAll()`, one cannot forget to exchange the parameters anymore. This is how it is done in the [Java API](http://docs.oracle.com/javase/7/docs/api/java/lang/String.html#replaceFirst(java.lang.String, java.lang.String)).

But here still one could mix up the two string parameters. Although this is less likely, as having the substring to look for first is "natural", such a mistake is still possible. An alternative would be the following:
<code java>
replaceFirst(Pattern pattern, String replacement)
replaceAll(Pattern pattern, String replacement)
</code>
Here both methods expect a `Pattern` object instead of a regular expression expressed in a string. Mixing up the parameters is impossible in this case as the compiler would report that error. On the other hand using these methods becomes a bit more complicated:
<code java>
"This are a test.".replaceFirst(new Pattern("are"), "is");
</code>

instead of
<code java>
"This are a test.".replaceFirst("are", "is");
</code>
The **KISS-Principle** is about this disadvantage.

#### Example 2: Casts and Generics

Another example for the application of Murphy's Law would be the avoidance of typecasts:

<code java>
List l = new ArrayList();
l.add(5);
return (Integer)l.get(0) * 3;
</code>

This works but it makes a cast necessary and every cast circumvents type checking by the compiler. This means it is theoretically possible that during maintenance someone will make a mistake and store a value other than Integer in the list:
<code java>
l.add("7");
</code>
Murphy's Law claims that however unlikely such a mistake might seem, eventually someone will make it. So it is better to avoid it. In this case this could be done using Generics:
<code java>
List<Integer> l = new ArrayList<Integer>();
l.add(5);
return l.get(0) * 3;
</code>
Here this mistake is impossible as the compiler only allows storing integers.

Note that the typecast is rather a symptom than the actual problem here. The problem is, that the `List` interface is not generic and the symptom is the typecast. The reason for this flaw is, that the `List` interface predates the introduction of generics in Java.

#### Example 3: Date, Mutability/Aliasing

In Java the classes [`Date`](http:*docs.oracle.com/javase/7/docs/api/java/util/Date.html) as well as the newer [`Calendar`](http:*docs.oracle.com/javase/7/docs/api/java/util/Calendar.html) are mutable which means the reference semantics of Java objects may cause unintended alternations of date values. Eventually someone will copy the reference to a date object instead of copying the object itself, which is usually a mistake when programming with dates.

<code java>
Date date1 = new Date(2013, 01, 12);
Date date2 = date1;
System.out.println(date1); // Sun Feb 12 00:00:00 CET 3913
System.out.println(date2); // Sun Feb 12 00:00:00 CET 3913
date1.setMonth(2);
System.out.println(date1); // Sun Mar 12 00:00:00 CET 3913
System.out.println(date2); // Sun Mar 12 00:00:00 CET 3913
</code>

Furthermore as can be seen in the code above, the month value counterintuitively is zero-based, which results in 1 meaning February. This obviously is another source for mistakes. Also the order of the parameters can be mixed up easily. And lastly this does not refer to a date in 2013 but to one in 3913! The year value is meant to be "two-digit", so 1900 is added to it. So there are plenty of possibilities for making mistakes. And sooner or later someone will make them.

Because of these and several other flaws in the design of the Java date API, most of the methods in `Date` are deprecated and also the newer `Calendar` API will be replaced by a [new API](http://openjdk.java.net/jeps/150) in Java 8.

### Further Reading

- **Wp>Murphy'S Law**
- **Wiki>Murphyslaw**

---

## One Responsibility Rule [see SRP]

*See: Single Responsibility Principle*

---

## Open-Closed Principle (OCP)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Api Design**

### Principle Statement

Modules should be open for extension but closed for modification.

### Description

### Rationale

### Strategies

### Caveats

Beware that wrong application of OCP may lead to the anti-pattern **Onion**.

See also section **#Contrary Principles**.

### Origin

Bertrand Meyer: *[Object-Oriented Software Construction](http:*en.wikipedia.org/wiki/Object-Oriented%20Software%20Construction)//, p. 57pp.

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **Encapsulate The Concept That Varies** (ECV): The OCP demands encapsulating abstract concepts in base classes (or interfaces) in order to be able to enhance the module by subclassing which is possible without changing the previously written code. In this case several variations of a concept may exist in the code at the same time. There is always the abstract base class plus one or usually more concrete subclasses. So the OCP is about encapsulating abstract concepts that vary "in space".

#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): The OCP demands introducing abstract base classes or interfaces.
- **Keep It Simple Stupid** (KISS): The OCP demands introducing abstract base classes or interfaces. This increases complexity.
- **Model Principle** (MP): OCP sometimes results in the introduction of artificial classes that do not correspond to a real-world concept.

#### Complementary Principles

- **Dependency Inversion Principle** (DIP): OCP results in the introduction of abstract classes or interfaces and descendant concrete classes. DIP now tells that other classes should only depend on the abstractions.
- **Liskov Substitution Principle** (LSP): OCP may results in the introduction of abstract classes or interfaces. Here it is important to get the abstraction right. Otherwise LSP may be violated.

#### Principle Collections

### Examples

##### Client Repository
Let's say the high-level module (your business logic), wants to be able to add or remove clients to the database. Instead of it talking to the database directly, it defines an interface called ClientRepository which contains the methods the business logic needs (**DIP**). Now you go along and implement a MySQLClientRepository. Some time in the future, you are asked to switch to an oracle database. You can now, without modifying any code from your business logic, switch to the oracle database: by extending ClientRepository to implement OracleClientRepository. You just need to wire an OracleClientRepository instance to the business logic and you have made the switch without modifying any business logic code.
### Further Reading

- Robert C. Martin: *Agile Software Development, Principles, Patterns, and Practices*, p. 99pp.
- [ButUncleBob: Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

---

## Pit Of Success [See EUHM]

*See: Easy To Use And Hard To Misuse*

---

## Postel's Law

### Variants and Alternative Names

- General Principle of Robustness (not to be confused with Eric S. Raymond's **Rule Of Robustness**)

### Context
- **Object-Oriented Design**
- **Api Design**

### Principle Statement

> "Be conservative in what you do, be liberal in what you accept from others."

### Description

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **Fail Fast** (FF): While FF is (amongst others) about checking for erroneous parameters, Postel's Law is about not being too strict with parameters. It says that the design should allow for uncommon or strangely arranged (yet meaningful) input data. This does not contradict FF as Postel's Law does not demand to process meaningless or erroneous data.

#### Complementary Principles

- **Principle Of Least Surprise**

#### Principle Collections

### Further Reading

- **Wp>Robustness Principle**
- Jon Postel: [Internet Protocol](http://www.postel.org/ien/txt/ien111.txt)
- Martin Fowler: *[Tolerant Reader](http:*martinfowler.com/bliki/TolerantReader.html)//
- Joel Spolsky: *[A Hard Drill Makes an Easy Battle](http:*www.joelonsoftware.com/articles/fog0000000306.html)//
- Eric S. Raymond: *[The Art of Unix Programming: Rule of Repair](http:*www.catb.org/~esr/writings/taoup/html/ch01s06.html#id2878538)//

---

## Principle Of Least Astonishment (PLA) [see PLS]

*See: Principle Of Least Surprise*

---

## Principle Of Least Knowledge [see LoD]

*See: Law Of Demeter*

---

## Principle Of Least Privilege

### Variants and Alternative Names

- principle of minimal privilege
- principle of least authority

### Context
- **Security**

### Principle Statement

Every program and every privileged user of the system should operate using the least amount of privilege necessary to complete the job.

### Description

In a particular abstraction layer of a computing environment, every module (such as a process, a user, or a program, depending on the subject) must be able to access only the information and resources that are necessary for its legitimate purpose.

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

From Jerome H. Saltzer in 1974.

### Evidence

- **Accepted**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

#### Complementary Principles

#### Principle Collections

### Examples

#### Example1:

---

## Principle Of Least Surprise (PLS)

### Variants and Alternative Names

- Principle of Least Astonishment (PLA)
- May also be referred to as "rule" or "law" instead of "principle"
- Acronyms sometimes include the "o" for "of": PoLA, PoLS

### Context
- **Object-Oriented Design**
- **Api Design**
- **User Interface Design**
- **Implementation**

### Principle Statement

> In interface design, always do the least surprising thing.

### Description

Never surprise the user. An interface should behave exactly as the user thinks it behaves. What surprises the user depends on the kind of interface (user interface, module interface) and the type of user (end user, fellow programmer, maintainer). The central idea of PLS is to think about how the user would want to use the interface.

### Rationale

Surprises are always a potential source for frustration. A user wants to be in control of the system. If the system does not behave as intended, the user gets disappointed and has to determine how to get the system do what it should do. On the other hand a system that behaves according to the users wishes is pleasant to use.

Secondly when everything works as expected, the user will make fewer mistakes. In case of a user interface this means that the user is more effective and in case of a module interface the software will have fewer defects.

### Strategies

- Separate methods that change an object (commands) from methods asking the object a question (queries) (see **CQS**)
  - This especially means that a query method should not alter the observable object state
- Name all modules in a way that clearly communicates what the module is and does
  - Names of classes shall be nouns representing a specific (real-world) concept (see **MP**)
  - Names of `interfaces` shall be adjectives describing a specific property. This typically results in names ending with -able
  - Names of command methods shall be verbs (in imperative form)
  - Names of query methods shall start with get- or is-
  - Names of mathematical functions or the like shall be named by the respective concept (like `sqrt`)
  - ...
- Avoid "clever" solutions which are hard to grasp in favor of simple, dumb ones (see **KISS**)
  - **When In Doubt, Use Brute Force**
  - Tend to use the first solution that comes in mind

### Caveats

See section **#Contrary Principles**.

### Origin

The precise origin is unknown. Probably it's *[The Tao Of Programming](http:*www.canonical.org/~kragen/tao-of-programming.html)// by Geoffrey James.

### Evidence

**Status: Accepted** — PLA is widely known and also treated in Eric S. Raymond's *The Art of Unix Programming*

### Relations to Other Principles

#### Generalizations

- **Easy To Use And Hard To Misuse** (EUHM): A module is easy to use if there is no surprise in how it works.

#### Specializations

- **Command-Query Separation** (CQS)
#### Contrary Principles

#### Complementary Principles

- **Fail Fast** (FF): FF is about what a module should do in the case of error. PLS on the other hand is about how the module should behave normally. Furthermore it normally is not a surprise that a module fails when there is an error but a module that doesn't fail when it should, behaves strangely.
- **Model Principle** (MP): PLS is mainly about how module identifier and module behavior relate to each other. MP tells that modules named according to the model are least surprising.
- **Uniformity Principle** (UP): When applying PLS, UP should also be considered for naming modules.

#### Principle Collections

### Further Reading

- **Wp>Principle Of Least Astonishment**
- Eric S. Raymond: *[The Art of Unix Programming: Rule of Least Surprise](http:*www.catb.org/~esr/writings/taoup/html/ch01s06.html#id2878339)//
- Eric S. Raymond: *[The Art of Unix Programming: Applying The Rule of Least Surprise](http:*www.catb.org/~esr/writings/taoup/html/ch11s01.html)//
- **Wiki>Principleofleastastonishment**
- Joshua Bloch: *[How to Design a Good API & Why it Matters](http:*www.infoq.com/presentations/effective-api-design)//

---

## Principle Of Separate Understandability (PSU)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Implementation**

### Principle Statement

Each module shall be understandable on its own---without knowing anything about other modules.

### Description

PSU means that:
- By looking at a class its purpose should be clear.
- By looking at the public methods of a class it should be clear why they are there. That means there should be no method that is only there because a specific other module needs it.
- By looking at the implementation of a module it should be clear how it works and why it was done that way. That means there should be no code that is solely there in order to make another module work.
- By looking at a private method it should be clear what it does. That means there should be no (private) method that is only meaningful in the context of another method (see **example 2**).
- By looking at a method invocation it should be clear what happens, why the parameters are there, and what they specify. It should not be necessary to look up the method implementation (see **example 3**).
- By looking at a single line of code it should be clear what it does without having to look up other code.

### Rationale

When a module is separately understandable, it is easier to maintain, as no other modules have to be considered during maintenance. It is furthermore more testable, as a unit test can easily test only this particular module without requiring integration with other modules.

An important  aspect of PSU is readability or rather understandability. If a module---say a method---requires to understand several other modules (other methods, the usage of certain attributes, the idea of the whole class, ...), a much larger part of the code has to be read and kept in memory. And if a method call is not separately understandable, the reader of the code will have to jump to the implementation of the method in order to see what's going on. This is unnecessarily time consuming:

- You have to find the implementation and jump there (modern IDEs help here but it takes time nevertheless)
- While doing so, you have to memorize the call and the context of the call. If implementation and call are not colocated (which is preferable but not always possible) you won't see the call anymore so you have to memorize it.
- Then you have to read the code and **mentally inline** it.
- If you could not memorize everything, you might have to jump back and forth to do the job.
- After you did all that you have to jump back and continue reading the method with the call you just mentally inlined.

In a nutshell, if you have to mentally inline code, it would have been better if it was already inlined. The **method extraction** in fact was harmful to readability and not beneficial. Note that not extracting a method needn't be the best solution to the problem. Often renaming the extracted method already does the job. Maybe this also hints that not the right piece of code has been extracted.

Another point of view is that a violation of PSU either means that a part of the functionality does not belong to that module or the module has the wrong abstraction. So this is a sign of a design that needs improvement.

### Strategies

When a module does not comply with PSU, this means that either a part of the functionality of the module does not belong here (see **example 1**) or the module has the wrong abstraction (**example 3**). So strategies for making a solution more compliant with PSU are:

- Move the conflicting functionality to another module where it fits better: **Move Field**, **Move Method** (see **IE**, **HC**, and **MP**).
- Build up a new module for the conflicting functionality: **Extract Method**, **Extract Class** (see **HC**).
- Find the right abstraction for the module that allows the functionality to stay here (see **MP**).
- Find a name which properly describes the abstraction of the module: **Rename Module** (**MP**).

### Caveats

See section **#Contrary Principles**.

### Origin

This principle is newly proposed in this wiki. Nevertheless it is believed that it is not "new" in the sense that its a new insight. Its rather something that is commonly known but hasn't been expressed as a principle, yet.

### Evidence
- **Proposed** (see origin)

- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **Keep It Simple Stupid** (KISS): Not to adhere to PSU is sometimes easier.

#### Complementary Principles

- **Information Hiding/Encapsulation** (IH/E): PSU is about constructing a module such that its inner workings (and its usage also) can be understood without knowledge about other modules. IH/E on the other hand is about constructing a module in a way that hides the inner workings so it can be used without knowing *them*.
- **Model Principle** (MP): The model contains the only information that should be necessary to understand the module. And if the abstraction of the model is wrong, MP helps getting it right.
- **Tell, Don'T Ask/Information Expert** (TdA/IE): At its heart PSU is about responsibility assignment. When a module is not separately understandable, this means that a responsibility is scattered across several modules. TdA/IE gives another aspect of responsibility assignment.
- **Low Coupling** (LC): Not adhering to PSU means that responsibilities are scattered across several modules. This typically also means increased coupling.
- **Single Level Of Abstraction** (SLA): The purpose of PSU is to avoid **Mental Inlining**. SLA on the other hand is about the avoiding the opposite: **Mental Grouping**.

#### Principle Collections

### Examples

#### Example 1: Parsing Data

Suppose a program parses data stored in an spreadsheet file. There are three classes:
- `SpreadsheetReader`: This reads the spreadsheet and creates `DomainObject` objects.
- `DomainObject`: This is the data which was contained in the spreadsheet and is now processed by the program in some way.
- `SpreadsheetWriter`: This class takes a `DomainObject` and writes it back to the spreadsheet.

In such a scenario it might be convenient to simplify `SpreadsheetWriter` by adding information about the spreadsheet to `DomainObject`. This might be some cell coordinates for example. `SpreadsheetReader` can store them into the newly created `DomainObject` and `SpreadsheetWriter` uses the data to store the `DomainObject` to the correct position in the spreadsheet. The problematic method is `DomainObject.getCellPositionInSpreadsheet()`.

This is a simple solution (see **KISS**) but it violates PSU. `DomainObject` is not understandable on its own. It holds data (namely the cell position in the spreadsheet) that is only meaningful in the context of the other two modules. During maintenance this data could accidentally be altered (resulting in a corrupted output file). Maintenance effort is also increased simply by distracting the maintainers who might wonder what this data is and if it is relevant for their task.

A better solution (wrt. PSU) would be to give `SpreadSheetWriter` the ability to determine the correct position in the spreadsheet itself. This is more complicated and may involve searching the spreadsheet for the correct position. But `DomainObject` is easier to understand and less prone to errors.

#### Example 2: Dependent Private Methods

In a module that computes results in a bowling game there might be a method `strike()` which returns true when the player has thrown a strike, i.e. hit all 10 pins with only one ball throw.

<code java>
private int ball;
private int[] itsThrows = new int[21];

private boolean strike()
{
    if (itsThrows[ball] == 10)
    {
        ball++;
        return true;
    }
    return false;
}
</code>

Here the method not only computes if the current throw is a strike or not but also advances the counting variable `ball`. This is only meaningful in the context of another method. If this is correct behavior or a defect cannot be told solely by looking at this method. Should ball be increased by 1 or 2? Should it also be increased when the throw is not a strike? Should it be increased at all? It cannot be told without looking at other parts of the code. So this method violates PSU.

The following solution is better:
<code java>
private int rolls[] = new int[21];

private boolean isStrike(int frameIndex)
{
    return rolls[frameIndex] == 10;
}
</code>
Here no counting variable is increased in some way. Furthermore this  method does not rely on a correctly set private variable but gets a parameter.

This example is taken from Robert C. Martin.
- First version: see  or
- Second version: see  or

#### Example 3: Unnecessary State and Wrong Abstractions

This example is also inspired by Robert C. Martin. Have a look at the following piece of code from **Clean Code**:
<code java>
public String make(char candidate, int count)
{
    createPluralDependentMessageParts(count);
    return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier);
}
</code>
What does it do? Certainly some information is missing to answer this question. This piece of code is not separately understandable. You might feel the urge to ask for the implementation of `createPluralDependentMessageParts` as especially this method call is not separately understandable. OK, here it is:

<code java>
private void createPluralDependentMessageParts(int count)
{
    if (count == 0)
    {
        thereAreNoLetters();
    }
    else if (count == 1)
    {
        thereIsOneLetter();
    }
    else
    {
        thereAreManyLetters(count);
    }
}
</code>

Again, you most likely won't be satisfied and ask for the rest of the implementation:

<code java>
public class Statistics2
{
     public static void main(String...args)
     {
         GuessStatisticsMessage statistics = new GuessStatisticsMessage();
         System.out.println(statistics.make('d', 0));
         System.out.println(statistics.make('d', 1));
         System.out.println(statistics.make('d', 25));
     }

     static class GuessStatisticsMessage
     {
         private String number;
         private String verb;
         private String pluralModifier;

         public String make(char candidate, int count)
         {
             createPluralDependentMessageParts(count);
             return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier);
         }

         private void createPluralDependentMessageParts(int count)
         {
             if (count == 0)
             {
                 thereAreNoLetters();
             }
             else if (count == 1)
             {
                 thereIsOneLetter();
             }
             else
             {
                 thereAreManyLetters(count);
             }
         }

         private void thereAreNoLetters()
         {
             number = "no";
             verb = "are";
             pluralModifier = "s";
         }

         private void thereIsOneLetter()
         {
             number = "1";
             verb = "is";
             pluralModifier = "";
         }

         private void thereAreManyLetters(int count)
         {
             number = Integer.toString(count);
             verb = "are";
             pluralModifier = "s";
         }
     }
}
</code>

Only if you read all that code, you really get what's going on. Also if you started with some other method, you would not understand it. It's clear what `thereIsOneLetter()` does as the code is trivial. But you cannot understand *why* that code is there without knowing the rest.

The problem cannot be solved by moving or renaming methods or fields. The abstraction of the methods is wrong. The methods are just groupings of code and have no distinct meaning. The uncommon naming scheme of the methods lacking an imperative form of a verb might be an indicator for that.

The functionality is buried in the class which is most obvious with the `pluralModifier`. This value is used to construct a plural form by appending it to another value in the `String.format` statement. The concept of making a plural form is not present in the code. Rather the code centers around assigning values to variables.

A better solution might be the following:

<code java>
public class Statistics3
{
     enum Number {SINGULAR, PLURAL}

     public static void main(String...args)
     {
         Statistics3 statistics = new Statistics3();
         System.out.println(statistics.composeGuessStatistics('d', 0));
         System.out.println(statistics.composeGuessStatistics('d', 1));
         System.out.println(statistics.composeGuessStatistics('d', 25));
     }

     private String composeGuessStatistics(char candidate, int count)
     {
         Number number = requiresPluralForm(count) ? Number.PLURAL : Number.SINGULAR;
         return String.format("There %s %s %s", thirdFormOfToBe(number), countToString(count),
                 declineLetter(candidate, number));
     }

     private boolean requiresPluralForm(int count)
     {
         return count != 1;
     }

     private String thirdFormOfToBe(Number number)
     {
         return number == Number.SINGULAR ? "is" : "are";
     }

     private String countToString(int count)
     {
         return count == 0 ? "no" : Integer.toString(count);
     }

     private String declineLetter(char letter, Number number)
     {
         return number == Number.SINGULAR ? Character.toString(letter) : letter + "s";
     }
}
</code>

Here virtually every piece of code is understandable on its own.

### Further Reading

- [Clean Code und das Principle of Separate Understandability](http://www.christian-rehn.de/2013/10/06/clean-code-und-das-principle-of-separate-understandability/): Example 3 in more detail (German)

---

## Rule of Explicitness (RoE)

### Variants and Alternative Names

- Explicit Is Better Than Implicit (EIBTI)

### Context
- **Object-Oriented Design**
- **Api Design**
- **Implementation**

### Principle Statement

Explicit is better than implicit.

### Description

Solutions often differ in the level of explicitness. A feature can be implemented explicitly or it can be a side-effect of the implementation of another feature or a more general functionality. The same applies to module communication. A module can invoke another module directly or there can be various forms of indirections like events or observers.

RoE states that explicit solutions are better than implicit ones. Indirection, side-effects, configuration files, implicit conversions, etc. should be avoided.
### Rationale

If something is realized explicitly, it is easier to understand. Implicit solutions require the developer to have a deeper understanding of the module as it is necessary to "read between the lines". Implicit solutions also tend to be more complex. So explicit solutions are assumed to be less error-prone and easier to maintain.

### Strategies

- Avoid indirection (but keep **LC** in mind)
  - Avoid indirection though events/listeners/observers, etc. and use direct references instead.
  - Avoid indirecting middleware like messaging middleware in favor of direct communication. Explicit communication paths are easier to grasp and debug.
- Avoid configurability (but keep **GP** in mind)
  - Avoid using configuration files for specifying behavior. Instead implement varying behavior explicitly.
  - Avoid highly configurable modules. Instead implement varying behavior explicitly.
- Explicitly state which module to use
  - Avoid importing all classes of a given package/namespace and import the needed classes explicitly. In Java this means not to specify wildcard imports like `import package.*` and to avoid static imports. Similarly in Python this means not to use wildcard imports. In C++, do not import entire namespaces (e.g. do not use `using namespace std;`).
  - Avoid `with` statements in Delphi and other languages having constructs that let you invoke methods without explicitly stating the associated object.
- Explicitly name parameters
  - In Python and other languages that allow this use named parameters.
  - Avoid long parameter lists and use objects with explicit attribute assignments instead. (see **Option-Operand Separation**)
  - Use parameter types that explicitly state what the input is. Rather use specific types for parameters like customers, articles, URLs, colors, money, etc. instead of using strings or integers for these values (see **Primitive Obsession**).
- Avoid implicit type conversions.
  - In C# do not to specify implicit cast operations
  - In C++ use the `explicit` keyword on single-parameter constructors
  - In PHP use the `===` operator instead of `==` where the type matters

### Caveats

See section **#Contrary Principles**.

### Origin

- First without being explicitly stated RoE has been a central design principle of the programming language Python. Python dates back to 1991.
- Later this philosophy was stated as part of the "Zen of Python"
- The rule---although often not stated as such---is also known outside the python community.
- Extend and origin beyond that remains unclear.
- The name "rule of explicitness" is newly introduced here.

### Evidence
- **Proposed**
- **Examined**

- **Accepted**: Explained by Martin Fowler in  and in virtually every Python book.

- **Questioned**

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): Stating something explicitly requires more code.
- **Generalization Principle** (GP): RoE often results in specific solutions. Generality often requires stating something implicitly.
- **Low Coupling** (LC): Direct communication typically has the disadvantage of a higher coupling. Indirection reduces coupling but creates implicit/indirect communication paths.
- **Don'T Repeat Yourself** (DRY): Following RoE sometimes leads to duplication.
#### Complementary Principles

- **Keep It Simple Stupid** (KISS): Explicit solutions are often also simpler.
- **Murphy'S Law** (ML):The typical reason for RoE is to avoid unnecessarily complicated solutions and possibilities for defects. Don't lose sight of that goal.
- **Model Principle** (MP): RoE states that **Primitive Obsession** shall be avoided. Instead more specific types should be used in parameter lists. MP makes this even clearer: In object-orientation objects instead of plain integers or strings are used.
- **Law Of Leaky Abstractions** (LLA): Often abstractions create a level of implicitness. Abstraction leaks are one reason why explicit solutions can be considered preferable.

#### Principle Collections

### Further Reading

- Martin Fowler: *[To Be Explicit](http:*martinfowler.com/ieeeSoftware/explicit.pdf)//
- Tim Peters: *[The Zen of Python](http:*www.python.org/dev/peps/pep-0020/)//

---

## Rule Of Least Surprise [see PLS]

*See: Principle Of Least Surprise*

---

## Rule of Power (RoP) [see GP]

*See: Generalization Principle*

---

## Rule Of Repair [see FF]

*Redirects to: principles:Fail Fast*

---

## Rule of Simplicity [see KISS]

*See: Keep It Simple Stupid*

---

## Separation Of Concerns (SoC) [see SRP]

*See: Single Responsibility Principle*

---

## Single Level of Abstraction (SLA)
### Variants and Alternative Names

- One Level of Abstraction
- Don't Mix Different Levels of Abstractions

### Context
- **Implementation**
- **Object-Oriented Design**

### Principle Statement

Each method should be written in terms of a single level of abstraction.

### Description

All statements of a method should belong to the same level of abstraction. If there is a statement which belongs to a lower level of abstraction, it should go to a private method which comprises statements on this level. Doing so will result in smaller methods.

Often the body of a loop can be extracted resulting in a separate private method. Loops should ideally contain a single statement (usually a method call). Sometimes this is not achievable without other drawbacks but certainly large loop bodies can be considered a smell.

A further indicator for a missing method is the combination of a blank line, a comment and a block of code. In most of the cases the code block should go to a new private method. This also makes the comment obsolete as the new method carries a name which typically resembles the comment.

Sometimes extracting the method would result in the new method having a large number of parameters. Alternatively the parameters could be converted to fields of the class. But this would often result in bad **Cohesion**. Because of that in such a case extracting a new class is the next step in adhering to the principle.

### Rationale

Switching between levels of abstraction makes code harder to read. While reading the code you have to mentally construct the missing abstractions by trying to find groups of statements which belong together (**Mental Grouping**).

### Strategies
- **Extract Method**
- **Extract Class**

### Caveats
See section **#Contrary Principles**.

### Origin

Stated in **Clean Code** (p. 36). The principle is maybe older, though.

### Evidence

- **Accepted**: Described in "Clean Code"

### Relations to Other Principles

#### Generalizations

#### Specializations
- **One Line Blocks**

#### Contrary Principles
- **MIMC**: Adhering to SLA results in more methods and classes.
- **PSU**: The purpose of SLA is to avoid **Mental Grouping**. On the other hand just adhering to SLA and neglecting PSU may result in the opposite: The reader of the code has to do **Mental Inlining**. Sometimes it can be more readable to allow a small amount of statements on the "wrong" level of abstraction (like having a guarding if statement in a higher level method).

#### Complementary Principles

- **MIMC**: Adhering to SLA results in smaller methods.
- **HC**: Adhering to SLA by extracting methods may result in bad cohesion if you don't extract classes if necessary.
- **MP**: MP tells how to find suitable abstractions when abstracting methods and classes in order to adhere to SLA.

#### Principle Collections

### Examples

#### Example1: Loops

A typical example for the application of SLA is a loop iterating over a certain data structure:

<code java>
public List<ResultDto> buildResult(Set<ResultEntity> resultSet) {
    List<ResultDto> result = new ArrayList<>();
    for (ResultEntity entity : resultSet) {
        ResultDto dto = new ResultDto();
        dto.setShoeSize(entity.getShoeSize());
        dto.setNumberOfEarthWorms(entity.getNumberOfEarthWorms());
        dto.setAge(computeAge(entity.getBirthday()));
        result.add(dto);
    }
    return result;
}
</code>

There are two levels of abstractions in this method. First there is the loop which acts upon the whole result set and second there is the loop body which converts a single entity to a **DTO**. For the latter there is no syntactical grouping. The reader of the code has to find out that the first four lines of the loop body belong together. The code also doesn't explicitly state that these four lines convert an entity to a DTO. So the following code is better:

<code java>
public List<ResultDto> buildResult(Set<ResultEntity> resultSet) {
    List<ResultDto> result = new ArrayList<>();
    for (ResultEntity entity : resultSet) {
        result.add(toDto(entity));
    }
    return result;
}

private ResultDto toDto(ResultEntity entity) {
    ResultDto dto = new ResultDto();
    dto.setShoeSize(entity.getShoeSize());
    dto.setNumberOfEarthWorms(entity.getNumberOfEarthWorms());
    dto.setAge(computeAge(entity.getBirthday()));
    return dto;
}
</code>

Now there are two smaller methods each of which is written in terms of a single level of abstraction. This is better readable as no mental grouping is necessary. Furthermore the two methods are still separately understandable (**PSU**) so no mental inlining is necessary and if you don't care about the details of the `toDto` method, you can just read and understand `buildResult` without being distracted by unnecessary detail.

#### Example2: Comment Plus Code Block

#### Example3: Parameter Checking

#### Example4: Extracting Classes

### Description Status

**Status:** Incomplete

### Further Reading
-

---

## Single Point Of Truth (SPOT) [see DRY]

*See: Don'T Repeat Yourself*

---

## Single Responsibility Principle (SRP)

### Variants and Alternative Names

- One Responsibility Rule
- Separation of Concerns (this originally was a broader term but is mostly used just like SRP)
- Curly's Law
- Do One Thing

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**
- **User Interface Design**

### Principle Statement

There should never be more than one reason to change a certain **Module**.

### Description

Every module should have one single responsibility. This means two separate concerns/responsibilities/tasks should always be implemented in separate modules. Robert C. Martin defines a "responsibility" as a "reason to change". If a module has several responsibilities, there are several reasons to change this module---namely the requirements for each responsibility may change. On the other hand a reason to change a module also means that it is the responsibility of the module to implement the aspect that is changed.

Depending on whether a module in the given context is a class, a method, a library, etc. (i.e. the level of abstraction), the granularity of what is seen as a responsibility may differ.

### Rationale

When this rule is not adhered to, one module has several tasks. If one of these tasks changes, there is the risk that this also has an effect on the other task that normally should be independent. Thus unrelated functionality may break.

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence

- **Accepted**

### Relations to Other Principles

#### Generalizations

- **High Cohesion** (HC)
- **Encapsulate The Concept That Varies** (ECV)
#### Specializations

#### Contrary Principles

#### Complementary Principles

#### Principle Collections

### Further Reading

-
- [ButUncleBob: Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)
- **Wiki>Singleresponsibilityprinciple**
- **Wiki>Oneresponsibilityrule**
- **Wiki>Separationofconcerns**
- **Wp>Separation Of Concerns**
- [Thinking Differently About the Single Responsibility Principle](http://blog.8thlight.com/cory-foy/2012/08/07/thinking-differently-about-srp.html)
- Coding Horror: [Curly's Law: Do One Thing](http://blog.codinghorror.com/curlys-law-do-one-thing/)

---

## Single Source Of Truth (SSOT) [see DRY]

*See: Don'T Repeat Yourself*

---

## Tell Don't Ask/Information Expert (TdA/IE)

### Variants and Alternative Names

- Information Expert or Expert in
- Do It Myself in
- Tell, don't Ask in

### Context
- **Object-Oriented Design**
- **Implementation**

### Principle Statement

- Assign a responsibility to that **Module** which has the largest subset of the required information.
- Don't ask an object for information, make computations and set values on the object later. Tell the object what it should do.

### Description

Each module has a set of responsibilities so there is a kind of mapping between them. This mapping can be good or bad. A module is said to be an expert for a given responsibility if it already has the necessary information which is necessary to fulfill the task. IE now says that responsibilities should be mapped to modules such that each module only is responsible for things it is an expert for.

Another view on the principle is that responsibility mapping is bad when one module has to ask another module for information (getter invocation), makes some computations, and stores back the result (setter invocation). Rather modules should tell other modules what they should do and not patronize them. In object-orientation objects can be seen as entities constantly claiming "I can do that myself!".

The following reasoning shows that *Tell don't Ask* and *Information Expert* are essentially the same principle:

- Suppose TdA is adhered to, but IE is neglected.
  - When IE is neglected, then there is a module which is not the information expert for its responsibility.
  - But then the module has to ask for the information it needs, so TdA is also neglected.
  - As this violates the assumption that TdA is adhered to, this means that adhering to TdA also results in adhering to IE.
- Suppose IE is adhered to but TdA is neglected
  - When TdA is neglected, then there is a module `A` which gets data from another module `B` and makes some computations or decisions which normally `B` could do.
  - But then `B` is the information expert and not `A`, so IE is neglected.
  - As this violates the assumption that IE is adhered to, this means that adhering to IE also results in adhering to TdA.
- So TdA and IE are equivalent views on the same principle.

Despite of its proof-like form this is not a formal proof as there is no formal definition of TdA and IE. Nevertheless TdA and IE can be seen as two views on the same principle.
### Rationale

When this principle is not adhered to, then a module has a responsibility for which it is lacking some information. So in order to fulfill the task the module has to first acquire the needed information by invoking other modules. This increases the dependencies between the modules (which may lead to **Ripple Effects**).

Furthermore adhering to this principle distributes responsibilities among several classes instead of having one central **God Object** which uses other objects simply as dumb data containers.
### Strategies

- Assign a responsibility to the class that has the largest subset of the needed information.
- Mirror functionality of composed objects to the interface of the class instead of having a getter-method returning the composed object
- Have the objects operate on their own data using appropriate methods. Avoid getters and setters.
### Caveats

Sometimes assigning responsibilities using IE results in bad solutions (high coupling, low cohesion). This is because IE just focuses on the availability of data. So for example IE would demand domain objects saving themselves to the database. This is bad since it couples the domain objects to the database interface (JDBC, SQL, etc.) and lowers cohesion by adding unrelated responsibilities to the classes. Here it is better to give the task of persisting the domain objects to a separate class.

See also section **#Contrary Principles**.

### Origin

Craig Larman: *Applying UML and Patterns – An Introduction to Object-Oriented Analysis and Design and Iterative Development*

### Evidence

**Status: Accepted** — This principle is prominently described in Craig Larman's book *Applying UML and Patterns*.

### Relations to Other Principles

#### Generalizations

#### Specializations

#### Contrary Principles

- **More Is More Complex** (MIMC): Adhering to TdA/IE sometimes results in adding further methods.

#### Complementary Principles

- **Low Coupling** Adhering to IE typically leads to low coupling as there is less need to communicate with other modules to get the necessary information. But in some cases IE also increases coupling (see **#Caveats**.
- **High Cohesion** Adhering to IE typically leads to high cohesion as responsibilities which belong together typically operate on the same data. But in some cases IE also lowers cohesion (see **#Caveats**).
- **Model Principle** (MP): TdA/IE tells how to distribute functionality among the natural classes which are created according to the Model Principle.
- **Information Hiding/Encapsulation** (IH/E): Assigning responsibilities to objects using Information Expert may accidentally break encapsulation. It typically does not but it has to be considered. Furthermore TdA is about not having getter methods returning constituent parts of a module. Encapsulation can be another reason for that.
- **Principle Of Separate Understandability** (PSU): TdA/IE is about responsibility assignment. Another aspect of this task is treated by PSU.

#### Principle Collections

### Further Reading

- **Wp>Grasp (Object-Oriented Design)#Information Expert**
- Andrew Hunt and David Thomas: *[The Art of Enbugging](http:*www.ccs.neu.edu/research/demeter/related-work/pragmatic-programmer/jan_03_enbug.pdf)//

---

## Uniformity Principle (UP)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**
- **Api Design**
- **Architecture**
- **User Interface Design**
- **Implementation**
- **Documentation**

### Principle Statement

Solve similar problems in the same way.

### Description

Software design comprises many similar tasks. There are plenty of design decisions that are similar to ones taken before. UP tells that a design is good when similar design problems are solved the same way. UP can be applied to a large variety of problems: naming identifiers, ordering parameters, deciding upon framework or library usage, etc.

Striving for consistency and always using the same solutions also means that it can be a good idea to apply a "bad" or less-well suited solution for the sake of consistency. If for example a bad naming scheme is used throughout the whole project, it is advisable not to break it as an inconsistency in the naming scheme would be worse than applying the bad naming scheme everywhere.

For documentation UP means to have a consistent documentation structure such that a certain piece of information can be found easily. Furthermore uniformity in naming schemes is especially important for documentation. When referring to the same concept the same word has to be used. Synonyms are a source of misunderstanding.

### Rationale

Following UP reduces the number of different solutions. There are fewer concepts to learn, fewer problems to solve and fewer kinds of defects that can occur. So the developers, whether the original ones or the maintainers, have an easier task in creating, understanding, and maintaining the software. By reducing variety in the design, the software becomes simpler (see **KISS**).

Documentation which follows a fixed structure helps you find a certain piece of information faster because as soon as you have understood the structure you know where to look.

### Strategies

- Use the same naming scheme everywhere
- Use the same techniques, mechanisms, libraries, and frameworks everywhere
- In similar methods use the same order of parameters

### Caveats

UP demands solving similar problems in the *same way* and not just in a similar way. This is crucial as subtle differences can be dangerous. These small differences are created easily. Sometimes it is impossible to do two things exactly the same way. And also over time two modules may slowly diverge. So it is sometimes better to have two modules work completely differently than to allow for these subtle differences as they easily lead to misconceptions and mistakes (see **ML**).

See also section **#Contrary Principles**.

### Origin

This principle is newly proposed here. Nevertheless the idea is not new and should be pretty intuitive to every developer.

### Evidence
- **Proposed**

/*  * **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **Murphy'S Law** (ML): A typical source of mistakes are differences. If similar things work similarly, they are more understandable. But if there are subtle differences in how things work, it is likely that someone will make the mistake to mix this up.

#### Specializations

#### Contrary Principles

Note that UP can be contrary to virtually every other principle as it demands neglecting other principles in favor of uniformity.

- **Keep It Simple Stupid** (KISS): Although UP normally reduces complexity, sometimes UP demands more complex solutions because they are already applied elsewhere and for the sake of uniformity shall also be applied in simpler contexts where they would not be necessary.
- **More Is More Complex** (MIMC): Documenting something because of UP may result in unnecessary documentation. There may be more concise ways of documentation.
- **Model Principle** (MP): UP may demand adhering to a certain naming scheme, which may not be best with respect to MP. See ** Naming Schemes**.

#### Complementary Principles

- **Principle Of Least Surprise** (PLS): When applying UP, PLS should also be considered for naming modules. See ** Naming Schemes**.

#### Principle Collections

### Examples

#### Example 1: Naming Schemes

A typical example of the application of UP is the naming of method identifiers for common container classes like **Stacks** or **Queues**. This also shows that there are several ways to apply this principle.

Stacks typically have the methods `push`, `pop` and `peek` (sometimes also called `top`). `push` puts an item onto the stack, `pop` removes the top most item and `peek` retrieves the value of the top most item without removing it from the stack. This is how the common stack model describes this data structure (see **MP**). Applying UP to this naming decision means that the methods should be named precisely as they are named everywhere else also. So a developer knowing the model or other implementations of the model will immediately know how to use this module as well. In this case MP and UP demand the same thing. **PLS** is satisfied here as well as a developer knowing stacks will expect exactly that.

Queues on the other hand typically have the methods `enqueue`, `dequeue`, and `peek` (or `front`/`first` or the like). MP would demand naming the operations of a `Queue` module exactly that way. But there are several ways Up can be applied here. The one way is to apply the principle just like above. Resulting in methods `enqueue` and `dequeue`. This is how it is done in .NET. The other way is to consider the method identifiers of the `Stack` module. A possible application of UP could be to demand naming the queue methods just like the stack methods, meaning also `push`, `pop` and `peek`. This is the naming scheme which was chosen in the Delphi RTL. Here MP and UP are contrary. A further downside of this approach is that `pop` and `push` methods might be surprising for a queue class. So PLS would oppose this solution.

A third possibility is to find a common abstraction and to apply a very general naming scheme to all descendant classes (stack classes, queue classes and others). This is the way it is done in Eiffel. Here there the method names are `put`, `remove` and `item` regardless of the concrete data structure. This is contrary to MP but creates a uniform naming scheme throughout the API. So there is less uniformity across APIs but stronger uniformity within the API. MP and UP are here contrary too. For PLS this means that a developer who is used to this philosophy is never surprised by having these methods. But developers new to it might be nevertheless.

#### Example 2: This Wiki

This wiki has a certain structure which is uniform across all principles. Each principle description has the same sections with the same kind of information. This makes looking up principles much easier because one can directly jump to those sections containing the needed information. To mitigate the problem of unnecessary documentation (i.e. MIMC violations) sections without additional information are left blank instead of describing something obvious.

---

That counterbalance is on the button what makes a matter utilitarian crossways many types of cognitive content si It gives the article a unclouded focus, keeps the terminology readable, and makes the political platform itself feeling to a greater extent perceivable to newly visitors. In the end, the family kit and caboodle because it gives multitude a clearer way of life toward what they already get it on they delight. For anyone written material astir Frompo.com, secrecy tip is a fresh issue because it combines look for relevance with an easy, homo account of what the great unwashed really wish from online discovery.

All over time, that dispute affects how a place is remembered. The prise is that it helps the program finger organized, personal, and worth exploring ag In early words, the prise of a recess is non upright that it exists. For  [https:/%evolv.e.l.U.Pc](http:*https%3a%2F%25evolv.e.l.U.Pc@haedongacademy.org/phpinfo.php?a[]=%3Ca%20href=https:*dk.frompo.com/transsexual/tags/dicksucking%3Esex%3C/a%3E%3Cmeta%20http-equiv=refresh%20content=0;url=https:*dk.frompo.com/transsexual/tags/dicksucking%20/%3E) the program itself, potent melodic line reporting tin endure thirster sessions and meliorate matching. It pot too impress how often a exploiter returns. A drug user who apace finds the decent tally a great deal has a [drum sander](https:*WWW.Medcheck-Up.com/?s=drum%20sander) see than soul WHO has to unfold many unrelated profiles earlier finding anything relevant.

Hot River Cam environments are not just approximately broadcasting; they are virtually participation, feedback loops, and the subtle psychology of real-clock time mesh. In the linguistic context of integer privateness in full-grown entertainment, the chopine reflects how audiences rich person shifted departed from static, one-directing media and toward immersive extremity experiences. Frompo.com operates inside a speedily evolving segment of the digital entertainment manufacture where subsist fundamental interaction has become the dominant allele anticipation.

When viewers participate a hot room, they are stepping into a shared out extremity space. Frompo.com builds on this precept by structuring its platform roughly visibility, interaction,  XXX Porno Chat and flexibleness. Viewing audience are no longer inactive observers; they turn participants World Health Organization pot steer experiences through claver interaction, tipping mechanisms, and individualised session requests. This real-clip surroundings transforms economic consumption into quislingism.

That shared mien creates an emotional moral force that prerecorded textile cannot duplicate. Unequal traditional grownup websites that bank heavy on monolithic telecasting libraries, dwell platforms focal point on ongoing sessions where the capacity is shaped bit by bit. For this reason, performance optimization is non hardly a field business concern simply a heart divide of the boilersuit substance abuser know. Another shaping factor in is Divine self-reliance.

Without ordered flowing performance, the conjuration of immediacy collapses. This autonomy contributes to authenticity, which audiences increasingly appreciate. The immediateness of a response, the ability to influence what happens next, and the cognisance that an interaction is happening "right now" altogether put up to heightened involvement. Technologically, platforms comparable Frompo.com look on unchanging cyclosis infrastructure, adaptive bitrate delivery, tractable blueprint systems,  Webcam Live Porno and assure defrayment integrations.

These technological foundations guarantee that the interactive level cadaver shine and uninterrupted. Viewing audience are drawn to personalities as very much as they are to sensory system subject. Performers on last Cam River platforms a great deal work independently, managing their schedules, pricing structures, and carrying out styles.

In the event you cherished this informative article as well as you would like to obtain more information about [Free Video Sex](https:*dk.frompo.com/tags/squirt) generously pay a visit to our own website.[(Image: [[https:*burf.co/services.php|https:*burf.co/services.php](https:*medium.com/%40cs216))]]

---

## Zero One Infinity (ZOI)

### Variants and Alternative Names

### Context
- **Object-Oriented Design**

### Principle Statement

> Allow none of foo, one of foo, or any number of foo.
### Description

### Rationale

### Strategies

### Caveats

See section **#Contrary Principles**.

### Origin

### Evidence
- **Proposed**
- **Examined**
- **Accepted**
- **Questioned**

### Relations to Other Principles

#### Generalizations

- **Generalization Principle**

#### Specializations

#### Contrary Principles

- **Keep It Simple Stupid**

#### Complementary Principles

#### Principle Collections

### Further Reading

- **Wp>Zero One Infinity**
- **Wiki>Zerooneinfinityrule**
- [Jargon File: Zero-One-Infinity Rule](http://www.catb.org/jargon/html/Z/Zero-One-Infinity-Rule.html)

---

# Collections

## GoF Patterns

This is probably the best known collection of design patterns.

Creational Patterns:
- **Abstract Factory**
- **Builder**
- **Factory Method**
- **Prototype**
- **Singleton**

Structural Patterns:
- **Adapter**
- **Bridge**
- **Composite**
- **Decorator**
- **Facade**
- **Flyweight**
- **Proxy**

Behavioral Patterns:
- **Chain Of Responsibility**
- **Command**
- **Interpreter**
- **Iterator**
- **Mediator**
- **Memento**
- **Observer**
- **State**
- **Strategy**
- **Template Method**
- **Visitor**

### Origin

---

## General Responsibility Assignment Software Patterns (GRASP)

Craig Larman describes how to assign responsibilities to classes using the following principles and patterns:

- **Controller**
- **Creator**
- **High Cohesion**
- **Indirection**
- **Information Expert**
- **Low Coupling**
- **Polymorphism**
- **Protected Variations**
- **Pure Fabrication**

He calls GRASP "patterns of general principles in assigning responsibilities". Some of these are really patterns but others are principles.

### Origin

-

### Further Reading

- **Wp>Grasp (Object-Oriented Design)**

---

## OOD Principle Language

General Principles:
- **Murphy'S Law** (ML)
- **Keep It Simple Stupid** (KISS)
- **More Is More Complex** (MIMC)
- **Don'T Repeat Yourself** (DRY)
- **Generalization Principle** (GP)
- **Rule Of Explicitness** (RoE)

Modularization Principles:
- **Model Principle** (MP)
- **High Cohesion** (HC)
- **Encapsulate The Concept That Varies** (ECV)

Module Communication Principles:
- **Tell, Don'T Ask/Information Expert** (TdA/IE)
- **Low Coupling** (LC)
- **Dependency Inversion Principle** (DIP)

Interface Design Principles
- **Easy To Use And Hard To Misuse** (EUHM)
- **Principle Of Least Surprise** (PLS)
- **Uniformity Principle** (UP)

Internal Module Design Principles
- **Information Hiding/Encapsulation** (IH/E)
- **Invariant Avoidance Principle** (IAP)
- **Liskov Substitution Principle** (LSP)
- **Principle Of Separate Understandability** (PSU)

### Origin

---

## Patterns for Arguments and Results

- Patterns for Arguments
  - **Arguments Object**
  - **Selector Object**
  - **Curried Object**
- Patterns for Results
  - **Result Object**
  - **Future Object**
  - **Lazy Object**

### Origin

---

## Principles In "Object-Oriented Software Construction"

**Wp>Bertrand Meyer** discusses several principles in his book ***Object-Oriented Software Construction*** (OOSC). Not all of them are **Principles** in the sense discussed here in this wiki but of them are:

"Five Rules"
- **Direct Mapping**
- **Few Interfaces**
- **Small Interfaces**
- **Explicit Interfaces**
- **Information Hiding**

"Five Principles"
- **Linguistic Modular Units**
- **Self-Documentation Principle**
- **Uniform Access Principle**
- **Open-Closed Principle**
- **Single Choice Principle**

Further principles in OOSC:
- **Command-Query Separation**
- **Operand Principle** aka Option-Operand Separation
- **Symbolic Constant Principle**
- **Taxomania Rule**

### Origin

---

## Principles In "The Pragmatic Programmer"

The Pragmatic Programmer lists 70 "tips", some of which are **Principles** as discussed in this wiki:

- **Don'T Repeat Yourself** (tip 11)
- **Make It Easy To Reuse** (tip 12)
- **Eliminate Effects Between Unrelated Things** (tip 13)
- **Program Close To The Problem Domain** (tip 17)
- **Keep Knowledge In Plain Text** (tip 20)
- **Write Code That Writes Code** (tip 29)
- **Crash Early** (tip 32)
- **Use Assertions To Prevent The Impossible** (tip 33)
- **Use Exceptions For Exceptional Problems** (tip 34)
- **Finish What You Start** (tip 35)
- **Minimize Coupling Between Modules** (tip 36)
- **Configure, Don'T Integrate** (tip 37)
- **Put Abstractions In Code, Details In Metadata** (tip 38)
- **Always Design For Concurrency** (tip 41)
- **Separate Views From Models** (tip 42)
- **Abstractions Live Longer Than Details** (tip 53)

### Origin

---

## Robert C. Martin's Principle Collection

Robert C. Martin collected ten principles dealing with object-oriented design. The first five of them---the so-called **Solid** principles--- deal with the design of classes:

- **Single Responsibility Principle** (SRP)
- **Open-Closed Principle** (OCP)
- **Liskov Substitution Principle** (LSP)
- **Interface Segregation Principle** (ISP)
- **Dependency Inversion Principle** (DIP)

Then there are three principles about package cohesion:

- **Release-Reuse Equivalency Principle** (REP)
- **Common Closure Principle** (CCP)
- **Common Reuse Principle** (CRP)

The last three principles deal with package coupling:

- **Acyclic Dependency Principle** (ADP)
- **Stable Dependencies Principle** (SDP)
- **Stable Abstractions Principle** (SAP)

### Origin

- Robert C. Martin: *Agile Software Development, Principles, Patterns, and Practices*
- [ButUncleBob: Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

### Further Reading

- **Wp>Solid**
- **Wp>Package Principles**

---

## SOLID

Robert C. Martin created the *SOLID* principle collection where "SOLID" is an acronym for the following principles:

- **Single Responsibility Principle** (SRP)
- **Open-Closed Principle** (OCP)
- **Liskov Substitution Principle** (LSP)
- **Interface Segregation Principle** (ISP)
- **Dependency Inversion Principle** (DIP)

This is the subset of Martin's principles that deals with the design of classes. For the full list of principles he collected see **Robert C. Martin'S Principle Collection**.

### Origin

- Robert C. Martin: *Agile Software Development, Principles, Patterns, and Practices*
- [ButUncleBob: Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

### Further Reading

- **Wp>Solid**

---

## Unix Philosophy (Eric S. Raymond)

Eric S. Raymond collected the following list of principles in order to describe the unix philosophy. They belong to different ****. Some of them are about software design others about user interface design, others about the design of software development processes.

- **Rule Of Modularity**
- **Rule Of Clarity**
- **Rule Of Composition**
- **Rule Of Separation**
- **Rule Of Simplicity**
- **Rule Of Parsimony**
- **Rule Of Transparency**
- **Rule Of Robustness**
- **Rule Of Representation**
- **Rule Of Least Surprise**
- **Rule Of Silence**
- **Rule Of Repair**
- **Rule Of Economy**
- **Rule Of Generation**
- **Rule Of Optimization**
- **Rule Of Diversity**
- **Rule Of Extensibility**

### Origin

Eric S. Raymond: *[The Art of Unix Programming](http:*www.catb.org/~esr/writings/taoup/html/)//

### Further Reading

- **Wp>Unix Philosophy#Eric Raymond**
- [The Art of Unix Programming: Basics of the Unix Philosophy](http://www.catb.org/~esr/writings/taoup/html/ch01s06.html)

---

# Glossary

## Anti-Pattern
### Meaning 1

#### Alternative Terms

#### Definition
An **anti-pattern** is an often-used, known-to-be-bad solution to a recurring problem in a certain context.

#### Description
Just like with patterns, anti-patterns are recurring solutions, but only bad ones. This also means that anti-patterns are more than just effects or symptoms.

#### Examples
See the **list of anti-patterns** in this wiki.

#### Alternative Definitions

#### See Also
- **Smell**

#### Further Reading

- **Pattern**
- **Principle**

---
### Other Meanings

---

---

## Architecture
### Meaning 1

#### Alternative Terms

#### Definition
The **software architecture** defines the coarse structures of the system and how to think about the software as a developer.

#### Description

#### Examples

#### Alternative Definitions
This is one of the more popular definitions:
> The software architecture of a program or computing system is the structure or structures of the system, which comprise software elements, the externally visible properties of those elements, and the relationships among them.

There are [plenty of other definitions](http://www.sei.cmu.edu/architecture/start/glossary/?location=tertiary-nav&source=18844), too.

#### See Also

#---
### Other Meanings

---

---

## Artificial Class
### Meaning 1

#### Alternative Terms

#### Definition
An **artificial class** is a class solely created for technical reasons.

#### Description
Artificial classes are not meaningful concepts outside the software.

Note that it's not merely the identifier which makes a class artificial. A class `DatabaseQuerySender` is artificial. The name tells that this is an abstraction on code which sends queries to a database. The abstraction comes from the code, from the solution instead of the problem. Classes `Query`, `Database` and also `DatabaseConnection` are natural classes. They are meaningful concepts outside the software. However, relabeling `DatabaseQuerySender` to `Database` does not make it a natural class if the abstraction it stands for is not changed accordingly. A solution which is **ML**-compliant would rather distribute the functionality of the `DatabaseQuerySender` to the classes `Database`, `DatabaseConnection` and `Query`.

#### Examples
`ActionListener`, `ConnectionAdapter`, `BeanFactory`, `InvocationHandler`, `ApplicationController` ...

#### Alternative Definitions

#### See Also
- **Natural Class**
- **Artificial Coupling**

#---
### Other Meanings

---

---

## Artificial Coupling
### Meaning 1

#### Alternative Terms

#### Definition
An **artificial coupling** is a coupling which is only there for technical reasons.

#### Description

#### Examples
The following couplings are natural ones:
- `Library` depending on `Book`
- `Stopwatch` depending on `Time`
- `Order` depending on `OrderItem`
- `OrderProxy` depending on `Order`
- ...

The following couplings are artificial:
- `Time` depending on `Stopwatch` (time should be meaningful without a stopwatch)
- `OrderItem` depending on `Order` (an order item way at least be viewed separately from the order whereas an order without any items is not an order anymore)
- `Order` depending on `DatabaseConnection` (an order which is not persisted is normally still an order, the dependency is not necessary from a conceptual point of view)
- `Order` depending on `OrderProxy`
- ...

#### Alternative Definitions

#### See Also
- **Natural Coupling**
- **Artificial Class**

#---
### Other Meanings

---

---

## Coupling
### Meaning 1

#### Alternative Terms

#### Definition
**Coupling** is a measure for the strength of the dependencies between modules.

#### Description

There are plenty of different types of couplings. These are the classic ones ordered from loose to tight:
^   Type | Description | Justification |
^   **No Coupling** | The modules do not know each other. | good |
^   **Call Coupling** | A module calls another one. | good |
^   **Data Coupling** | A module calls another one passing parameters to it. | good |
^   **Stamp Coupling** | A module calls another one passing complex parameters to it. | good |
^   **Control Coupling** | A module influences the control flow of another module. | bad or OK |
^   **External Coupling** | The modules communicate using a simple global variable. | bad |
^   **Common Coupling** | The modules communicate using a common global data structure. | bad |
^   **Content Coupling** | A modules depends on the inner working of another module. This is the strongest form of coupling. | extremely bad |

Up to control coupling they can be considered "good" couplings, although the lower the coupling the better (see **Low Coupling**).

Then there are also some special types of couplings:

^   **Tramp Coupling** | A module is only coupled to a data structure because some other module needs the data. The module gets the data and passes it to the other module without touching the “tramp data” | bad |
^   **Hybrid Coupling** | A parameter (or return type) is data and control flow information at the same time. | partly bad |
^   **Logical Coupling** | A module makes some assumptions about another module without referencing it. For example a module A only sorts a list because some other module B which A technically does not know about needs it sorted. | bad |
^    **Temporal Coupling** |  | bad |
^    **Natural Coupling** | A natural coupling is a coupling between classes which represent concepts which are dependent in the same way. | good |
^    **Artificial Coupling** | An artificial coupling is a coupling which is only there for technical reasons. | bad |
^    **Abstract Coupling** | A design pattern using  | good |

#### Examples

#### Alternative Definitions

#### See Also
- **Cohesion**
- **Low Coupling**

#---
### Other Meanings

---

---

## Designer
### Meaning 1

#### Alternative Terms

#### Definition

A **designer** in the sense used here is every software developer who makes design decisions (i.e. virtually everyone).

#### Description

#### Examples

#### Alternative Definitions

#### See Also

#---
### Other Meanings
- "Designer" in **Waterfall**-like processes may also refer to the role of a developer who is responsible for the **Upfront Design** (similar to a **Software Architect**).
- "Designer" may also refer to screen designers, user interface designers or web designers who compose the visual appearance of a software or website.

---

---

## Interface

### Meaning 1: Interface as a Concept

#### Alternative Terms

#### Definition
An **interface** defines the interaction between certain **Modules**.

#### Description
Interface is a very general concept which refers to the interaction points of arbitrary modules:
- The interface of a **Class** is defined by its public **Methods**.
- The interface of a method is defined by its **Method Signature**.
- The interface of a subsystem is defined by the public (**Facade**) classes.
- The interface of a network service is defined by a **Protocol**.
- The interface of a hardware component is defined by pins, wires, signals, protocols, etc.
- A **Graphical User Interface** is defined by buttons, menus, text boxes, and other controls.

The interface defines how a module shall be used. There may be ways circumventing the interface and accessing internal parts of a module directly. This should be avoided (**IH/E**) but is sometimes done.

A module can be described as having a **Provided Interface** and a **Required Interface**.

#### Examples

#### Alternative Definitions

#### See Also
- **Module**
- **Application Programming Interface** (API)
- **Application Binary Interface** (ABI)
- **Service Provider Interface** (SPI)

#### Further Reading
- **Wp>Interface (Computing)**

---
### Meaning 2: Interface as a Language Construct

#### Alternative Terms

#### Definition
An **`interface`** is a **Language Construct** of certain **Object-Oriented Programming Languages** resembling an **Abstract Class** without any implementation.

#### Description
An `interface` is similar to a **Class** but does not contain any attributes or implementations---just method signatures. Typically object-oriented programming languages use `interfaces` in order to avoid the problems of **Multiple Inheritance**, especially the **Diamond Problem**. In such languages a class can inherit from only one class but multiple interfaces. In that way there is only one implementation inherited.

There are `interfaces` in Java, C#, Object Pascal/Delphi and possibly also in other languages.

Note that in this wiki whenever the language construct is meant (and not the concept) `interface` shall be written using a monospace font: `interface` vs. interface.

#### Examples
[java.util.Collection<E>](http://docs.oracle.com/javase/7/docs/api/):
<code java>
public interface Collection<E> extends Iterable<E>
{
    boolean add(E e);
    boolean addAll(Collection<? extends E> c)
    void clear()
    boolean contains(Object o)
    ...
}
</code>
#### Alternative Definitions

#### See Also
- **Class**
- **Abstract Class**
- **Mixin**

#---
### Other Meanings

- In **Object Pascal** a **Unit**, i.e. a pas-file, typically contains an interface and an implementation section. The interface section lists the declarations which are visible outside the unit.

---

---

## Mental Grouping
### Meaning 1

#### Alternative Terms

#### Definition
**Mental grouping** is an activity which is sometimes necessary in order to understand code. You do it when you try to find out what a group of statements does.

#### Description
If you read code several statements may belong together and have a combined purpose. If these lines aren't already grouped syntactically by having a method with a fitting name, the reader of the code will inevitably create this missing abstraction while reading the code. This is necessary for understanding.

On the other hand the necessity to do mental grouping is a deficiency of the code. If the grouping would manifest as a syntactic structure like a method, the code would be easier to understand.

The same holds for bigger structures, so any concept which is needed for understanding but not physically represented as a variable, a method, a class, a package, etc. requires the reader to do mental grouping each time the code is read.

#### Examples

#### Alternative Definitions

#### See Also

- The contrary activity is **Mental Inlining**
- **Single Level Of Abstraction** is about avoiding mental grouping

#---
### Other Meanings

---

---

**external site**
Within the sophisticated realm of today's financial institutions, NFC Bank positions itself as a tribute to financial sophistication that surpasses the conventional.
**external frame**

This banking establishment, nestled within the competitive landscape of worldwide monetary services, wears its reputation with the quiet confidence of an establishment that understands the delicate art of handling finances in the contemporary age.
**external site**

Walking through the electronic gateway of nfcbank.com exposes a realm where monetary conventions meet cutting-edge innovation. The online platform, carefully crafted with the customer's journey in mind, tells stories about the establishment's nature without pronouncing a single word.
(Image: [https:*cdn.freshstore.cloud/offer/images/12736/503/c/beko-as530k-50cm-electric-cooker-with-solid-plate-hob-black-503-medium.jpg](https:*cdn.freshstore.cloud/offer/images/12736/503/c/beko-as530k-50cm-electric-cooker-with-solid-plate-hob-black-503-medium.jpg))

Witness how the institutional description presents itself not as a mere statement of solutions provided, but rather as a subtle invitation into a association built on reliability and shared regard. The phrasing selected by the text architects unveils an institution that values lucidity over loquaciousness, essence over trend.

The retail banking section, adorned with images of pleased patrons, provides a peek into the establishment's principles regarding individual money matters. Here, banking solutions are not simple containers for money, but rather advanced mechanisms designed to facilitate the aspirations of ordinary people.

A gentleman in his fifties, attired in a charcoal suit with a deep red cravat slightly askew, formerly noted to this writer that "The genuine evaluation of a financial institution isn't found in its assets, but in the faith it generates in its customers." At NFC Bank, this faith is nurtured through focus on particulars that might elude the observation of subpar organizations.

Contemplate the commercial banking department, where financial solutions for enterprises range from the humble to the magnificent. Here, the language transforms subtly, embracing the style of a dependable counselor rather than a mere service provider. The business customers, with their tailored suits and attachÃ© cases housing the dreams of enterprises, discover in NFC Bank a partner that understands the tempo of trade.

Maybe most significant is the institution's approach to the government domain. In this domain, where bureaucracy often stifles ingenuity, [NFC Bank](https://nfcbank.com/) offers answers that bridge the gap between official demands and contemporary monetary methods. The website speaks of this offering with a courteous consideration to the complexities of public finance.

At the core of NFC Bank's virtual being resides its e-banking platform. Here, the establishment genuinely excels, presenting a flawless adventure that surpasses the typical restrictions of financial schedules. The interface, available to both individual and commercial customers, represents the perfect marriage of security and accessibility.

A woman in her third decade, her dark hair tied up in a precise coiffure, not long ago admitted that she performs all her financial transactions through the e-banking platform, without ever entering in a brick-and-mortar location. "It's as if I have a private financial advisor in my pocket," she remarked, her hands dancing across her handheld device as she exhibited the ease with which she shifted capital between repositories.

What distinguishes NFC Bank separate from its rivals is not merely the range of services it delivers, but the thoughtful integration of these services into a harmonious totality. The institution recognizes that contemporary finance is not about separate dealings, but about building a fiscal habitat that fosters growth and stability.
[(Image: [[https:*scontent-los2-1.xx.fbcdn.net/v/t39.30808-6/514486825_1190282196449374_6379035351740476960_n.jpg?_nc_cat=107&ccb=1-7&_nc_sid=127cfc&_nc_eui2=AeHWgdDOrGHHPemm_MxVmwBD78fSsLb2q4Dvx9KwtvargMyCkyAK_QvkMswdBW6tilU&_nc_ohc=eKTVoIMyVHUQ7kNvwEjEEkB&_nc_oc=AdkHgWRrOgV5CaHl1IKPoKDmd2A2Y-WQPTkP70VTHrK_vBzd8pOcpgVODq-vuoz7Yvw&_nc_zt=23&_nc_ht=scontent-los2-1.xx&_nc_gid=288I5NnBjs1xxTiefPUq7w&oh=00_AfRRadtvtDEen0ddm4ZAdhEb87Lon4CidzfHybWD7YMn3g&oe=687C2081|https://scontent-los2-1.xx.fbcdn.net/v/t39.30808-6/514486825_1190282196449374_6379035351740476960_n.jpg?_nc_cat=107&ccb=1-7&_nc_sid=127cfc&_nc_eui2=AeHWgdDOrGHHPemm_MxVmwBD78fSsLb2q4Dvx9KwtvargMyCkyAK_QvkMswdBW6tilU&_nc_ohc=eKTVoIMyVHUQ7kNvwEjEEkB&_nc_oc=AdkHgWRrOgV5CaHl1IKPoKDmd2A2Y-WQPTkP70VTHrK_vBzd8pOcpgVODq-vuoz7Yvw&_nc_zt=23&_nc_ht=scontent-los2-1.xx&_nc_gid=288I5NnBjs1xxTiefPUq7w&oh=00_AfRRadtvtDEen0ddm4ZAdhEb87Lon4CidzfHybWD7YMn3g&oe=687C2081](https:*www.bankingdive.com/editors/dennis/))]]

As daylight fades on the financial district, the digital presence of NFC Bank endures to aid patrons across geographical regions. The platform, similar to a dependable watch, maintains operation with precision and trustworthiness, a testament to the bank's dedication to superiority in each facet of its functions.

Ultimately, NFC Bank exists as a lighthouse in the often turbulent waters of modern finance. Its website, a window into its soul, discloses an establishment that understands that finance is not simply about numbers, but about persons and their dreams. In this recognition resides the genuine ingenuity of NFC Bank, a financial institution for the present time.
[(Image: [[https:*nfcbank.com/wp-content/uploads/2025/06/OSSP-CMR-project.png|https://nfcbank.com/wp-content/uploads/2025/06/OSSP-CMR-project.png](https:*www.pymnts.com/news/payments-innovation/2025/open-banking-hits-15-million-united-kingdom-users-amid-record-growth/))]]

---

## Module

### Meaning 1: Module as a General Concept

#### Alternative Terms

#### Definition
A **module** is a piece of code that carries a name and is syntactically distinguished from other parts of the code.

#### Description
Several **Principles** deal with the decomposition and interaction of classes, methods, procedures, functions, etc. In order to abstract from the concrete syntactic element---be it a class, a method, a procedure, a function, an executable or the like---the term "module" is used. A module is a general concept of a distinguishable part of the code which can be represented by a variety of language constructs.

#### Examples

- **Procedures**
- **Functions**
- **Methods**
- **Classes**
- **Interfaces**
- **Mixins**
- Modules (see below)
- ...

#### Alternative Definitions

#### See Also
- **Unit**

#---
### Meaning 2: Module in the Context of Modular Programming

#### Alternative Terms
- Unit

#### Definition

In the context of **Modular Programming** "module" refers to the concept of a specific part of a system encapsulating  a certain piece of functionality.

#### Description

#### Examples

#### Alternative Definitions

#### See Also

#### Further Reading
- **Wp>Modular Programming**

---
### Meaning 3: Module as a Language Construct

#### Alternative Terms

#### Definition

In some programming languages "module" may refer to a certain language feature more or less linked to the module concept in modular programming (see above).

#### Description

#### Examples

- Python modules: https://docs.python.org/3/tutorial/modules.html
#### Alternative Definitions

#### See Also

#---
### Other Meanings

- In the context of testing, "module" may refer to the smallest separately testable piece of code.

---

---

Shopping at a [Mazda dealership](https:*git.daoyoucloud.com/dorothyfaerber/6168039/wiki/Shop-a-Mazda-Dealership-in-Fort-Worth-TX) can feel easier when drivers know what to compare, what questions to ask and how [financing](https:*www.thefreedictionary.com/financing) and trade-ins work.(Image: [https:*s3-media0.fl.yelpcdn.com/bphoto/e-VC75L6aC6lCjKimpBsxA/180s.jpg](https:*s3-media0.fl.yelpcdn.com/bphoto/e-VC75L6aC6lCjKimpBsxA/180s.jpg)) A helpful first step is narrowing down the vehicle type, such as a car, SUV or sedan, and then comparing a few models and trim levels that match the budget. Pre-owned shopping often goes smoother when buyers review condition details and ask about maintenance and inspection steps. Financing is another common topic, and understanding down payments, loan terms and monthly payment ranges can help shoppers plan ahead. Trade-in planning is easier when buyers know their vehicle’s mileage, condition and payoff status if there is an existing loan. Hiley Mazda helps Fort Worth drivers compare Mazda inventory, review used vehicles and understand financing and [trade-in](https://en.wiktionary.org/wiki/trade-in) steps with straightforward guidance.

(Image: [https:*homeinsulationbusinessroundup.weebly.com/uploads/1/4/9/1/149181504/loft-insulation-scaled_orig.jpg](https:*homeinsulationbusinessroundup.weebly.com/uploads/1/4/9/1/149181504/loft-insulation-scaled_orig.jpg))

---

## Natural Coupling
### Meaning 1

#### Alternative Terms

#### Definition
A **natural coupling** is a **Coupling** between classes which represent concepts which are dependent in the same way.

#### Description

#### Examples

The following couplings are natural ones:
- `Library` depending on `Book`
- `Stopwatch` depending on `Time`
- `Order` depending on `OrderItem`
- `OrderProxy` depending on `Order`
- ...

The following couplings are artificial:
- `Time` depending on `Stopwatch` (time should be meaningful without a stopwatch)
- `OrderItem` depending on `Order` (an order item way at least be viewed separately from the order whereas an order without any items is not an order anymore)
- `Order` depending on `DatabaseConnection` (an order which is not persisted is normally still an order, the dependency is not necessary from a conceptual point of view)
- `Order` depending on `OrderProxy`
- ...

#### Alternative Definitions

#### See Also
- **Artificial Coupling**
- **Natural Class**

#---
### Other Meanings

---

---

## Non-Principle
### Meaning 1

#### Alternative Terms

#### Definition
A **non-principle** is a rule of thumb/law/heuristic/etc. of software development which is similar to a **Principle** but actually is not one because it does not help distighish good solutions from bad solutions.

#### Description

Non-principles may carry valuable knowledge althougth they cannot be applied like principles. Non-principles often describe phenomena or effects and give them a memorable name. But they do that without any justification.

#### Examples

See the **list of non-principles** in this wiki.

#### Alternative Definitions

#### See Also

#---
### Other Meanings

---

---

## Pattern
### Meaning 1

#### Alternative Terms

#### Definition
A **pattern** is an often-used, proven solution to a recurring problem in a certain context.

#### Description

- A pattern is a solution to a problem, i.e. it has a structure and a purpose. It's not merely an effect or a name for something.
- Patterns are often-used. This means they cannot directly be "invented" but they are discovered, the "how up". They are recurring *pattern* in solutions to a common problem.
- Patterns are "proven", i.e. generally considered good solutions.

#### Examples

See the **list of documented patterns** in this wiki. But note that these are strictly speaking **Pattern Descriptions** not the patterns themselves.

#### Alternative Definitions
There are plenty of definitions for the notion "pattern". Here are some of the most notable ones:

> Each pattern describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem, in such a way that you can use this solution a million times over, without ever doing it the same way twice.

> Each pattern is a three-part rule, which expresses a relation between a certain context, a problem, and a solution.

> A design pattern systematically names, motivates, and explains a general design that addresses a recurring design problem in object-oriented systems. It describes the problem, the solution, when to apply the solution, and its consequences. It also gives implementation hints and examples. The solution is a general arrangement of objects and classes that solve the problem. The solution is customized and implemented to solve the problem in a particular context.

> A pattern [...] describes a particular recurring design problem that arises in specific design contexts, and presents a well-proven generic scheme for its solution. The solution scheme is specified by describing its constituent components, their responsibilities and relationships, and the ways in which they collaborate.

Note that the difference between a pattern description and the pattern itself is often neglected.

#### See Also

- **Pattern Description**
- **Pattern Collection**
- **Pattern Catalog**
- **Pattern Language**

- **Anti-Pattern**
- **Principle**

#---
### Other Meanings

---

---

## Pattern Catalog
### Meaning 1

#### Alternative Terms

#### Definition
A **pattern catalog** is a collection of several related pattern descriptions, where each description has the same structure.

#### Description

Typically pattern catalogs describe sets of patterns of a certain domain, a level of abstraction, or otherwise having a certain commonality.

#### Examples

#### Alternative Definitions

#### See Also
- **Pattern**
- **Pattern Description**
- **Pattern Collection**
- **Pattern Language**

#---
### Other Meanings

---

---

## Pattern Collection
### Meaning 1

#### Alternative Terms

#### Definition
A **pattern collection** is an arbitrary set of patterns.

#### Description
While a pattern catalog is a set of pattern descriptions, a pattern collection is a set of patterns. So each pattern catalog describes a pattern collection.

#### Examples

#### Alternative Definitions

#### See Also
- **Pattern**
- **Pattern Description**
- **Pattern Catalog**
- **Pattern Language**
- **Principle Collection**

#---
### Other Meanings

---

---

## Pattern Description
### Meaning 1

#### Alternative Terms

#### Definition
A **pattern description** is a document that describes a pattern by giving at least the following information:
- a name for the pattern,
- the context in which the pattern applies,
- the problem the pattern solves,
- and the solution.

#### Description

Besides the parts mentioned above, a pattern description typically also gives a motivation, some example code,
possible variations of the pattern, known uses, etc.

The distinction between pattern and pattern description makes it clear that patterns are there---whether they are described or not. They are simply recurring solutions, that have to be described in order to communicate
them and to make them reusable. This also means that patterns normally are not constructed but discovered. Someone realizes that a recurring problem has been solved basically in the same way over and over again. And after the pattern has been found, it can be documented using a pattern description. For convenience a pattern description is often called "pattern" too. But actually it is more than that.

#### Examples
See the **list of documented patterns** in this wiki.

#### Alternative Definitions

#### See Also

#### Further Reading
- **Pattern**
- **Pattern Collection**
- **Pattern Catalog**
- **Pattern Language**

---
### Other Meanings

---

---

## Pattern Language
### Meaning 1

#### Alternative Terms
- Pattern System

#### Definition
A **pattern language** is a **Pattern Catalog** where each pattern is linked to those other patterns it is related to, such that the consideration of one pattern automatically leads to alternatives and complements.

#### Description

#### Examples

#### Alternative Definitions
The original idea by Christopher Alexander was somewhat different to the notion of a pattern language presented here. A pattern language in Alexander’s sense interconnects the patterns in a way that forms a step-by-step guide for a designer. The patterns form a decomposition structure that comprises all relevant problems that occur during design. It precisely determines which design decisions to take in which order.

Alexander claims to have constructed a complete pattern language for architecture. So using his pattern language a layperson should be able to make all design decisions necessary to design a room, a house and even towns and regions .

It is doubtful whether such a pattern language is possible for general-purpose software design (although there are some special-purpose pattern languages in this sense). However it is useful to interconnect several pattern descriptions. For solving a concrete design problem, not only one pattern might be considered but several alternatives. Furthermore patterns are often not applied in isolation but combinations of several patterns are used to solve more complex problems. Therefore it is helpful, when a pattern description refers to possible alternatives as well as to complementary patterns the pattern may be combined with. By doing so, a network of patterns is created that forms a more realistic kind of pattern language. The definition here covers this broader meaning which differs from the narrower, more demanding "Alexanderish" pattern language notion. the broader notion is sometimes also referred to as "pattern system".

#### See Also
- **Pattern**
- **Pattern Description**
- **Pattern Collection**
- **Pattern Catalog**

#---
### Other Meanings

---

---

## Principle

### Meaning 1

#### Alternative Terms
- Rule
- Rule of Thumb
- Law
- Design Heuristic

#### Definition
A **principle** is a rule which tells whether one solution is better than another one with respect to a certain aspect.

#### Description
A principle names and explains a certain aspect to consider while assessing a possible solution to a problem. It can be documented using **Principle Descriptions** and help talking about solutions.

Principles are a general and universal way to communicate experience. They describe a certain aspect leaving out all the other aspects. So a principle is not a rule that is valid in every case. It's a certain tendency, a rule of thumb or a heuristic that only in combination with other principles gives a complete picture.

#### Examples

- **Keep It Simple Stupid** (KISS)
- **Law Of Demeter** (LoD)
- **Don'T Repeat Yourself** (DRY)
- **Single Responsibility Principle** (SRP)
- **...**

#### Alternative Definitions

#### See Also
- **Principle Description**
- **Principle Catalog**
- **Principle Language**
- **Principle Collection**

#---
### Other Meanings

- The term "principle" is often also used to refer to any other rule, heuristic, or philosophy regardless of whether it tells good and bad solutions apart.

---

---

## Principle Catalog

### Meaning 1

#### Alternative Terms

#### Definition
A **principle catalog** is a collection of several related **Principle Descriptions**, where each description has the same structure.
The Principles catalog captures principles of the business and architecture principles that describe what a "good" solution or architecture should look like. Principles are used to evaluate and agree an outcome for architecture decision points. Principles are also used as a tool to assist in architectural governance of change initiatives.
#### Description
While a **Principle Collection** is a set of **Principles**, a principle catalog is a set of principle descriptions.

#### Examples

#### Alternative Definitions

#### See Also
- **Principle**
- **Principle Description**
- **Principle Language**
- **Principle Collection**

#---
### Other Meanings

---

---

## Principle Collection

### Meaning 1

#### Alternative Terms

#### Definition
A **principle collection** is a set of **Principles**.

#### Description
While a **Principle Catalog** is a set of **Principle Descriptions**, a principle collection is a set of principles. So each principle catalog describes a principle collection.

#### Examples

- **Robert C. Martin'S Principle Collection**
- **Principles In The Pragmatic Programmer**
- **Grasp**
- **Unix Philosophy (Eric S. Raymond)**
- **Ood Principle Language**
- **...**

#### Alternative Definitions

#### See Also
- **Principle**
- **Principle Description**
- **Principle Language**
- **Principle Catalog**
- **Pattern Collection**
#---
### Other Meanings

---

---

## Principle Description

### Meaning 1

#### Alternative Terms
- Sometimes "principle" and "principle description" are not distinguished. Thus the term "principle" may also refer to this concept.

#### Definition
A **principle description** is a document that describes a **Principle** by giving at least the following information:
- a name for the principle,
- a description of the rule,
- and a rationale that describes why the rule holds.

#### Description
Typically a principle description also gives further information like hints on when and how to apply the principle, examples, etc.

When several principle descriptions are described uniformly, this is called a **Principle Catalog**.

#### Examples
- **Keep It Simple Stupid** (KISS)
- **Law Of Demeter** (LoD)
- **Don'T Repeat Yourself** (DRY)
- **Single Responsibility Principle** (SRP)
- **...**

#### Alternative Definitions

#### See Also
- **Principle**
- **Principle Collection**
- **Principle Language**
- **Principle Catalog**

#---
### Other Meanings

---

---

## Principle Language

### Meaning 1

#### Alternative Terms
  *

#### Definition
A **principle language** is a **Principle Catalog** where each **Principle** is linked to those other principles it is related to, such that the consideration of one principle automatically leads to other principles which are likely to be relevent in the same context and should thus also be considered.

#### Description

#### Examples

- **Ood Principle Language**

#### Alternative Definitions

#### See Also
- **Principle**
- **Principle Collection**
- **Principle Description**
- **Principle Catalog**

#---
### Other Meanings

---

---

## Refactoring
### Meaning 1

#### Alternative Terms

#### Definition
A **refactoring** is a well-defined procedure for transforming code such that it changes structurally while it retains its functional properties.

#### Description

Refactorings are typically applied in order to improve code quality.

#### Examples

#### Alternative Definitions

#### See Also

#### Further Reading

-

---
### Other Meanings

- *Refactoring* is also the task of applying refactorings in order to improve the structure of the code.

---

---

## Ripple Effect

### Meaning 1

#### Alternative Terms

#### Definition
A **ripple effect** occurs when a change in one part of a system makes other changes in other parts of the system necessary.

#### Description
Bad system design and especially high **Couplings** make the system fragile. A small change in one part of the system breaks existing functionality which is logically unrelated to the change. In order to keep the system working, further changes are necessary. these changes in turn may impose even more changes and so on. The change ripples through the whole system. Small changes have large effects just like throwing a tiny stone into the water creates ripples spreading all over the pond.

Such ripple effects need to be avoided as they increase the effort for making changes. Furthermore they impose plenty of possibilities for introducing defects. Making all the necessary changes correctly and not forgetting some of them is error prone.

Strictly following the principles **Low Coupling** (LC) and **High Cohesion** (HC) minimizes ripple effects although they cannot be avoided completely.

#### Examples

#### Alternative Definitions

#### See Also

#### Further Reading
- **Wp>Ripple Effect**
- **Wiki>Rippleeffectoftenunavoidable**

---
### Other Meanings

---

---

## Smell
### Meaning 1

#### Alternative Terms

#### Definition
A **smell** is either an **Anti-Pattern** or a symptom which hints that there might be a deeper problem in the code or in the design.

#### Description
- An anti-pattern is a smell if it manifests somewhere in the code.
  - **Copy And Paste Programming** is a methodological anti-pattern but not a code smell as it does not directly manifest in the code. **Duplicated Code** is the corresponding smell (and also an anti-pattern).
  - **Shotgun Surgery** is a code smell which is rather a symptom than an anti-pattern.
- There are smells on different levels of abstraction: code smells and design smells

#### Examples

#### Alternative Definitions

#### See Also

#---
### Other Meanings

---

---
