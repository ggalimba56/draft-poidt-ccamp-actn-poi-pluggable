---
coding: utf-8

title: >
  Applicability of Abstraction and Control of Traffic Engineered Networks 
  (ACTN) to Packet Optical Integration (POI) extensions to support Router
  Optical  interfaces

docname: draft-poidt-ccamp-actn-poi-pluggable-latest
submissiontype: IETF
workgroup: CCAMP Working Group
category: info
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Gabriele Galimberti
    role: editor
    org: Cisco
    street: Via S. Maria Molgora, 48 c
    city: 20871 - Vimercate
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

--- abstract

   This document extends the draft-ietf-teas-actn-poi-applicability to
   the use case where the DWDM optical coherent interface is equipped on
   the Packet device.  It identifies the YANG data models being defined
   by the IETF to support this deployment architecture and specific
   scenarios relevant for Service Providers.  Existing IETF protocols
   and data models are identified for each multi-layer (packet over
   optical) scenario with a specific focus on the MPI (Multi-Domain
   Service Coordinator to Provisioning Network Controllers Interface)in
   the ACTN architecture.

--- middle

{: #introduction}

# Introduction

   The full automation of the multilayer/multidomain network is a topic
   of high importance in the industry and the service providers
   community.  Typically, the layers composing such network are the IP/
   MPLS, (with Segment Routing) and the Optical ones.  The requirements
   of high bandwidth availability and dynamic control of the networks
   are of capital importance too.  The draft-ietf-teas-actn-poi-
   applicability specifies very well how to control and manage
   multilayer/multidomain networks using the Abstraction and Control of
   TE Networks (ACTN) architecture, see also {{my_figure-0}}.

   New DWDM Coherent pluggable optics, such as ZR {{OIF-400ZR-01-0}} and
   ZR+ {{Open_ZR-Plus_MSA}}, are enabling new multilayer network use cases
   where the DWDM interface is located within the packet domain
   equipment instead of being part of the Optical domain {{my_figure-1}}.

   ZR and ZR+ (and also CFP2-DCO) deployment in routers has already
   started and are expanding significantly.  The way the DWDM pluggable
   are in general managed is not yet completely specified and defined by
   any standard and it is becoming an urgent matter to cover for Service
   Providers.  Full end-to-end management solution of these DWDM
   coherent pluggable optics, leveraging on ACTN hierarchical
   architecture, is becoming critical to allow a wider deployment beyond
   simple point-to-point high capacity link scenarios between two IP/
   MPLS routers.

   {{my_figure-0}} The ACTN architecture, defined in {{!RFC8453}}, is used to
   control the multi-domain network shown in {{my_figure-1}} , where each
   Packet PNC (P-PNC) is responsible for controlling its IP domain,
   which can be either an Autonomous System (AS), {{!RFC1930}}, or an IGP
   area within the same operator network.  Each Optical PNC (O-PNC) in
   the below topology is responsible for controlling its own Optical
   Domain.

~~~~ ascii-art
                           +----------+
                           |   MDSC   |
                           +-----+----+
                                 |
   MPI interf. +-----------+-----+------+-----------+
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

   {{my_figure-1}} shows how the Packet Node DWDM coherent Ports are connected
   to the ROADM ports.

~~~~ ascii-art

   +------+       +------+  _________  +------+       +------+
   |P.N.1 |       | O.N. | /        /\ | O.N. |       |P.N.2 |
   |    P1| ----- |      ||        |  ||      | ----- |P1    |
 ==|    P2| ----- |      ||Optical |  ||      | ----- |P2    |==
 ==|    P3| ----- |      ||Network |  ||      | ----- |P3    |==
   |    P4| ----- |      ||        |  ||      | ----- |P4    |
   |      |       | ROADM| \________\/ | ROADM|       |      |
   +------+       +------+             +------+       +------+
    Packet                  Optical                    Packet
    Layer                   Layer                      Layer 



                         
P.N. = Packet Node (ROADM)
O.N. = Optical DWDM Node
ROADM = Lambda/Spectrum switch
Px = DWDM (coherent pluggable) Router ports 

~~~~
{: #my_figure-1 title="Cross layer interconnection"}

# Reference architecture and network scenario

   As described in {{my_figure-0}} and according to the Packet Optical
   Integration (POI) draft {{!I-D.draft-ietf-teas-actn-poi-applicability}} in
   which ACTN hierarchy is deployed {{!RFC8453}}, the PNCs are in charge to
   control a single domain (e.g.  Packet or Optical) while the MDSC is
   responsible to coordinate the operations across the different domains
   having the visibility of the whole network multi-domain and multi-
   layer network topolgy.

   A specific standard interface (MPI) allows the MDSC to interact with
   the different Provisioning Network Controller (O/P-PNCs).  Although
   the MPI interface should present an abstracted topology to the MDSC
   (hiding technology-specific aspects of the network and hiding
   topology details depending on the policy chosen) in the case of DWDM
   coherent pluggable located in the PN some information related to the
   physical component must be shared on MPI.  The above statement is
   assumed as the Domain PNC (e.g.  O-PNC) may not be able to get
   information from or set parameters to a node belonging to a different
   domain (e.g.  P-PNC).

   The reason of this change is due to the following statements:

   O-PNC routing and wavelength assignment

   > The MDSC can ask the O-PNC to set an optical circuit between two
   ROADM ports (A and Z).  The O-PNC having the full Optical Topology
   network knowledge can calculate the Optical Path, the wavelength
   assignment (RWA), etc.  K-circuits may be calculated and sorted
   based on some parameters (e.g. number of hops, path length, OSNR,
   etc.)

   Optical Circuit Feasibility

   > O-PNC can calculate the estimated OSNR for the A to Z circuits and
   sort them from the best to the worse performance or select the
   most suitable performance circuit.  To verify the circuit
   feasibility the O-PNC needs to know the Transceiver optical
   characteristics, e.g.  OSNR Robustness, DC capability, supported
   PDL, FEC, etc.  For more details refer to draft-ietf-ccamp-dwdm-
   if-param-yang.  The above parameters may not be directly retrieved
   from Packet Node by the O-PNC, (e.g. because the Packet Node
   supports only proprietary models or the Packet Nodes is not able
   to support dual writing operation or the Packet Node belongs to a
   different IP AS nor reachable by O-PNC), then they must be read by
   the P-PNC, shared to MDSC via MPI and finally to O-PNC.  Other
   parameters like central frequency and transmit power are
   calculated by the O-PNC and must be provisioned to the Pluggable
   optics when the circuit is set-up.

   Nominal Central frequency

   > After having verified the Circuit Optical feasibility the O-PNC
   shares the channel central frequency to MDSC so that the MDSC can
   ask P-PNC to provision the Lambda to Router Pluggable.

   FEC Coding

   > This parameter indicate what Forward Error Correction (FEC) code
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
      Transceiver, i.e. Receiver Sensitivity.  It is an input for O-PNC
      to properly calculate the optical power to set at ROADM port

   Receiver input power

   > This parameter is the measured input power at the receiver.  It is
      an input for O-PNC to properly check the patchcord (between
      transceiver and ROADM) loss comparing it with the ROADM port
      received power.

   operational-mode

   > In order to make the MPI communication more efficient and improve
      the abstraction, the above (and more) parameters can be summarised
      by the operational-mode parameter.  The operational-mode can be
      either standard ("application code" defined by ITU-T G.698.2) or
      organization/vendor specific.  In both cases are strings of
      characters defined by ITU-T or by vendors.  A pluggable may
      support several operational modes, those values are collected by
      the P-PNC and notified to O-PNC through the MDSC.  They are used,
      by O-PNC, to check the circuit optical feasibility.  For each
      transceiver the O-PNC will select only one operational-mode to be
      set, together with central frequency and TX power, in the
      pluggable though MDSC and P-PNC.

   The above optocal parameters are related to the Edge Node Transceiver
   and are used by the Optical Network control plane in order to
   calculate the optical feasibility and the spectrum allocation.  The
   parameters are read by the P-PNC from the DWDM pluggable and shared
   with MDSC to give the visibility of the pluggable characteristics.
   MDSC can use the info to understand the client capabolity and, again,
   share the same info to O-PNC for the impairment verification.  On the
   opposite direction O-PNC can send to MDSC the values (e.g.
   operational mode, lambda, TX power) to provision the Client (Packet)
   DWDM Pluggable.  The pluggable provisioning will be done by the
   P-PNC.  For more details on the optical interface parameters see:
   {{!I-D.ietf-ccamp-dwdm-if-param-yang}}.

   In summary the pluggable parameters exchanged from O-PNC to MDSC
     to P-PNC for end to end service provisioning are:

     - Pluggable Service source port-ID
     - Pluggable Service destination port-ID
     - Central Frequency (Lambda) (common to source and destination)
     - TX Output power (source port-ID)
     - TX Output power (destination port-ID)
     - Operational-mode (compatible)
     - Vendor OUI (if the operational mode in not standard)
     - Pluggable part number (if the operational mode in not standard)
     - Admin-state (common ?)

# Use Cases

   The different services supported by the network are shown in
   {{my_figure-3}}.  This draft is focused on the inter-layer link, the DCO
   links setting through the MC-links setting although the POI first
   goal is to set an IP service.

   {{my_figure-3}}

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
|  |       |  |              DCO-link             |  |       |  |
|  |  DCO <-------------------------------------------> DCO  |  |
+--+-------+--+                                   +--+-------+--+
       ^                                                 ^       
       |       +-------+                 +-------+       |       
       |       |  OLS  |                 |  OLS  |       |       
       |       |       |                 |       |       |       
       +-----> | <-----------------------------> |<------+       
  inter-layer  |       |    MC-link      |       | inter-layer   
     link      |       |                 |       |    link       
               +-------+                 +-------+               
                                                                 
                                                                 

                         
IP-link = IP service, out of this document scope
Eth-link = Ethernet connection 
DCO-link = Pluggable connection (OTSi connection)
MC-link = Media Channel link (MC optical circuit)

~~~~
{: #my_figure-3 title="Cross layer interconnection"}

   The use cases supported by the models are:

   Inter Layer (or domain) Link discovery and provisioning

   > The inter-layer links (or inter-domain links)are the
      interconnections (fiber) between the pluggable ports (in the
      Packet Layer) and the ROADM ports (in the Optical Layer).  They
      are set in the Packet and DWDM nodes either manually (e.g.  CLI)
      or via PNCs.  The values identifying the inter layer links may be
      defined by MDSC which has the visibility of both IP and Optical
      layers.  The "plug-id" could be used for this purpose.

   Network topology discovery and provisioning

   > MDSC retrieves the packet network topology from the P-PNC and the
      optical network topology from the O-PNC.  MDSC collects and
      rebuilds the service topology based on the services information
      coming from P-PNC and O-PNC as described in draft-ietf-teas-actn-
      poi- applicability.  {{!I-D.draft-ietf-teas-actn-poi-applicability}}

   End to End Packet service provisioning / deletion

   > MDSC is asked to set a Packet service between two Routers
      requiring additional connectivity bandwidth.

   Optical Circuit provisioning / deletion

   > MDSC is asked to set an Optical Circuit between two router ports
      (O-PNC will receive the same request from MDSC).  This is
      specially needed during the network installation to provide
      Connectivity between two Routers, the IP link will be set up later
      using this optical tunnel.

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

## Inter Layer Link discovery and provisioning

   The inter-layer links are set in the Packet and DWDM nodes either
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

   The inter-layer link must be set (or clear) any time a new pluggable
   module is installed (or removed) and it is connected to the ROADM
   port with the fiber patchcord.  When a new DCO is installed an
   inventory notification must be reported to the PNC and MDSC, the
   reported info are:

            - Pluggable port-ID (e.g. rack/shelf/slot/port or UUID)
            - Supported Operational-modes
            - Vendor OUI (if the operational mode in not standard)
            - Pluggable part number (if the op-mode in not standard)
            - Manufacturing data

   It would be also possible to auto-discover the inter-layer (inter-
   domain) links between DWDM coherent pluggables and ROADM ports by
   checking the input/output power levels (and probably switching on/off
   the lasers of the pluggables).  This would require the help of MDSC,
   O-PNC and P-PNC.  The same methoc could be used to verify the
   provisioned connectivty.  For further study in this draft.

## Network topology discovery and provisioning

   The first operation executed by the P-PNC and O-PNC is to discover
   the network topology and share it with the MDSC via the MPI.  The
   PNCs will discover and share also the inter-layer links (or
   connections) so that the MDSC can rebuild the full network topology
   associating the DWDM Router ports to the ROADM ports.  Once the
   association is discovered the P-PNC must share the characteristics of
   Pluggable module with the MDSC and then MDSC with the O-PNC.  At this
   point the Hierarchical controller (MDSC) and the domain controllers
   have all the information to commit and honour any service request
   coming from the OSS/orchestrator.  The details of the general
   operations are described in draft-ietf-teas-actn-poi-applicability,
   while this draft describes how to operate the Pluggable module during
   the optical circuit set-up operation.  As the Pluggable can be
   inserted or remove at any time it is relevant to have admin and
   operational state notification from the network to the PNC and MDSC.

{: #e2e-svc-provisioning}

## End to End service provisioning / deletion

   The End to End service provisioning is a multilayer provisioning
   involving both the packet layer and the optical layer.  The MDSC
   plays a key role as it has the full network visibility and can co-
   ordinate the different domain controllers' operations.  The service
   request can be driven by the operator using the MDSC UI or the MDSC
   receives the service request from the operator OSS/Orchestrator.

   The workflow for the creation of an end to end service is composed by
   the following steps:

~~~~ ascii-art
   1. MDSC receives an end to end service request from OSS/Orchestrator
   2. MDSC starts computing the different operations to implement the
      service.
   3. First MDSC starts to compute the routing, the bandwidth, the
      constrains of the packet service.
   4. If the Packer network can support the service without additional
      connections among the Routers
      4.1. then the packet service is commissioned through the P-PNC
      4.2. a notification with all the service info is sent to OSS.
   5. If more optical connectivity is needed
     5.1. the MDSC notifies the operator about the extra bandwidth need
     5.2. optionally, automatically identifies the spare router ports
          to be used for the connection extension (e.g. A and Z).
     5.3. The Router ports (pluggable) must be connected to A' and Z'
          ROADM ports and must be compatible (in terms of optical
          parameters, etc.).
   6. MDSC (autonomously or under operator demand) asks to O-PNC to set
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
   10. MDSC is then ready to commission the packet service through P-PNC
      10.1. has the visibility of end to end optical circuit (active)
      10.2. the packer service is commissioned
      10.3. MDSC service DB is updated
   11. The MDSC notifies the OSS of successful end to end service set-up

~~~~

   NOTE: the Optical service may not be feasible due to optical
   impairments calculation failure.  In this case the O-PNC will reject
   the optical circuit creation request to MDSC.  It is up to the
   operator (through MDSC) to scale down (e.g.  propose a 300Gb/s
   instead of a 400Gb/s service) the request or plan a network upgrade.

   Another point to note is the information sent by MDSC to O-PNC about
   the pluggable characteristics.  In reality this info should be known
   by the O-PNC at network commissioning time when the Inter Layer Link
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
   1. MDSC receives an end to end service request from the OSS/Orchestr.
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
      provisioning parameters to P-PNC to complete the optical set-up.
   8. MDSC verifies the end to end optical circuits (active)
   9. The MDSC notifies the OSS of successful optical circuit set-up.

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
     3.2. modifications to MDSC are sent to notify the new circuits data
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

   RSVP-TE message security is described in {{!RFC5920}}.  IPsec and HMAC-
   MD5 authentication are common examples of existing mechanisms.  This
   document only defines new UNI objects that are carried in existing
   UNI messages, thus it does not introduce new security considerations.

# IANA Considerations


~~~~ ascii-art
   // [TEMPLATE TODO] In order to comply with IESG policy as set forth
   // in http://www.ietf.org/ID-Checklist.html, every Internet-Draft
   // that is submitted to the IESG for publication MUST contain an IANA
   // Considerations section.  The requirements for this section vary
   // depending what actions are required of the IANA.  See "Guidelines
   // for Writing an IANA Considerations Section in RFCs" [RFC8126]. and
   // see [RFC4181] section 3.5 for more information on writing an IANA
   // clause for a MIB module internet draft.
~~~~

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

   prefix: ietf-ext-xponder-wdm-if reference: RFC XXXX

--- back

{: numbered="false"}

# Acknowledgments

   This document was prepared using kramdown.
