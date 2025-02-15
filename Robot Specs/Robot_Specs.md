**Scenario **

A hypothetical stationary semi-autonomous asset that relies on
hypothetical centralized Command and Control (C2) has been emplaced in a
hypothetical forward operating environment. This asset is in a
tactically secured environment that is isolated from the centralized C2
location by an area that has become tactically contested. The asset can
normally operate by communicating with the C2 system using Radio
Frequency (RF) systems; however, the area of emplacement is frequency
contested and as a result reliable C2 communications cannot be
established. 

**Overview **

The TTU Capstone team will be tasked with designing a prototype Cable
Installation Robot for Contested Environments (CIRCE) system to restore
C2 communications to the emplaced asset without exposing the warfighter
to the unnecessary risk of trying to run communications cables in a
tactically contested environment. 

**System Overview **

-   The CIRCE system will operate in the RF and tactically contested
    environment reducing risk to the warfighter and error in cable
    installation. 

<!-- -->

-   The CIRCE system will be composed of three interacting components: 

<!-- -->

-   CirceBot - the cable installation robot (EE) 

<!-- -->

-   CirceSoft - the path planning software (CS) 

**\
**

**CirceBot Overview **

-   The CirceBot will operate semi-autonomously using the path
    transmitted by CirceSoft as a guide and reporting back to
    CirceSoft.  

<!-- -->

-   The CirceBot will manage its own position relative to the path
    provided and attempt to navigate to the provided points 

<!-- -->

-   The CirceBot must have a GPS but will operate in GPS denied
    environments and as a result must have ability to navigate to
    waypoints using other mechanisms 

<!-- -->

-   The CirceBot will install ethernet cable of 100 yards according to
    best practices associated with the cable type being installed. 

<!-- -->

-   The CirceBot will communicate with using hardlines to CirceSoft for
    purposes of telemetry transmittal and command and control. This is
    done to enable operation in frequency contested environments. 

<!-- -->

-   The CirceBot will have a GPS receiver and will transmit the location
    read from the receiver to CirceSoft. CirceSoft will then transmit
    the GPS location back to the CirceBot.  

** **

** **

**\
**

**CirceBot Requirements **

-   CirceBot shall receive and execute commands of the format supplied
    in the CirceSoft2CirceBot.proto spec from CirceSoft. 

<!-- -->

-   CirceBot shall transmit telemetry in the CirceBot2CirceSoft.proto
    spec to CirceSoft. 

<!-- -->

-   CirceBot shall accept planned paths from CirceSoft in the specified
    format and attempt to follow those paths to its objective. 

<!-- -->

-   CirceBot shall dispense the cable in accordance with cable
    installation guidelines including proper curve radiuses, proper
    strain relief, proper tension tolerances, etc. 

<!-- -->

-   CirceBot shall include an independent power source and be capable of
    a minimum 20 minutes of operation. 

<!-- -->

-   CirceBot shall operate in Tennessee environments 

<!-- -->

-   CirceBot shall transmit current position, current velocity, meters
    of cable left, heading, battery life left in a percentage, and an
    error code if one occurs 

<!-- -->

-   CirceBot shall receive Next Position waypoints and navigate to next
    waypoint. 

<!-- -->

-   CirceBot shall be able to carry 100 yards of Ethernet cable.
    (approximately 10 lbs.) 

<!-- -->

-   CirceBot shall have the capability to report error codes on a
    self-diagnosis basis 

<!-- -->

-   CirceBot shall have the ability to be recharged 

<!-- -->

-   CirceBot shall send commands at a minimum 10 Hz 

<!-- -->

-   CirceBot shall receive commands at a minimum 10 Hz 

<!-- -->

-   CirceBot shall communicate with CirceSoft using a the WebSocket
    protocol 

<!-- -->

-   CirceBot shall have a switch that will simulate GPS denied
    environments turning off the internal GPS system 

<!-- -->

-   CirceBot shall communicate with CirceSoft using the ethernet cable
    that is being deployed. 

<!-- -->

-   CirceBot shall stop once specified destination has been reached. 

<!-- -->

-   CirceBot shall have easily accessible ethernet cable to be reloaded 

<!-- -->

-   CirceBot shall have the capability of switching out ethernet cable
    reels in under 2 minutes 

<!-- -->

-   CirceBot shall have the capability of switching out ethernet cable
    without use of external tools 

**CirceSoft Overview **

