[vmodel]: ./images/v-model.png "v-model"
[safetyculture]: ./images/safetyculture.tiff "safetyculture"
[hara]: ./images/hara.png "hara"
[adas-archi]: ./images/adas-archi.png "adas-archi"
[system-hierarchy]: ./images/system-hierarchy.png "system-hierarchy"
[adas-sub-system]: ./images/adas-sub-system.png "adas-sub-system"
[asil]: ./images/asil.jpg "asil"
[asil-decomp]: ./images/asil-decomp.png "asil-decomp"
[ftti]: ./images/ftti.tiff "ftti"
[sw-req]: ./images/sw-req.tiff "sw-req"


# Functional Safety of a Lane Assistance System

Find the here the original [README](./udacity_README.md) provided by Udacity.

---

## Project Instructions

Document a safety case for a lane assistance item. This would include the creation of functional safety documents based on what I've learned in the lessons.

The initial documents provided  are simplified versions of what a functional safety manager would create as part of a safety case. A safety case is a collection of documents proving that a project has made a vehicle safer.

More specific instructions are in [this pdf document here.](./Project_Instructions.pdf)

## Lane Assistance System

### System Architecture

For this project, a simplified version of a Lane Assistance System was under consideration.

It has 2 functions:

* **Lane Departure Warning**
* **Lane Keeping Assistance**

These system are supposed to function in the following situation:

* When the driver drifts towards the edge of the lane

In that situation, two things will happen:

* the **Lane Departure Warning** function will vibrate the steering wheel
* the **Lane Keeping Assistance** function will move the steering wheel so that the wheels turn towards the center of the lane

Diagram of the system architecture:

![alt text][adas-archi]

### Sub-System Architecture

If we dig down further into the power steering ECU sub-system, we divide the ECU into its hardware and software components. As we traverse down the V model, the architecture diagrams become more focused and detailed.

![alt text][adas-sub-system]

## Key Takeaways

### Functional Safety

**Functional Safety** is the process of identifying risky situations and lowers them to acceptable levels.

Safety is the absence of unreasonable risk.

### ISO 26262

#### General Introduction

ISO 26262 only covers electronic and electrical malfunctions in passenger vehicle systems. The standard provides a framework for reducing risks that could harm people's health. It is interesting to note that ISO 26262 does **not** consider nominal performance as part of functional safety. 

#### V Model

ISO 26262 uses a process model called the V model. Here is a simplified representation:

![alt text][vmodel]

Sections of the complete V Model:
* Management of Functional Safety
* Concept Phase
    * Item Definition
    * Initiation of the safety life cycle
    * Hazard Analysis and Risk Assessment
    * Function Safety Concept
* Product Development at the System Level 
* Product Development at the Hardware Level
* Product Development at the Software Level
* Product and Operations
    * Production
    * Operation, Service and Decommissioning
* Supporting Processes
* ASIL-oriented and Safety-oriented Analysis
* Guidelines on ISO 26262

This project is focused on the left side of the V model.

#### MISRA

The Motor Industry Software Reliability Association defines guidelines around the C and C++ programming languages to extract a subnet of both that are appropriate for safety critical applications:
* Defensive implementation techniques
* Language subnets
* Style guides
* Naming Conventions

#### Hardware Failure Metrics 🛠

Hardware failures are inevitable.

Designing a safe system is the ensure that as most hardware failures would be detected.

The ASIL define the acceptance rate of random hardware failures. An ASIL D element, for example, should have fewer than one failure every 100 million hours. 

## Description

### 1. Safety Plan

**[01_SafetyPlan_LaneAssistance.pdf](./01_SafetyPlan_LaneAssistance.pdf)**

As part of the safety plan, I included the following major elements:
* what system is under consideration
* the goal of the project
* what steps will be taken to ensure safety
* the roles and personnel involved in the project
* the project timeline

The safety case would discuss what elements you added to the system in order to make it safe. And it would provide testing evidence that shows the system functioning properly. The documentation provides evidence that what you added to the system really does make the vehicle safer. Somebody who is completely independent from the functional safety team should evaluate the validity of the safety case.

In the current ongoing pursue of autonomous driving, a few fatalities could not be avoided. At these occasions it is striking how we, as human, do not forgive technology when it fails us. 

Technological failures are also human failures since we are designing and implementing these systems ourselves. That is why is it of the utmost importance the promote a **Safety Culture** at the workplace.

![alt text][safetyculture]

#### Development Interface Agreement (DIA)

A DIA defines the roles and responsibilities between companies involved in developing a product. All involved parties need to agree on the contents of the DIA before the project begins.

The DIA also specifies what evidence and work products each party will provide to prove that work was done according to the agreement.

The ultimate goal is to ensure that all parties are developing safe vehicles in compliance with ISO 26262.

### 2. Hazard Analysis and Risk Assessment

**[02_HazardAnalysisAndRiskAssessment.pdf](./02_HazardAnalysisAndRiskAssessment.pdf)**

The ultimate goal of Hazard Analysis and Risk Assessment is to define requirements specifying what ADAS needs to do in order to avoid hazardous situations.

![alt text][hara]

### Situational Analysis

