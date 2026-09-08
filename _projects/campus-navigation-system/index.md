---
layout: post
title: Interactive Campus Navigation System
description:  A physical 3D printed campus map with ESP32-driven LED wayfinding, built for on-campus deployment at Lone Star College. Pressing a button for a destination illuminates the corresponding building on the map. Developed with a five-person team in collaboration with Computer Engineering faculty and the campus president.
skills:
  - ESP32 firmware
  - Fusion 360 CAD
  - 3D printing
  - LED control
  - Stakeholder collaboration
  - Iterative prototyping

main-image: /campus-map.jpg
---

## The Problem

New students and campus visitors regularly struggled to find buildings, and the existing static signage did not help much for anyone who did not already know the layout. The idea was to build something physical and immediate: press a button for where you are going, and the building lights up in front of you.

---

## Scope

The map covers 15 campus buildings and 4 designated parking areas. Each building is a separate printed piece with an LED positioned inside it, and each destination has a corresponding button on the map's control surface.

---

## My Role

I led the 3D CAD design and prototyping. That meant translating the campus footprint into a scaled physical model in Fusion 360, designing each building as a printable part with internal clearance for LED placement and wiring, and iterating on the layout so the whole map stayed at a workable size while keeping individual buildings identifiable.

I also worked on the ESP32 firmware that maps each button press to its corresponding building LED.

---

## Working with Stakeholders

This was the first project where the design had to satisfy people outside the engineering team. We worked with Computer Engineering faculty and presented to the campus president, which meant the design had to hold up to questions that were not technical: how it would be maintained, how visitors would understand it without instructions, whether it could scale if the campus added buildings.

That changed how I approached the work. Decisions I would have made on aesthetics alone had to be justified in terms of durability, clarity, and cost.

---

## What I Took From It

Designing for a physical space is unforgiving in ways that designing on a screen is not. Scale that looks fine in CAD can be unreadable in person, and a building that prints cleanly on its own can be impossible to wire once it is part of an assembly. Most of the iteration on this project came from printing something, looking at it in context, and adjusting.
