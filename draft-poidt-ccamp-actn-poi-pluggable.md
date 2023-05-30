---
coding: utf-8

title: >
  Applicability of Abstraction and Control

docname: draft-poidt-ccamp-actn-poi-pluggable-00
submissiontype: IETF
workgroup: "Common Control and Measurement Plane"
category: info
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Gabriele Galimberti
    role: editor
    org:
    street:
    city:
    country: Italy
    phone: "+390392091462"
    email: ggalimbe56@gmail.com
  -
    ins: J. Bouquier
    name: Jean-Francois Bouquier
    role: editor
    org: Vodafone
    email: jeff.bouquier@vodafone.com
  -
    name: Ori Gerstel
    role: editor
    org: Cisco
    street: AMOT ATRIUM Tower 19th floor
    city: TEL AVIV-YAFO, TA
    country: Israel
    email: ogerstel@cisco.com
  -
    name: Brent Foster
    role: editor
    org: Cisco
    street: Research Triangle Park
    city: North Carolina
    country: UNITED STATES OF AMERICA
    email: brfoster@cisco.com
  -
    name: Daniele Ceccarelli
    role: editor
    org: Cisco
    email: daniele.ietf@gmail.com
  -
    name: Sergio Belotti
    role: editor
    org: Nokia
    email: sergio.belotti@nokia.com
    
  -
    name: Oscar Gonzalez de Dios
    role: editor
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
    
contributor:
  -
    name: Phil Bedard
    org: Cisco
    email: phbedard@cisco.com
  -
    name: Rana El Desouky Kazamel
    org: Cisco
    email: reldesou@cisco.com
  -
    name: Gert Grammel
    org: Juniper
    email: ggrammel@juniper.net
  -
    name: Prasenjit Manna
    org: Cisco
    email: prmanna@cisco.com
  -
    name: Jose-Angel Perez
    org: Vodafone
    email: jose-angel.perez@vodafone.com
  -
    name: Manuel-Julian Lopez
    org: Vodafone
    email: manuel-julian.lopez@vodafone.com

normative:
  RFC8795:
  RFC7698:
  RFC7699:
  RFC6205:
  RFC7792:
  RFC2119:
  ITU.G698.2:
    title: >
        Amplified multichannel dense wavelength division multiplexing
        applications with single channel optical interfaces
    author:
      org: International Telecommunications Union
    date: November 2009
    seriesinfo: ITU-T Recommendation G.698.2
  ITU.G694.1:
    title: >
        Spectral grids for WDM applications: DWDM frequency grid
    author:
      org: International Telecommunications Union
    date: February 2012
    seriesinfo: ITU-T Recommendation G.698.2
  ITU.G872:
    title: >
        Architecture of optical transport networks
    author:
      org: International Telecommunications Union
    date: January 2017
    seriesinfo: ITU-T Recommendation G.872
  OIF-400ZR-01-0:
    title: >
        Implementation Agreement 400ZR
    author:
      org: Optical Internetworking Forum (OIF)
    date: March 2020
    seriesinfo: OIF OIF-400ZR-01-0
  Open_ZR-Plus_MSA:
    title: >
        400ZR+ Multi-Source Agreement
    author:
      org: OpenZR+ Multi-Source Agreement
    date: September 2020
    seriesinfo: OpenZR+ Open ZR+ MSA

informative:
  RFC3410:
  RFC2629:
  MANTRA-whitepaper-IPoWDM:
   title: MANTRA whitepaper IPoWDM convergent SDN architecture
   author:
   org: Telecom Infra Project
   date: August 2022
   seriesinfo: 
   target:  https://cdn.brandfolder.io/D8DI15S7/at/n85t9h48bqtkhm9k7tqbs9fv/TIP_OOPT_MANTRA_IP_over_DWDM_Whitepaper_-_Final_Version3.pdf


--- abstract

   This document extends the I-D.draft-ietf-teas-actn-poi-applicability
   to the use case where the DWDM optical coherent interface is equipped on
   the Packet device.  The document analyzes several control architectures
   and identifies the YANG data models being defined
   by the IETF to support this deployment architectures and specific
   scenarios relevant for Service Providers.  Existing IETF protocols
   and data models are identified for each multi-layer (packet over
   optical) scenario with a specific focus on the MPI (Multi-Domain
   Service Coordinator to Provisioning Network Controllers Interface)in
   the ACTN architecture.   CHECK again

--- middle