Situational Analysis is the task of imagining hypothetical driving situations and analysis what would happen if the vehicle malfunction in each situation.

During Situational Analysis, we want to focus on the most critical scenarios.

I used the following predefined situation phrasing with predefined guidewords:

**[Operation Mode]** on **[Operational Scenario]** during **[Environment Details]** with **[Situational Details]** and **[Item Usage]** system.

The lessons took the following situation as an example:

**Normal driving** on **country roads** during **normal conditions** with **high speed** and **system incorrectly used**.

(The driver is misusing the lane keeping assistance function as an autonomous function.)

### Hazard Identification


### Hazardous Event Classification

ISO 26262 defines a risk as follow:

> **Risk** = **Severity** x **Exposure** x **Controllability**

* Severity measures how badly a person could get injured in an accident.
* Exposure is how often or how long would driver find themselves in a specific situation.
* Controllability is how likely the drive could gain control of the vehicle during a hazardous event.

Driving on a wet road for example is classified to be **E3 - Quite Often** according to the functional safety standard.

### ASIL Levels

ASIL stands for Automotive Safety Integrity Levels.

The 5 risk levels (from least to most risky) are:

* QM - Quality Management
* ASIL A
* ASIL B
* ASIL C
* ASIL D

Whenever a hazardous situation is classified as QM, there is no need to apply ISO 26262. The acceptance level is low enough.

![alt text][asil]

Different measures for different ASILs:

* For Software units with ASIL D more rigorous testing such as [MC/DC coverage](https://en.wikipedia.org/wiki/Modified_condition/decision_coverage) are highly recommended whereas ASIL B only mandates DC coverage.
* ASIL D suggests a target for the PMHF metric failure rate of no greater than 10 dangerous, undetected failures per billion hours of operation whereas ASIL B only suggests a failure rate of no more than 100 failures per billion hours

### 3. Functional Safety Concept

**[03_FunctionalSafetyConcept_LaneAssistance.pdf](./03_FunctionalSafetyConcept_LaneAssistance.pdf)**

The functional safety concept provides a high level overview of the system. Based on the hazard analysis and risk assessment, it details what the system is required to do in order to reduce risks involved by the Lane Assistance functionality to acceptable levels. It considers the system as a black box and only defines how it should behave from an exterior point of view without any knowledge of the specific implementation.

Here are the few steps I followed to achieve the technical safety concept:

* Derived functional safety requirements from the safety goals
* Refined the item architecture
* Allocated functional safety requirements to the item architecture
* Determined ASILs for the 3 subsystems
* Decomposition of the ASILs

![alt text][asil-decomp]

The 3 main attributes to each functional safety requirements were discussed ans specified:

1. **Fault Tolerant Time Interval (FTTI)**
    The FTTI for both systems is set to be 50 ms. 

![alt text][ftti]

2. **Warning and Degradation Concept**
    This document defines how the driver will be alerted when the system has reduces functionality or is turned off completely.
    It also includes how the vehicle will transition to and recover from a safe state.

3. **Verification and Validation Acceptance Criteria**
    * *Validation* is the process of ensuring that the chosen values and behaviors of the system are reasonable. In this case, we would need to validate that the torque amplitudes and frequencies of the LDW are appropriate for the driving task and suit every drivers. 
    * *Verification* is the process of ensuring that the safety requirements are met. In this case we would need to verify that the system really does turn off whenever the LKA has been enabled for MAX_DURATION.


### 4. Technical Safety Concept

**[04_TechnicalSafetyConcept_LaneAssistance.pdf](./04_TechnicalSafetyConcept_LaneAssistance.pdf)**

The technical safety concept is a level deeper into the details of the system. It has knowledge of how the system is implemented.

**Technical safety requirements describe what a system will do when a malfunction violates a safety goal.**

### 5. Software Requirements and Architecture

**[05_SoftwareRequirementsAndArchitecture_LaneAssistance.pdf](./05_SoftwareRequirementsAndArchitecture_LaneAssistance.pdf)**

Most of the software safety requirements are derived from the technical safety requirements.

![alt text][sw-req]

The customer, internal, safety and other kinds of requirements are all part of the **same** and unique overall product architectural design. 



## Annexes

### Vocabulary

A **fault** is when something inappropriate happens to the system, such as a defect or unexpected behavior.

A fault can lead to a **failure**, which is synonymous with a malfunction. Failure means that the system has stopped working properly. The system is no longer doing what it is supposed to do.

A failure could lead to a **hazard**. A hazard is a situation that could cause injury to a person or harm a person's health. If a system fails, the situation is potentially hazardous. Not all failures are necessarily hazardous, which means hazards have different levels of risk.

**Risk** measures the level of danger in a situation. An example of a high risk situation is one that it is likely to happen and also cause serious injury.

*Faults leads to failures. A failure leads to a hazard. A hazard has a certain a level of risk.*

An **Item** is just a high level system in the vehicle; in this case, the item is the lane assistance system.

A **System** is a set of elements that have at least:
* a sensor
* a controller
* an actuator

Here is a summary how ISO 26262 labels system hierarchies with typical examples for each part:

![alt text][system-hierarchy]