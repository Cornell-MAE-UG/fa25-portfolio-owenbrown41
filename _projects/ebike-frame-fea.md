---
layout: project
order: 1
title: Step-Through E‑Bike Frame
description: A commuter e-bike frame with no top tube, designed in Fusion 360, analyzed with shell FEA in Ansys, and optimized for mass against a factor of safety of 2.
course: MAE 3150
term: Spring 2026
technologies: [Fusion 360, Ansys Mechanical, Ansys DesignXplorer, Shell FEA, Parametric optimization]
image: /assets/images/ebike-fea/new-deformation.jpg
card_image: /assets/images/ebike-fea/card.jpg
imagealt: Ansys total deformation contour on the step-through frame, highest at the top of the seat tube
imagecaption: "Total deformation, final design. Peak of 0.189 mm at the top of the seat tube."
stats:
  - label: Min factor of safety
    value: "2.73"
    note: Requirement was 2.0
  - label: Frame mass
    value: "1.25"
    unit: kg
    note: At 2 mm shell thickness
  - label: Peak von Mises stress
    value: "102.6"
    unit: MPa
    note: Back of the bottom bracket
  - label: Max deformation
    value: "0.189"
    unit: mm
    note: Under 700 N rider + 150 N pedal load
---
## The brief

The final project for MAE 3150 was to design an electric commuter bike frame in Fusion 360 and analyze how it responds to load in Ansys. Two requirements shaped the design. The frame couldn't have a top tube, so riders can step through instead of swinging a leg over. It also needed a place to mount a battery.

A frame like this is exactly the kind that ends up in a city bike-share fleet. It's easy to get on, it puts out zero emissions, and it gets people outside. If the frame is safe, that's cheap, healthy transportation for a lot of people.

## Checking the setup on the baseline frame

Before working on my own design, I ran the class's provided geometry to make sure my setup was right. Ansys modeled the frame as **surface shells** at 2 mm thickness rather than as solids, which is the right idealization for thin-walled tubes.

<table class="spec">
<thead><tr><th>Setup</th><th>Value</th></tr></thead>
<tbody>
<tr><td>Element type</td><td>Shell, 2 mm thickness</td></tr>
<tr><td>Fixed supports</td><td>Head tube and rear thru-axle, the points the wheels hold in place</td></tr>
<tr><td>Rider load</td><td>700 N</td></tr>
<tr><td>Pedaling load</td><td>150 N</td></tr>
</tbody>
</table>

<div class="fig fig-wide"><img src="{{ '/assets/images/ebike-fea/baseline-loads.jpg' | relative_url }}" alt="Baseline frame with fixed supports and the two applied forces marked" loading="lazy"><p class="cap"><span class="fig-num">FIG. 01</span>Baseline frame with supports and loads applied.</p></div>

The baseline came out at **0.111 mm** max deformation, **52.5 MPa** peak von Mises stress, and a minimum factor of safety of **5.3**. Those numbers are the reference point for everything after this.

<div class="fig-grid"><div class="fig"><img src="{{ '/assets/images/ebike-fea/baseline-deformation.jpg' | relative_url }}" alt="Baseline total deformation contour" loading="lazy"><p class="cap"><span class="fig-num">FIG. 02</span>Baseline total deformation.</p></div><div class="fig"><img src="{{ '/assets/images/ebike-fea/baseline-stress.jpg' | relative_url }}" alt="Baseline equivalent stress contour" loading="lazy"><p class="cap"><span class="fig-num">FIG. 03</span>Baseline von Mises stress.</p></div></div>

## My design

I removed the top tube and added a cross tube low in the front triangle, between the down tube and the seat tube. That makes a small triangle that holds the battery. The cross tube also braces the front of the frame, so a hit to the front wheel from a curb or pothole isn't carried by the down tube alone. I kept the original tube diameters and moved the seat stays' attachment point lower on the seat tube.

<div class="fig fig-wide"><img src="{{ '/assets/images/ebike-fea/new-design-cad.jpg' | relative_url }}" alt="Fusion 360 model of the step-through frame with a low cross tube forming a battery triangle" loading="lazy"><p class="cap"><span class="fig-num">FIG. 04</span>Fusion 360 model. The low cross tube forms the battery triangle.</p></div>

Taking out the top tube cost stiffness, as expected. Max deformation rose from 0.111 mm to 0.189 mm. It's still less than a fifth of a millimeter, and it's in the same place on both frames: the top of the seat tube. Peak stress roughly doubled to 102.6 MPa, concentrated on the back of the bottom bracket, which is a common hot spot on bike frames. The minimum factor of safety of **2.73** still clears the required 2, so the frame stays in the linear-elastic range and doesn't yield.

<div class="table-scroll"><table class="spec">
<thead><tr><th>Result</th><th class="num">Baseline</th><th class="num">My design</th></tr></thead>
<tbody>
<tr><td>Max deformation</td><td class="num">0.111 mm</td><td class="num">0.189 mm</td></tr>
<tr><td>Peak von Mises stress</td><td class="num">52.5 MPa</td><td class="num">102.6 MPa</td></tr>
<tr class="hl"><td>Min factor of safety</td><td class="num">5.3</td><td class="num">2.73</td></tr>
<tr><td>Peak stress location</td><td class="num">Bottom bracket</td><td class="num">Bottom bracket</td></tr>
</tbody>
</table></div>

