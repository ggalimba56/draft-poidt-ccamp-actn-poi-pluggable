# ACTN POI plluggable week (December 12th, 2023)


****Attendees****
- [] Dieter Beller (Nokia)
- [x] Jean-Francois Bouquier (Vodafone)
- [] Daniele Ceccarelli (Cisco)
- [] Ori Gerstel (Cisco)
- [x] Aihua Guo
- [] Esther Lerouzic (Orange)
- [] Julien Meuric (Orange)
- [x] Italo Busi (Huawei)
- [] Gert Grammel (Juniper)
- [x] Gabriele Galimberti 
- [] Yuji Tochio
- [] Enrico Griseri (Nokia)
- [x] Sergio Belotti (Nokia)
- [] Victor Lopez
- [] Haomian Zheng
- [] Igor Bryskin
- [] Dirk Schroetter (Cisco)
- [] Brent Foster (Cisco)
- [] Jiang Sun (CMCC)
- [x] Prasenjit Manna (cisco)
- [x] Jose' Angel Perez (Vodafone)
- [] Peter Landon
- [X] Nigel Davis (Ciena)
- [x] Oscar Gonzalez de Dios (Telefonica)
- [] Fabio Peruzzini (TIM)
- [] Gene
- [] Manuel Lopez
- [x] Roberto Manzotti (Cisco)
- [x] Paolo Volpato
- [] Bradley Riapolov
- [] Phil Bedard (Cisco)
- [x] Reza Rokui (Ciena)
- [] Stefan Dahlford (Ericsson)
- [] Manolo Lopez Mordillo (Vodafone)

## Admin

draft-poidt-ccamp-actn-poi-pluggable-02.txt run for IPR poll to be WG doc. 

Jilien Meuric accepted to co-ordinate the several drafts on the table: draft-poidt-ccamp-actn-poi-pluggable, draft-davis-ccamp-photonic-plug-control-arch and poi-pluggable-nm-yang.

A request to move the call to a different time slot was raised:
possible slot fitting all the attendees is Thursday at 5:00pm CET. If all agree the next call will be on Jan. 11th, 2024.
For the moment the call is be-weekly.

Ask the ccamp chairs and secretary to move the meeting invitation.

### Review APs (action point) 

Daniele on the IPR status, still running.

Jeff added some text to MD file, Jeff to share it ?
No other comments suggestions.

### Closed Issues


### Next calls
Next call will be regular on Jan. 11th, 2024

#### General discussion

The main discussion was on the way we can proceed with the models definition and their application.

There are two model classes: device model and service models.
The device models are related to the interface between the PNCs and the devices (Routers, ROADMs. etc.), these nodels can feet any option (1, 2, and 3) and can leverage the models already difined in IETF (e.g. L0-type, i/f models, etc.) or forllow the openconfig models.
An analisys is needed to close the gap between what is it available and what is needed.
Service models are related to the interface between the PNCs and the MDSC. The goal is to have the models fitting all the options but a detailed analisys is needed to confirm that.

In any case models definition is driven by the use cases, the first to be addressed is the inter-layer link discovery or the verification, the inter-layer link is the connection between the Router pluggable and the ROAD port.

Italo proposed to evaluate the https://datatracker.ietf.org/doc/html/draft-ietf-ccamp-optical-impairment-topology-yang#name-optical-transponders-in-a-r

This can be reused also for the pluggable.
Also the draft-ietf-ccamp-optical-impairment-topology-yang, ietf-ccamp-layer0-types-ext-RFC9093-bis and draft-ietf-ccamp-dwdm-if-param-yang must be considered as a good starting point for the device models.

Action Point is to look at the draft and write a proposal for the next call. (Gab, Aihua and Italo will co-ordinate before Christmas vacations to set the proposal, volonteers are welcome.)

Another point of discussion was about a new GitHub repo to host the models and the proposals. Matter will be rediscussed during the next call. 
 
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
 


##### Discussion



##### post-meeting notes 