{: #introduction}

# Introduction

   The full automation of the multilayer/multidomain network is a topic
   of high importance in the industry and the service providers
   community.  Typically, the layers composing such network are the IP/
   MPLS packet domain (with Segment Routing) and the Optical domain 
   providing DWDM transmission and photonic switching.  The requirements
   of high bandwidth availability and dynamic control of the networks
   are of capital importance too.  
   The {{!I-D.draft-ietf-teas-actn-poi-applicability}} 
   specifies very well how to control and manage
   multilayer/multidomain networks using the Abstraction and Control of
   TE Networks (ACTN) architecture, see also {{my_figure-0}}.

   New pluggable 

   Coherent pluggable digital coherent optics, such as ZR 
   {{OIF-400ZR-01-0}} and ZR+ {{Open_ZR-Plus_MSA}}, are enabling new 
   multilayer network use cases where the DWDM optical transmission
   function is located within the packet domain equipment instead of 
   being part of the Optical domain {{my_figure-1}}.
   This means that an IP/MPLS capable device becomes a multi-technology
   IP/MPLS/Optical device, as the optical connections (OTSi service
   and media channels) start and end at such devices.

   The coherent pluggable interface deployment in routers has already
   started and are expanding significantly.  The way the pluggables 
   are in general managed is not yet completely specified and defined by
   any standard and it is becoming an urgent matter to cover for Service
   Providers.  A full end-to-end management solution of these pluggable
   digital coherent optics, leveraging on ACTN hierarchical
   architecture, is becoming critical to allow a wider deployment beyond
   simple point-to-point high-capacity link scenarios between two IP/
   MPLS routers.

   {{my_figure-0}} ACTN architecture, defined in {{!RFC8453}}, is used
   to control the multi-layer and multi-domain network shown in 
   {{my_figure-1}}, where each Packet PNC (P-PNC) is responsible for 
   controlling its IP/MPLS domain, which can be either an Autonomous 
   System (AS), {{!RFC1930}}, or an IGP area within the same operator 
   network.  Each Optical PNC (O-PNC) in the below topology is 
   responsible for controlling its own Optical Domain.

~~~~ ascii-art
                            +----------+
                            |   MDSC   |
                            +-+-++-+-+-+
                              | |  | |
               +--------------+ |  | +--------------+
   MPI interf. |           +----+  +----+           |
               |           |            |           |
          +----+----+ +----+----+  +----+----+ +----+----+
          | P-PNC 1 | | O-PNC 1 |  | O-PNC 2 | | P-PNC 2 |
          +----+----+ +----+----+  +----+----+ +----+----+
               |           |            |           |
               |           \            /           |
     +-------------------+  \          /  +-------------------+
CE1 / P.N.1         P.N.2 \  |        /  / P.N.3         P.N.4 \ CE2
o--/---o               o---\-|-------|--/---o               o---\--o
   \   :               :   / |       |  \   :               :   /
    \  : PKT Domain 1  :  /  |       |   \  : PKT Domain 2  :  /
     +-:---------------:-+   |       |    +-:---------------:-+
       :               :     |       |      :               :
       :               :     |       |      :               :
     +-:---------------:------+     +-------:---------------:--+
    /  :               :       \   /        :               :   \
   /   o...............o  O.N.  \ /    O.N. o...............o    \
   \     Optical Domain 1       / \       Optical Domain 2       /
    \                          /   \                            /
     +------------------------+     +--------------------------+

~~~~
{: #my_figure-0 title="Reference multilayer/multidomain Scenario"}

   {{my_figure-1}} shows how the Packet Node DWDM coherent Ports are 
   connected to the ROADM ports.

~~~~ ascii-art

   +------+       +------+  _________  +------+       +------+
   |P.N.1 |       | O.N. | /        /\ | O.N. |       |P.N.2 |
   |    P1| ----- |      ||        |  ||      | ----- |P1    |
 ==|    P2| ----- |      ||Optical |  ||      | ----- |P2    |==
 ==|    P3| ----- |      ||Network |  ||      | ----- |P3    |==
   |    P4| ----- |      ||        |  ||      | ----- |P4    |
   |      |       |ROADM | \________\/ |ROADM |       |      |
   +------+       +------+             +------+       +------+
Packet+Optical              Optical               Packet+Optical
    Layer                   Layer                      Layer



P.N. = Packet/Optical Node (IPoDWDM router)
O.N. = Optical Switching DWDM Node (ROADM)
ROADM = Lambda/Spectrum switch
Px = DWDM (coherent pluggable) Router ports

~~~~
{: #my_figure-1 title="Cross layer interconnection"}

# Reference architecture and network scenario

   As described in {{my_figure-0}} and according to the Packet Optical
   Integration (POI) draft 
   {{!I-D.draft-ietf-teas-actn-poi-applicability}} in which ACTN 
   hierarchy is deployed {{!RFC8453}}, the PNCs are in charge to
   control a single domain (e.g.  Packet or Optical) while the MDSC is
   responsible to coordinate the operations across the different 
   domains having the visibility of the whole network multi-domain and
   multi-layer network topology.

   An architecture analysis has already been carried out by the MANTRA 
   sub-group in the OOPT – (Open Optical & Packet Transport)a Telecom
   Infra Project (TIP) group Project Group in the MANTRA whitepaper 
   {{MANTRA-whitepaper-IPoWDM}}.

   Two different architectural options have been identified, namely:
~~~~ ascii-art
     - Option 1: Dual SBI management of IPoDWDM routers
     - Option 2: Single SBI management of IPoDWDM routers
~~~~

   Both the options foresee that the packet and the optical domain are 
   kept separate from a management perspective.


## Option 1 - Dual SBI management of IPoWDM routers

The figure {{my_figure-2}} describes the architecture of option 1.


~~~~ ascii-art

                        +----------+
                        |   MDSC   |
                        +--+----+--+
                           |    |  
  MPI interface    +-------+    +-------+
                   |                    | 
              +----+----+          +----+----+
              |  P-PNC  |          |  O-PNC  |
              +--+---+--+          +----+----+
                 ^   ^               ^  ^  ^
                 |   |               |  |  |
         +-------+   +---------------|--|--|------+
         |                           |  |  |      |  
         |   +-----------------------+  |  +--+   |
         |   |               +----------+     |   |
         |   |               |                |   |
         |   |               v                |   |
         v   |        +--------------+        |   v
        +-----+      /                \      +-----+
       / P.N.1 \..../..O.N--------O.N..\..../ P.N.2 \
       \       /    \  Optical Domain  /    \       / 
        +-----+      \                /      +-----+
                      +--------------+

   P.N. = Packet/Optical Node (IPoWDM router) 
   O.N. = Optical Switching DWDM Node (ROADM)
   ROADM = Lambda/Spectrum switch
~~~~
{: #my_figure-2 title="Dual SBI management Scenario"}


   The peculiarity of this option consists of the fact that both the 
   packet SDN controller (P-PNC) and the optical SDN controller (O-PNC)
   have access to the coherent pluggable optics on the routers. 
   The P-PNC is the only entity allowed to configure them, while the
   O-PNC is granted with read-only permissions to avoid database 
   inconsistency between them. Data write access permissions are 
   expected to be implemented on the routers to only grant 
   configuration rights to the P-PNC.

   The read-only capabilities of the O-PNC consist in the following 
   tasks:
     - Device discovery, poll or stream configuration, state and 
       static capabilities.
     - Performance monitoring, periodically poll or stream performance 
       counters.
     - Fault notification, received asynchronous alarm notifications.

   The consequence of this split of roles is that the P-PNC exposes
   the coherent pluggables configuration APIs, while the O-PNC only
   the APIs needed for path computation (for OTSi services planning)
   and service provisioning for OLS media channel services.


 ## Option 2 - Single SBI management of IPoWDM routers

   The architecture related to option 2 is more meeting a canonical  ACTN approach, i.e.
   any PNCs only manage resources related to his own administrative domain,  like in {{!RFC8453}} and 
   {{!I-D.draft-ietf-teas-actn-poi-applicability}} is described 
   in figure {{my_figure-3}}.


~~~~ ascii-art

                        +----------+
                        |   MDSC   |
                        +--+----+--+
                           |    |  
  MPI interface    +-------+    +-------+
                   |                    | 
              +----+----+          +----+----+
              |  P-PNC  |          |  O-PNC  |
              +--+---+--+          +----+----+
                 ^   ^                  ^  
                 |   |                  |  
         +-------+   +------------------|---------+
         |                              |         |  
         |                              |         |
         |                   +----------+         |
         |                   |                    |
         |                   v                    |
         v            +--------------+            v
        +-----+      /                \      +-----+
       / P.N.1 \..../..O.N--------O.N..\..../ P.N.2 \
       \       /    \  Optical Domain  /    \       / 
        +-----+      \                /      +-----+
                      +--------------+

   P.N. = Packet/Optical Node (IPoWDM router) 
   O.N. = Optical Switching DWDM Node (ROADM)
   ROADM = Lambda/Spectrum switch
~~~~
{: #my_figure-3 title="Single SBI management Scenario"}


   In option 2 the P-PNC is the only component which has access to
   the routers and implements all the management capabilities. 
   In this case the P-PNC not only needs to expose to the MDSC all
   the needed info for the management of the multi-layer network,
   but also physical impairment data needed for the computation and
   validation of the optical channel. In addition, also performance
   data need to be exported, as well as the API needed for the
   configuration of the pluggables.


   In other words, the P-PNC is in charge of discovering the devices
   (both the routers and the pluggables), configuring them, monitoring
   the performances and managing the faults via the asynchronous
   notifications coming for the devices. The information collected
   needs to be exposed on the controller NBI and made available to
   the MDSCs, OSS systems or other applications.
   The pluggables characteristics can be exposed using
   {{!I-D.draft-ietf-ccamp-dwdm-if-param-yang}}
   and {{!I-D.draft-ietf-ccamp-optical-impairment-topology-yang}}.


   The role of the MDSC can be summarized into the following functions: 
   discovery of the pluggables capabilities,
   consolidation of topology and services at all layers from L0 to L3,
   provisioning of multi-layer services including the OTSi service 
   (whose path computation can be done by the MDSC itself or 
   requested to the O-PNC), monitoring the performances and managing
   the faults/alarms of the services at all layers.
   
   As the path computation for the optical layer is performed by the
   O-PNC, it needs to expose path computation APIs to the MDSC in order to accept the path characteristics
   or return the effective patameters of the computed path. 

  # Updates to the ACTN MPI interface


   A specific standard interface (i.e. ACTN MPI - MDSC-PNC interface)
   allows the MDSC to interact with the different O/P-PNCs.
   Although the MPI interface should present an abstracted topology to
   the MDSC (hiding technology-specific aspects of the network and
   hiding topology details depending on the policy chosen) in the case
   of DWDM coherent pluggable located in the router some information 
   related to the physical component must be shared on MPI. The above
   statement is assumed as the Domain PNC (e.g. O-PNC) may not be able
   to get information from a node belonging to a different
   domain (e.g.  P-PNC) or to set parameters.


  To better explain the reason of this change, there are two phases before setting an optical circuit:

   O-PNC routing and wavelength assignment and Optical Circuit Feasibility.

   > During the first phase the MDSC can ask the O-PNC to set an optical circuit between two
   ROADM ports (A and Z).  The O-PNC having the full Optical Topology
   network knowledge can calculate the Optical Path, the wavelength
   assignment (RWA), etc.  K-circuits may be calculated and sorted
   based on some parameters (e.g. number of hops, path length, OSNR,
   etc.)

   Optical Circuit Feasibility

   > During the optical circuit feasibility, the O-PNC can calculate the estimated OSNR for the A to Z circuits
   and sort them from the best to the worst performance or select the
   most suitable one from a optical performance standpoint.  
   To verify the circuit 
   feasibility the O-PNC needs to know the Transceiver optical
   characteristics, e.g.  OSNR Robustness, DC capability, supported
   PDL, FEC, etc.  For more details refer to 
   {{!I-D.draft-ietf-ccamp-dwdm-if-param-yang}} and {{!I-D.draft-ietf-optical-impairment-topology-yang}}. 
   The above parameters may not be directly retrieved
   from Packet Node by the O-PNC, (e.g. because the Packet Node
   supports only proprietary models or the Packet Nodes is not able
   to support dual writing operation or the Packet Node belongs to a
   different IP AS nor reachable by O-PNC), then they must be read by
   the P-PNC, shared to MDSC via MPI and finally to O-PNC.  Other
   parameters like central frequency and transmit power are
   calculated by the O-PNC and must be provisioned to the Pluggable
   optics when the circuit is set-up.
   
   ## Transceiver optical parameters capabilities
   
   We can summarize here the list of parameters needed for O-PNC to compute optical circuit
   feasibility and spectrum allocation. The parameters are read by the P-PNC from the DWDM
   pluggable and shared with MDSC to give the visibility of the pluggable characteristics.

   Nominal Central frequency

   > After having verified the Circuit Optical feasibility the O-PNC
   shares the channel central frequency to MDSC so that the MDSC can
   ask P-PNC to provision the Lambda to Router Pluggable.

   FEC Coding

   > This parameter indicates what Forward Error Correction (FEC) code
   is used at Ss and Rs (R/W) (not mentioned in G.698.2), it is used
   by the O-PNC to calculate the optical feasibility.  The FEC coding
   list (FEC can be many) supported by the pluggable is an input for
   O-PNC, one coding is selected for a specific circuit and is shared
   (as output) to MDSC for pluggable provisioning.

   Modulation format

   > This parameter indicates the list of supported Modulation Formats
   and the provisioned Modulation Format.  It is an input for O-PNC

   Transmitter Output power

   > This parameter provisions the Transceiver Output power.

   Receiver input power range

   > This parameter is the Min and Max input power supported by the
      Transceiver, i.e. Receiver Sensitivity.  It is an input for 
      O-PNC to properly calculate the optical power to set at ROADM 
      port

   Receiver input power

   > This parameter is the measured input power at the receiver.  
      It is an input for O-PNC to properly check the patchcord 
      (between transceiver and ROADM) loss comparing it with the ROADM
      port received power.

   Operational-mode

   > In order to make the MPI communication more efficient and improve
      the abstraction, the above (and more) parameters can be 
      summarized by the operational-mode parameter.  
      The operational-mode can be either standard ("application code"
      defined by ITU-T G.698.2) or organization/vendor specific.
      In both cases are strings of characters defined by ITU-T or
      by vendors.  A pluggable may support several operational modes,
      those values are collected by the P-PNC and notified to O-PNC
      through the MDSC.  They are used, by O-PNC, to check the circuit
      optical feasibility.  For each transceiver the O-PNC will select
      only one operational-mode to be set, together with central 
      frequency and TX power, in the pluggable though MDSC and P-PNC.

   The above optical parameters are related to the Edge Node Transceiver
   and are used by the Optical Network controller in order to
   calculate the optical feasibility and the spectrum allocation.  The
   parameters are read by the P-PNC from the DWDM pluggable and shared
   with MDSC to give the visibility of the pluggable characteristics.
   MDSC can use the info to understand the pluggable capability and, again,
   share the same info to O-PNC for the impairment verification.  
   
   ## Pluggables provisioning
   
   On the opposite direction O-PNC can send to MDSC the values (e.g.
   operational mode, lambda, TX power) to provision the Client (Packet)
   DWDM Pluggable.  The pluggable provisioning will be done by the
   P-PNC.  For more details on the optical interface parameters see:
   {{!I-D.draft-ietf-ccamp-dwdm-if-param-yang}} 
   {{!I-D.draft-ietf-ccamp-optical-impairment-topology-yang}}.

   In summary the pluggable parameters exchanged from O-PNC to MDSC
     to P-PNC for end to end service provisioning are:

     - Pluggable Service source port-ID
     - Pluggable Service destination port-ID
     - Central Frequency (Lambda) (common to source and destination)
     - TX Output power (source port-ID)
     - TX Output power (destination port-ID)
     - Operational-mode (compatible)
     - Vendor OUI 
     - Pluggable part number (if the operational mode in not standard)
     - Admin-state (common ?)

# Use Cases

   The different services supported by the network are shown in
   {{my_figure-4}}.  This draft is focused on the inter-domain link, 
   the coherent pluggable interfaces setting through the MC-links setting although the POI
   first goal is to set an IP service.

   {{my_figure-4}}

~~~~ ascii-art



+-------------+                                   +-------------+
|     IP      |                                   |     IP      |
|             |              IP-link              |             |
|   <------------------------------------------------------->   |
|             |                                   |             |
|             |                                   |             |
|  +-------+  |              Eth-link             |  +-------+  |
|  |  <--------------------------------------------------->  |  |
|  |       |  |                                   |  |       |  |
|  |       |  |              OTSi-link            |  |       |  |
|  |  DCO <-------------------------------------------> DCO  |  |
+--+-------+--+                                   +--+-------+--+
       ^                                                 ^
       |       +-------+                 +-------+       |
       |       |  OLS  |                 |  OLS  |       |
       |       |       |                 |       |       |
       +-----> | <-----------------------------> |<------+
  inter-domain |       |    MC-link      |       | inter-domain
     link      |       |                 |       |    link
               +-------+                 +-------+



IP-link = IP service, out of this document scope
Eth-link = Ethernet connection
DCO = Coherent pluggable interface (Digital Coherent Optics)
OTSi-link = Pluggable connection (OTSi connection)
MC-link = Media Channel link (MC optical circuit)

~~~~
{: #my_figure-4 title="Cross layer interconnection"}

   The use cases supported by the models are:

   Inter Layer (or domain) Link discovery and provisioning

   > The inter-domain links (or inter-domain links) are the
      interconnections (fiber) between the pluggable ports (in the
      Packet Layer) and the ROADM ports (in the Optical Layer).  They
      are set in the Packet and DWDM nodes either manually (e.g.  CLI)
      or via PNCs.  The values identifying the inter layer links may be
      defined by MDSC which has the visibility of both IP and Optical
      layers.  The "plug-id" {{!RFC8795}} could be used for this purpose.

   Network topology discovery and provisioning

   > MDSC retrieves the packet network topology from the P-PNC and the
      optical network topology from the O-PNC.  MDSC collects and
      rebuilds the service topology based on the services information
      coming from P-PNC and O-PNC as described in
      {{!I-D.draft-ietf-teas-actn-poi-applicability}}

   End to End Packet service provisioning / deletion

   > MDSC is asked to set a Packet service between two Routers
      requiring additional connectivity bandwidth.

   Optical Circuit provisioning / deletion

   > MDSC is asked to set an Optical Circuit between two router ports
      (O-PNC will receive the same request from MDSC).  This is
      specially needed during the network installation to provide
      Connectivity between two Routers, the IP link will be set up
      later using this optical tunnel.

   LAG extension

   > MDSC is asked to extend a service bandwidth.  This may require
      more Router optical connectivity.

   Optical Restoration

   > O-PNC detects an optical network failure and reroutes the optical
      circuits to a different path (and lambda).

   Network Maintenance Operations

   > MDSC is asked to isolate part of the optical network for
      maintenance and coordinate the O-PNC and P-PNC to preserve the
      traffic during the maintenance operation.

   In the following sub-sections a workflow for each use case is 
   described.  Each workflow is analyzed consider a scenario like the 
   one described in option 2. In those cases when the workflow differs
   between option 1 and option 2, a note describing the differences 
   is added.    

## Inter Domain Link discovery and provisioning

   The inter-domain links are set in the Packet and DWDM nodes either
   manually (e.g. via CLI or NMS) during the installation phase when the
   operator connects the Pluggable Transceiver to the ROADM port via
   fiber patch cord or is defined by the MDSC controller and provisioned
   via the PNCs.  One method and model to define the Inter Layer Link
   is, for example, to assign a value to the patchcord (for Tx and RX
   directions) and store those values in the Pluggable and ROADM port
   provisioning when the fiber is connected between the two ports.  This
   allows the PNCs to retrieve the values and share them with the MDSC
   for the correlation and check.  Other smarter and automatic methods
   of patchcord discovery may be defined but are outside of this draft
   scope.

   The inter-domain link must be set (or clear) any time a new pluggable
   module is installed (or removed) and it is connected to the ROADM
   port with the fiber patchcord.  When a new coherent pluggable interface is installed an
   inventory notification must be reported to the PNC and MDSC, the
   reported info are:

            - Pluggable port-ID (e.g. rack/shelf/slot/port or UUID)
            - Supported Operational-modes
            - Vendor OUI (if the operational mode in not standard)
            - Pluggable part number (if the op-mode in not standard)
            - Manufacturing data

   It would be also possible to auto-discover the inter-domain (inter-
   domain) links between DWDM coherent pluggables and ROADM ports by
   checking the input/output power levels (and probably switching on/off
   the lasers of the pluggables).  This would require the help of MDSC,
   O-PNC and P-PNC.  The same method could be used to verify the
   provisioned connectivity.  For further study in this draft.

   In this use case no difference between option 1 and 2 is foreseen,
   the MDSC can discover the inter-domain links correlating the
   information received by the P-PNC and O-PN

## Network topology discovery and provisioning

   The first operation executed by the P-PNC and O-PNC is to discover
   the network topology and share it with the MDSC via the MPI.  The
   PNCs will discover and share also the inter-layer links (or
   connections) so that the MDSC can rebuild the full network topology
   associating the DWDM Router ports to the ROADM ports.  Once the
   association is discovered the P-PNC must share the characteristics of
   Pluggable module with the MDSC and then MDSC with the O-PNC.  At this
   point the Hierarchical controller (MDSC) and the domain controllers
   have all the information to commit and honor any service request
   coming from the OSS/orchestrator.  The details of the general
   operations are described in 
   {{!I-D.draft-ietf-teas-actn-poi-applicability}},
   while this draft describes how to operate the Pluggable module during
   the optical circuit set-up operation.  As the Pluggable can be
   inserted or remove at any time it is relevant to have admin and
   operational state notification from the network to the PNC and MDSC.

   Also in this case the MDSC can retrieve all the needed info from
   the P-PNC in both options and optionally the pluggables and
   inter-domain links also from the O-PNC in case of option 1.

{: #e2e-svc-provisioning}

## End to End service provisioning / deletion

   The End to End service provisioning is a multilayer provisioning
   involving both the packet layer and the optical layer.  The MDSC
   plays a key role as it has the full network visibility and can co-
   ordinate the different domain controllers operations.  The service
   request can be driven by the operator using the MDSC UI or the MDSC
   receives the service request from the operator OSS/Orchestrator.

   The workflow for the creation of an end to end service is composed by
   the following steps:

~~~~ ascii-art
Option 1

  1. MDSC receives a end to end service request from OSS/Orchestrator
  2. MDSC starts computing the different operations to implement the
     service.
  3. First MDSC starts to compute the routing, the bandwidth, the
     constrains of the packet service.
  4. If the Packet network can support the service without additional
     connections among the Routers
     4.1. then the packet service is commissioned through the P-PNC
     4.2. a notification with all the service info is sent to OSS.
  5. If more optical connectivity is needed
     5.1. MDSC notifies the operator about the extra bandwidth need
     5.2. optionally, automatically identifies the spare router ports
          to be used for the connection extension (e.g. A and Z).
     5.3. The Router ports (pluggable) must be connected to A' and Z'
          ROADM ports and must be compatible (in terms of optical
          parameters, etc.).
  6. MDSC (autonomously / under operator demand) asks to O-PNC to set
     an optical circuit between ROADM ports A' and Z' assuming that
     the information on the pluggable supported parameters (A and Z):
            - Pluggable Service source port-ID
            - Pluggable Service destination port-ID
            - Operational-mode or standard mode (compatible)
            - Vendor OUI (if the operational mode in not standard)
            - Pluggable part number (if the op-mode in not standard)
            - Admin-state
          are already known by the O-PNC  
     6.2. the bandwidth (e.g. 100G or 400G, etc.)
     6.3. the routing constraints (e.g. SRLG XRO, etc)
  7. O-PNC calculates the optical route, selects the Lambda, verifies
     the optical feasibility, calculates the pluggable TX power.
     7.1.  If all is OK, provisions the optical circuit in ROADM.
     7.2.  If anything went wrong the O-PNC rejects the MDSC request.
  8. O-PNC updates the MDSC of successful circuit provisioning
     including the path, the Lambda, the operational mode (or the
     explicit optical parameters), the TX power, SRLG, etc.
     The optical circuit at this point is provisioned but not yet
     operational (no optical power coming from the transceivers yet)
  9. The MDSC updates the service DB and forward the pluggable
     provisioning parameters to P-PNC to complete the optical set-up.
  10. MDSC is ready to commission the packet service through P-PNC
      10.1. has the visibility of end to end optical circuit (active)
      10.2. the packet service is commissioned
      10.3. MDSC service DB is updated
  11. MDSC notifies the OSS of successful end to end service set-up
  12. The service assurance can then start, through the O-PNC for the
      optical circuit (including Pluggable Alarms and PM), 
      through the P-PNC for the IP/MPLS service. 

~~~~

   NOTE: the Optical service may not be feasible due to optical
   impairments calculation failure.  In this case the O-PNC will reject
   the optical circuit creation request to MDSC.  It is up to the
   operator (through MDSC) to scale down (e.g.  propose a 300Gb/s
   instead of a 400Gb/s service) the request or plan a network upgrade.

   Another point to note is the information about the pluggable are
   directly collected by O-PNC. In reality this info should be known
   by the O-PNC at network commissioning time when the Inter Domain
   Link is set or discovered.  The pluggable information may have 
   multiple instances when the pluggable support multiple bit rate 
   (e.g.  ZR+).
   In case of multiple bit rate (and multiple operational mode) the
   O-PNC can decide to propose to MDSC a different bit rate (higher or
   lower) calculated in base of the optical validation algorithms.  That
   is: MDSC ask for a 400Gb/s bit rate while O-PNC proposer a 300Gb/s
   bit rate, instead of rejecting the circuit request.


~~~~ ascii-art
 Option 2

  1. MDSC receives a end to end service request from OSS/Orchestrator
  2. MDSC starts computing the different operations to implement the
     service.
  3. First MDSC starts to compute the routing, the bandwidth, the
     constrains of the packet service.
  4. If the Packet network can support the service without additional
     connections among the Routers
     4.1. then the packet service is commissioned through the P-PNC
     4.2. a notification with all the service info is sent to OSS.
  5. If more optical connectivity is needed
     5.1. MDSC notifies the operator about the extra bandwidth need
     5.2. optionally, automatically identifies the spare router ports
          to be used for the connection extension (e.g. A and Z).
     5.3. The Router ports (pluggable) must be connected to A' and Z'
          ROADM ports and must be compatible (in terms of optical
          parameters, etc.).
  6. MDSC (autonomously / under operator demand) asks to O-PNC to set
     an optical circuit between ROADM ports A' and Z' providing
     information on:
     6.1. the pluggable supported parameters (A and Z)
            - Pluggable Service source port-ID
            - Pluggable Service destination port-ID
            - Operational-mode or standard mode (compatible)
            - Vendor OUI (if the operational mode in not standard)
            - Pluggable part number (if the op-mode in not standard)
            - Admin-state (common ?)
     6.2. the bandwidth (e.g. 100G or 400G, etc.)
     6.3. the routing constraints (e.g. SRLG XRO, etc)
  7. O-PNC calculates the optical route, selects the Lambda, verifies
     the optical feasibility, calculates the pluggable TX power.
     7.1.  If all is OK, provisions the optical circuit in ROADM.
     7.2.  If anything went wrong the O-PNC rejects the MDSC request.
  8. O-PNC updates the MDSC of successful circuit provisioning
     including the path, the Lambda, the operational mode (or the
     explicit optical parameters), the TX power, SRLG, etc.
     The optical circuit at this point is provisioned but not yet
     operational (no optical power coming from the transceivers yet)
  9. The MDSC updates the service DB and forward the pluggable
     provisioning parameters to P-PNC to complete the optical set-up.
  10. MDSC is ready to commission the packet service through P-PNC
      10.1. has the visibility of end to end optical circuit (active)
      10.2. the packet service is commissioned
      10.3. MDSC service DB is updated
  11. MDSC notifies the OSS of successful end to end service set-up
  12. The service assurance can then start, through the O-PNC for the
       optical circuit, through the P-PNC for the pluggable and the 
       IP/MPLS service.

~~~~

   NOTE: the Optical service may not be feasible due to optical
   impairments calculation failure.  In this case the O-PNC will reject
   the optical circuit creation request to MDSC.  It is up to the
   operator (through MDSC) to scale down (e.g.  propose a 300Gb/s
   instead of a 400Gb/s service) the request or plan a network upgrade.
   Another point to note is the information sent by MDSC to O-PNC about
   the pluggable characteristics.  In reality this info should be known
   by the O-PNC at network commissioning time when the Inter Domain Link
   is set or discovered.  The pluggable information may have multiple
   instances when the pluggable support multiple bit rate (e.g.  ZR+).
   In case of multiple bit rate (and multiple operational mode) the
   O-PNC can decide to propose to MDSC a different bit rate (higher or
   lower) calculated in base of the optical validation algorithms.  That
   is: MDSC ask for a 400Gb/s bit rate while O-PNC proposer a 300Gb/s
   bit rate, instead of rejecting the circuit request.



## Optical Circuit provisioning / deletion

   Upon receiving an optical service request from the OSS/Orchestrator,
   the MDSC starts performing the different operations to implement the
   optical service (e.g. from A to Z).  As an alternative the service
   request can be driven by the operator using the MDSC UI.

   The steps of the workflow are:

~~~~ ascii-art
  1. MDSC receives a end to end service request from the OSS/Orchestr
  2. MDSC starts computing the different operations to implement the
     service.
  3. to check whether the optical connectivity is feasible
     3.1. automatically identifies the router ports to be
          used for the optical connection (e.g. A and Z).
     3.2. The Router ports (pluggable) must be connected to A' and Z'
          ROADM ports and must be compatible (in terms of optical
          parameters, etc.).
  4. MDSC asks to O-PNC to set the optical circuit between ROADM
     ports A' and Z' providing information on:
     4.1. the pluggable supported parameters (A and Z)
            - Pluggable Service source port-ID
            - Pluggable Service destination port-ID
            - Operational-mode or standard mode (compatible)
            - Vendor OUI (if the operational mode in not standard)
            - Pluggable part number (if the op-mode in not standard)
            - Admin-state (common ?)
     4.2. the bandwidth (e.g. 100G or 400G, etc.)
     4.3. the routing constraints (e.g. SRLG XRO, etc)
  5. O-PNC calculates the optical route, selects the Lambda, verifies
     the optical feasibility, the pluggable TX power.
     5.1.  If all is OK, provisions the optical circuit
  6. O-PNC updates the MDSC of successful circuit provisioning
     including the path, the Lambda, the operational mode (or the
     explicit optical parameters), the TX power, etc.
  7. The MDSC updates the service DB and forward the pluggable
      provisioning parameters to P-PNC to complete the optical set-up
  8. MDSC verifies the end to end optical circuits (active)
  9. The MDSC notifies the OSS of successful optical circuit set-up.
  10.The assurance operational can start, fully driven by O-PNC in 
     option 1 or coordinated by MDSC in option 2. 
~~~~

   NOTE: the Optical service may not be feasible due to optical
   impairments calculation failure.  In this case the O-PNC will reject
   the optical circuit creation request to MDSC.  It is up to the
   operator (through MDSC) to scale down the request or plan a network
   upgrade.

   The same consideration done in the previus use case are applicable
   here.

## LAG extension

   Upon receiving a LAG service request from OSS/Orchestrator, the MDSC
   start computing the different operations to implement the request.

   The MDSC would determine if an existing multi-layer connection exists
   between the routers participating in the LAG.  If so, the MDSC would
   request the P-PNC to configure and add the new LAG bundle member link

   using this existing connection, and notify the OSS confirmation of
   the additional link.  If more optical connectivity is needed, then
   the procedures defined in {{e2e-svc-provisioning}} would be followed.

## Optical Restoration

   For this use case the trigger for the Domain controller and MDSC to
   take actions is coming from the optical data plane when the O-PNC
   detects or is notified about an optical network failure (e.g. a fiber
   cut or a node failure).  This kind of events affect the traffic and a
   number of optical circuits are lost.

~~~~ ascii-art
  1. First action is taken by the O-PNC to identify what are the
     affected circuits enabled to restoration
  2. For the circuits enabled to restoration O-PNC starts to compute
     2.1. the restore paths
     2.2. their feasibility and any optical parameter change (e.g.
          lambda retuning, TX power, etc.)
  3. If the restore path and all parameters are OK for the optical
     feasibility
     3.1. the restore path is provisioned
     3.2. nodifications to MDSC are sent to notify the circuits data
            - circuit path + SRLG
            - Pluggable Service source port-ID
            - Pluggable Service destination port-ID
            - Operational-mode or standard mode (compatible)
            - Admin-state (common ?)
  4. The MDSC updates the circuit DB and forward any pluggable
     provisioning change to P-PNC
  5. P-PNC will take care to apply the new provisioning data to the
     pluggables (e.g. lambda, operational data, TX power, etc.)
  6. The Restoration process is then completed and the IP connection
     between the routers is recovered.

~~~~

   NOTE: the restoration may not be feasible due to optical impairments
   calculation failure.  In this case the O-PNC will notify the optical
   circuit restoration failure to MDSC.  It is up to the operator,
   through MDSC, to take actions and/or plan a network upgrade.

   In case the optical circuit restoration is revertible, is again O-PNC
   responsibility to monitor the failure after the fix and start the
   revert procedure to bring the restore path to the original route.

## Network Maintenance Operations

   The maintenance operation is requested by the OSS when a part of the
   network needs a maintenance activity.  There could be Packet network
   maintenance or Optical network Maintenance.  As an alternative the
   maintenance request can be driven by the operator using the MDSC UI.

   The Packet network maintenance is simple and is addressed by the MDSC
   in cooperation with the P-PNC.

   The optical network maintenance is more complex and needs the MDSC
   coordination to ask the P-PNC to move away the traffic from the
   resources under maintenance in the optical network.  That means MDSC
   has to search in the service DB whether a service is using a definite
   optical link and re-route the service to a part of the optical
   network not affected by the maintenance operation.  Upon maintenance
   completion the MDSC will bring all the traffic back to the original
   route.

# Optical Interface for external transponder in a WDM network

   This document proposes an augmentation to the ietf-interface module
   called ietf-ext-xponder-wdm-if.  The ietf-ext-xponder-wdm-if, author
   note: define the model, is an augment to the ietf-interface.  It
   allows the user to set the operating mode of transceivers as well as
   other operational parameters.  The module also provides threshold
   settings and notifications to supervise measured parameters and
   notify the client.

# Structure of the Yang Module

   ietf-ext-xponder-wdm-if is a top-level model for the support of this
   feature.

# Security Considerations

   This document does not introduce any new interfaces or protocols
   in addition to the ACTN architecture defined in {{!RFC8453}}, 
   hence the same considerations apply. In addition,  IPsec and HMAC-
   MD5 authentication are common examples of existing mechanisms.

# IANA Considerations



   This document registers a URI in the IETF XML registry {{!RFC3688}}.
   Following the format in {{!RFC3688}}, the following registration is
   requested to be made:

   URI: urn:ietf:params:xml:ns:yang:ietf-interfaces:ietf-ext-xponder-
   wdm-if

   Registrant Contact: The IESG.

   XML: N/A, the requested URI is an XML namespace.

   This document registers a YANG module in the YANG Module Names
   registry {{!RFC6020}}.

   This document registers a YANG module in the YANG Module Names
   registry {{!RFC6020}}.

   prefix: {{!I-D.draft-ietf-ccamp-dwdm-if-param-yang}}

--- back

{: numbered="false"}

# Acknowledgments

   Many Thanks to Italo Busi for the several comments, suggestion and 
   support in GitHub process management.
   This document was prepared using kramdown.
