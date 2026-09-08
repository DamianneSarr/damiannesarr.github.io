---
layout: post
title: Rooftop Photovoltaic System Concept (ASME EMPOWER)
description:  A solar energy integration concept for a campus building, developed as the electrical engineer on a four-person cross-disciplinary team during the ASME EMPOWER Summer Engineering Internship. The design targeted a 15 to 25 percent offset of the building's daytime electrical load and was presented to practicing engineers and managers at program close.
skills:
  - Photovoltaic system design
  - NEC 705 interconnection
  - ASCE 7 structural loading
  - Inverter architecture selection
  - Feasibility analysis
  - Bill of materials
  - Gantt and PERT scheduling
  - Technical presentation

main-image: /Final_cad_modeling.jpg
---

## The Assignment

Our team was tasked with developing a solar energy integration concept for a campus building, taking it from initial feasibility through a defensible design recommendation. The team was four students across four disciplines: electrical, mechanical, civil, and aerospace. I was the electrical engineer.

Target capacity was 40 to 60 kW, sized to offset 15 to 25 percent of the building's daytime electrical load.

---

## Electrical Design

### Inverter architecture

The building's roof had partial shading from adjacent structures, which drove the central electrical decision: string inverters or microinverters.

String inverters are cheaper and simpler, but a shaded panel drags down the entire string it belongs to. Microinverters cost more per panel but isolate each one, so shading on a single module does not reduce output across the array. I evaluated both against the shading pattern on this specific roof.

### Interconnection

Tying a generation source into an existing building electrical system is governed by NEC Article 705. None of us had worked with it before, so I read the requirements directly rather than relying on secondary summaries, and developed an interconnection approach that met them.

---

## Site Constraints

The design was shaped as much by what the roof would not allow as by what we wanted:

- **Structural loading.** Panels and mounting hardware add dead load, and wind uplift under ASCE 7 was a governing constraint on how the array could be anchored.
- **Maintenance access.** Facilities required 4 ft pathways preserved across the roof, which removed usable area from the layout.
- **Fixed obstructions.** Existing rooftop mechanical equipment could not be relocated, so the array had to work around it.

---

## Alternatives Considered

We modeled three configurations and compared them on projected output, cost, and constructability:

| Option | Outcome |
|--------|---------|
| Rooftop mounted | Lowest cost, most affected by shading and obstruction losses |
| Canopy mounted | Approximately 18 percent higher projected production, higher structural cost |
| Hybrid | Marginal gain over canopy, significantly more complex |

We recommended the canopy-mounted design. Elevating the array cleared the shading and obstruction problems that constrained the rooftop option, and the production gain justified the additional structural work.

{% include image-gallery.html images="Engineering_solution_concept.jpg" height="400" %}

---

## Project Documentation

A large part of the program was learning how engineering work is actually delivered. Alongside the technical design, our team produced a project charter, business case, feasibility study, work breakdown structure, bill of materials, and Gantt and PERT schedules with critical path analysis.

At program close we presented the final concept and supporting documentation to an audience of practicing CAD engineers, engineering managers, and hiring managers.

{% include image-gallery.html images="Results_and_lessons.jpg" height="400" %}

---

## What I Took From It

Two things stuck with me. The first is that code research is a normal part of the job. Nobody handed us NEC 705 or ASCE 7; finding out which standards applied and reading them was the work.

The second is that constraints tend to produce the design. The 4 ft access pathways and the fixed rooftop equipment shaped our layout more than any preference we had going in, and the recommendation we landed on came from taking those seriously rather than designing around an ideal roof that did not exist.
