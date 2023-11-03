# ACTN POI plluggable week (October 31st,2023)


****Attendees****
- [] Dieter Beller
- [x] Jean-Francois Bouquier
- [x] Daniele Ceccarelli
- [] Ori Gerstel
- [x] Aihua Guo
- [] Esther Lerouzic
- [] Julien Meuric
- [] Italo Busi
- [] Gert Grammel
- [x] Gabriele Galimberti 
- [] Yuji Tochio
- [] Enrico Griseri
- [] Sergio Belotti
- [] Victor Lopez
- [] Haomian Zheng
- [] Igor Bryskin
- [] Dirk Schroetter
- [] Brent Foster
- [] Jiang Sun (CMCC)
- [x] Prasenjit Manna (cisco)
- [x] Jose' Angel Perez
- [] Peter Landon
- [] Nigel Davis (Ciena)
- [] Oscar Gonzalez de Dios
- [x] Fabio Peruzzini
- [] Gene
- [] Manuel Lopez
- [] Roberto Manzotti
- [] Paolo Volpato
- [] Bradley Riapolov
- [x] Phil Bedard
- [] Reza Rokui
- [x] Nigel davis


## Admin

draft-poidt-ccamp-actn-poi-pluggable-02.txt run for IPR poll to be WG doc. 

IETF-118 draft submission cut-off Oct. 23rd (name change ?)
IPR is not yet complited, 
Fabio Peruzzini will be added as a contributor,
also draft title must be fixed.
The proposal is to add Fabio and generate -03 version. 
-03 version will be ranemed in -00 WG draft.

A request to nominate a chair outside ccamp and the authors to manage the drafts (either draft-poidt-ccamp-actn-poi-pluggable and draft-davis-ccamp-photonic-plug-control-arch).
Jilien Meuric accepted to co-ordinate the several drafts on the table.


### Review APs (action point) 

Daniele on the IPR status, still running.

Jeff added some text to MD file, Jeff to share it ?
No other comments suggestions.

### Closed Issues


### Next calls
Next call will be regular on Nov. 14th, 2023 ?

#### General discussion

Discussion on Option 1 and 2 applicability and benefits opposed to Option 3 (Nigel draft).  The debate is still open among Optila experts and would require the IP network expersts view on how to manage Pluggable in Router network.

A side meeting will be arranged in IETF Prague to further discuss about it the options.
 
---------- 
OIF to propose a separate channel to manage/control the pluggables (Nigel/Oscar to give more visibility), although this is a 2 years far proposal.
OIF IPRs do not allow to share yet any info on the above matter.
The activity was just announced, we'll see any further detail on it.

Jeff warns to keep the solution simple (at least for the moment) so we preserve and lower the iterop issues (that is option 1 and 2).
Jeff clarified He was concerned about the interoperability among the vendors.
Feedbacks on Nigel proposal will come soon (option 3).

Restart the discussion on the Arch and work on the ACTN POI Coherent Module Yang model proposal (between PNC and MDSC). Phil Bedard made a proposal.

Preliminary Comments on Phil are suggesting to keep a good alignment to IETF models (i/f Yang and TOpology), T-API, and
OpenConfig manifest. 
Action is to work with Phil on this direction and make the gap analisys (Daniele and Gab. and Italo action).

Discussion on gap analisys:
- Tunnel model definition from the router perspective, either   from WSON and SSON side.
- Address either the Pluggable and the Integrated transceivers.
- Associate the Optical tunnel to the Ethernet link
- L1 (OTN), L2 (Eth), "inverse multiplexing" cases (?)

Action to All: raise any usecase not yet mentioned in the discussion.

Scope of the draft (e.g. metro, long haul, network configuration, etc.) to be clarified. 
Add some text on the scope and why we are going to the option 1 and 2. Jeff and Oscar volonteer to propose some text.
The proposal will be added to the draft (Gab, Daniele) in -03 version or WG draft -00 (if any).

Aihua - Reza: add life cycle description and add scenarios on optical restoration, etc.

Address and describe also what a solution can be in green field vs. a brown field.

Check again on the draft terminology, please add issues in case of modification needed.
Need to open issues on GitHub in order to track the changes.
Open issues in GitHub on the two above statements
 
### issues on agenda
 
A.I. Gab. to Co-ordinate with Daniele on modification allowed on -02 to generate -03 draft version or wait the WG adoption process termination and add the modificatio on the new WG draft.

##### Discussion

Oscar suggested to add some Yang models examples to give an
idea on the draft evolution to ccamp. Unfortunately a real 
discussion on this matter is not yet started, We can just present the plans to evolve the draft.

Ways to operate:
1) Add the Yang models to the current draft
2) Generate a new draft to specify the Yang models related to this draft
3) Augment existing Yang drafts with the models needed by this draft

Option 2 is the selected one at least for the moment.

In any case there are two steps to follow:
1) make the models analysis
2) design the Yang models

Share the Yang model proposal reasonably before the next call for review (in GitHub)

##### post-meeting notes 


