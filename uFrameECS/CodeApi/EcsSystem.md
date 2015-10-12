# EcsSystem
This is the base class for all systems.  It derives from SystemServiceMonoBehaviour which is part of the uFrame Kernel

## Properties
Name | Type | Description
-----|------|------------
ComponentSystem|Property|Access to the component system.  Use this to get/register components or groups.


## Setup Method
The setup method is used to register groups/components, and setup event listeners using EventAggregator