-   The CirceSoft application will use imagery of the field of operation
    to determine the optimum path as determined by cable installation
    guidelines, cable length, obstacle avoidance, potential placement
    hazards and shortest path. 

<!-- -->

-   The CirceSoft application will be able to consume an image or RTSP
    stream from an elevated position to plan the path. 

<!-- -->

-   The CirceSoft application will analyze the course and determine best
    placement of the cable to reach the asset and alert the user if no
    path can be found. 

<!-- -->

-   The CirceSoft application will determine how much cabling is
    necessary to link the C2 and emplaced asset and alert the user if
    more cabling is required using the cable length reported by CirceBot
    as a metric. 

<!-- -->

-   GOAL: The CirceSoft application will determine and flag any
    potential hazards to the emplaced cable such as running the cable
    over a road, through water or any other area where additional
    efforts will be required to ensure cable integrity. 

<!-- -->

-   Note: The CirceSoft application only provides the path CirceBot is
    required to autonomously navigate. 

<!-- -->

-   The CirceSoft application will run as a containerized application
    that will be accessible from any web browser (responsive behavior is
    desired) and will have a simple interface that communicates status,
    telemetry, path and other relevant information to the end user. 

<!-- -->

-   The CirceSoft application container will use RedHat ubi9 with all
    relevant security updates 

<!-- -->

-   The CirceSoft application will communicate relevant path details to
    CirceBot 

<!-- -->

-   The CirceSoft application will receive telemetry and data output
    from CirceBot. 

<!-- -->

-   The CirceSoft application will project suggested paths on the image
    input overlay until the end user selects a path. 

<!-- -->

-   The CirceSoft application will create the overall path and provide a
    waypoint for the CirceBot to follow.  

** **

** **

** **

** **

**\
**

**CirceSoft Requirements **

-   CirceSoft shall send commands of the format supplied in the
    CirceSoft2CirceBot.proto spec to CirceBot. 

<!-- -->

-   CirceSoft shall receive feedback of the format supplied in the
    CirceStation2CirceBot.proto spec from CirceBot. 

<!-- -->

-   CirceSoft shall send paths of the specified format to CirceBot. 

<!-- -->

-   CirceSoft shall run in a containerized environment based on the
    RedHat Enterprise Linux Universal Base Image 9. 

<!-- -->

-   CirceSoft shall analyze image or RTSP input to determine a path for
    CirceBot to take. 

<!-- -->

-   CirceSoft will recognize and warn user of roadways using AI/ML
    methods. 

<!-- -->

-   CirceSoft shall show a continuous update of progression of
    CirceBot. 

<!-- -->

-   CirceSoft shall notify user when CirceBot has completed path. 

<!-- -->

-   CirceSoft shall allow users to set a planned path for CirceBot. 

<!-- -->

-   CirceSoft shall take in planned path and interpolate planned points
    to send to CirceBot 

<!-- -->

-   CirceSoft shall calculate a percentage complete from current
    position of CirceBot 

<!-- -->

-   CirceSoft shall send next planned path point from current CirceBot
    position. 

<!-- -->

-   CirceSoft shall show distance CirceBot can travel from user picture
    represented by a circle centered around CirceBot. 

<!-- -->

-   CirceSoft shall send commands at a minimum 10 Hz 

<!-- -->

-   CirceSoft shall receive commands at a minimum 10 Hz 

<!-- -->

-   CirceSoft shall communicate with CirceBot using standard WebSocket
    protocol 

 

 

**Departments and Concepts **

-   Electrical and Computer Engineering 

<!-- -->

-   Microcontroller design and implementation 

<!-- -->

-   Programming 

<!-- -->

-   Guidance, Navigation and Control 

<!-- -->

-   Power system 

<!-- -->

-   Communication  

<!-- -->

-   Signal Processing 

<!-- -->

-   Image Processing 

<!-- -->

-   Kalman/Particle Filtering** **

** **

-   **Computer Science  **

<!-- -->

-   AI/ML 

<!-- -->

-   Path Planning 

<!-- -->

-   Path Optimization 

<!-- -->

-   Resource Optimization 

<!-- -->

-   Computer Vision/Image Analysis 

<!-- -->

-   Human Computer Interaction 

<!-- -->

-   Data Visualization 

<!-- -->

-   Hardware Interaction 

<!-- -->

-   Telemetry processing 

<!-- -->

-   Responsive website design 

<!-- -->

-   Cybersecurity 