<div class="fig-grid"><div class="fig"><img src="{{ '/assets/images/ebike-fea/new-stress.jpg' | relative_url }}" alt="Equivalent stress contour on the new design with the maximum at the bottom bracket" loading="lazy"><p class="cap"><span class="fig-num">FIG. 05</span>Von Mises stress. Peak at the bottom bracket.</p></div><div class="fig"><img src="{{ '/assets/images/ebike-fea/new-fos.jpg' | relative_url }}" alt="Safety factor contour on the new design with the minimum of 2.73 at the bottom bracket" loading="lazy"><p class="cap"><span class="fig-num">FIG. 06</span>Safety factor. Minimum of 2.73 at the bottom bracket.</p></div></div>

## Mesh convergence

I refined the mesh until the results stopped changing. Deformation settled cleanly on both frames, changing less than 0.2% on the last refinement.

Peak stress behaved differently. It was still rising at the last refinement, by 8.8% on the baseline and 7.5% on my design. Stress that keeps climbing as the mesh gets finer usually means a sharp corner, here the bottom bracket joint, where the stress keeps growing with mesh refinement. My next iteration would add a fillet at that joint so the peak stress settles to a real value.

<ul class="legend"><li><span class="key k-a"></span>Baseline frame</li><li><span class="key k-b"></span>My design</li></ul>
<div class="chart-pair"><div class="chart"><p class="chart-title">Deformation converges</p><p class="chart-sub">Max total deformation vs. mesh size</p>{% include charts/ebike-conv-deformation.svg %}</div><div class="chart"><p class="chart-title">Peak stress keeps climbing</p><p class="chart-sub">Max von Mises stress vs. mesh size</p>{% include charts/ebike-conv-stress.svg %}</div></div>
<div class="data-wrap"><details class="data"><summary>Convergence data</summary><div class="table-scroll"><table class="spec"><thead><tr><th>Frame</th><th class="num">Elements</th><th class="num">Deformation (mm)</th><th class="num">Stress (MPa)</th></tr></thead><tbody><tr><td>Baseline</td><td class="num">16,811</td><td class="num">0.10923</td><td class="num">30.2</td></tr><tr><td>Baseline</td><td class="num">18,353</td><td class="num">0.11015</td><td class="num">37.2</td></tr><tr><td>Baseline</td><td class="num">21,647</td><td class="num">0.11048</td><td class="num">48.1</td></tr><tr><td>Baseline</td><td class="num">27,457</td><td class="num">0.11058</td><td class="num">52.5</td></tr><tr><td>My design</td><td class="num">14,833</td><td class="num">0.18576</td><td class="num">52.5</td></tr><tr><td>My design</td><td class="num">15,555</td><td class="num">0.18819</td><td class="num">65.9</td></tr><tr><td>My design</td><td class="num">17,668</td><td class="num">0.18848</td><td class="num">82.7</td></tr><tr><td>My design</td><td class="num">21,919</td><td class="num">0.18877</td><td class="num">95.2</td></tr><tr><td>My design</td><td class="num">31,789</td><td class="num">0.18874</td><td class="num">102.6</td></tr></tbody></table></div></details></div>

## Optimizing wall thickness

In Ansys DesignXplorer I made shell thickness a parameter and ran a design of experiments from 2 to 4 mm, with two goals: minimize frame mass and keep the factor of safety at or above 2.

Mass grows linearly with thickness, but factor of safety grows much faster. Going from 2 mm to 4 mm doubles the mass and multiplies the factor of safety by almost five. So if you want a higher factor of safety, adding thickness buys a lot of it for relatively little weight.

<div class="chart fig-wide"><p class="chart-title">Thicker walls buy safety cheaply</p><p class="chart-sub">Minimum factor of safety vs. frame mass, one point per shell thickness. Hover a point for values.</p>{% include charts/ebike-tradeoff.svg %}</div>

<div class="table-scroll"><table class="spec">
<thead><tr><th>Shell thickness</th><th class="num">Frame mass</th><th class="num">Min FOS</th></tr></thead>
<tbody>
<tr class="hl"><td>2.0 mm</td><td class="num">1.252 kg</td><td class="num">2.73</td></tr>
<tr><td>2.5 mm</td><td class="num">1.564 kg</td><td class="num">4.10</td></tr>
<tr><td>3.0 mm</td><td class="num">1.877 kg</td><td class="num">5.73</td></tr>
<tr><td>3.5 mm</td><td class="num">2.190 kg</td><td class="num">7.99</td></tr>
<tr><td>4.0 mm</td><td class="num">2.503 kg</td><td class="num">13.12</td></tr>
</tbody>
</table></div>

The optimizer's three candidate points all landed at about 2.0 mm, 1.25 kg, and a factor of safety of 2.73, which is the bottom of the range I gave it. That raises the next question: how thin can the frame get before it drops below a factor of safety of 2? Answering it would take a second study that extends the thickness range below 2 mm.

## What I learned

Fusion 360 was new to me, and the hardest part of the project was getting my geometry into a state Ansys could mesh and solve. Ansys needs a lot from a model before it will analyze it cleanly and efficiently. I also took away a workflow I'll use on every simulation from now on:

1. **Start with the objective.** Know what you're solving for, such as frame deflection, durability, or response to an impact, before you open the software.
2. **Pick the analysis type** that answers that question most directly.
3. **Simplify and idealize.** Decals and small fillets don't change frame deflection, so leave them out and spend the compute on what matters. Use joints to idealize complicated part interactions.
4. **Set boundary conditions you can explain physically.** The thru-axles are "fixed" because the wheels hold them in place relative to the ground.
5. **Choose a material model** that fits the question. If the design shouldn't yield, a linear model works, and any stress above yield counts as failure.
