# Plate Pusher

**Interactive finite-element analysis of 2D plates: bending out of plane and membrane action in plane.**

Plate Pusher is a single-file web app for engineers and students. Draw a slab, wall or plate, support it, load it and see how it carries load. Results update as you edit. Its look and layout follow [Frame Pusher](https://claude.ai/artifact/7igCHuhTxqEmDHG8SntAUn), the companion 2D frame tool.

- **Live app:** https://claude.ai/artifact/9Uc8jHt3B8UhyCdnPh3oFy
- **Version:** 1.1 (2 November 2026)
- **Units:** kN and m. Inputs: E in GPa, thickness and element size in mm. Outputs: moments in kNm/m, shears and membrane forces in kN/m, stresses in MPa, deflections in mm.

---

## Contents

1. [Features](#features)
2. [Quick start](#quick-start)
3. [Tools and shortcuts](#tools-and-shortcuts)
4. [Results](#results)
5. [Section cuts](#section-cuts)
6. [Element interrogation](#element-interrogation)
7. [Stiffness-matrix inspector](#stiffness-matrix-inspector)
8. [Examples](#examples)
9. [Verification](#verification)
10. [Method](#method)
11. [Sign conventions](#sign-conventions)
12. [Limitations](#limitations)
13. [Saving and opening models](#saving-and-opening-models)
14. [Source layout and build](#source-layout-and-build)
15. [Roadmap](#roadmap)

---

## Features

| Area | What you can do |
|---|---|
| **Drawing** | Draw plates and openings as rectangles, polygons or circles; several plates join into one. Add mesh refinement zones. Snap to a grid, vertices and edges. Select, move and reshape items with handles. Undo and redo. |
| **Supports** | Edge supports: simple (soft or hard), clamped, line springs, in-plane pins and rollers, fully fixed. Point supports: columns, fixed columns and springs. Elastic (Winkler) ground under a plate or a patch. |
| **Loads** | Out of plane: pressure over a whole plate or a patch, line loads and point loads. In plane: point forces and edge tractions. |
| **Annotation** | Text notes, arrows, dimensions and boxes. |
| **Results** | Contours of deflection, moments, shears, principal moments, Wood–Armer design moments, membrane forces and stresses (including von Mises). Options: averaged or unaveraged values, contour lines, peak labels, principal vectors, deformed shape, reactions, and a rotatable 3D view of the deflected plate. |
| **Interrogation** | Section cuts with charts along the cut and integrated resultants. A readout of the selected result at the cursor. |
| **Learning** | A live stiffness-matrix inspector, an element-by-element assembly stepper, element matrices with their eigenvalues, a method guide and a closed-form verification suite. |
| **Getting started** | A welcome splash page, an examples gallery with 18 models, and a keyboard-shortcut guide. |

---

## Quick start

1. Open the app. On the welcome page choose **Start with a simple slab**, **Examples gallery** or **Blank canvas**.
2. **Draw a plate.** Press `R` and drag a rectangle, or choose Polygon or Circle in the tool flyout.
3. **Support it.** Press `L` and click an edge, or Shift-click the plate to support its whole outline.
4. **Load it.** Press `Q` and click the plate to apply a pressure over all of it.
5. **Read the results.** Choose a quantity under **Result** in the bottom bar. Hover over the plate to read values at the cursor.
6. **Cut a section.** Press `S` and drag a line across the plate. The side panel plots results along the cut and integrates them.
7. **Look inside the solver.** Press `M` to open the stiffness matrix.

Pan by dragging empty space or right-dragging; zoom with the scroll wheel. `F` fits the view and `Esc` cancels the current action.

---

## Tools and shortcuts

| Key | Tool | Use |
|---|---|---|
| `V` | Select | Click to select and edit in the pop-up; drag to move; drag handles to reshape. |
| `R` | Plate | Drag a rectangle, click polygon corners, or drag a circle from its centre. |
| `O` | Opening | Same as Plate; cuts a hole through the plate. |
| `Z` | Mesh zone | Drag a rectangle where the mesh should be finer. |
| `L` | Edge support | Drag along an edge, click an edge, or Shift-click a plate for its whole outline. |
| `C` | Point support | Click to place a column, spring or in-plane support. |
| `G` | Elastic ground | Click a plate or drag a patch (Winkler modulus k in kN/m³). |
| `Q` | Pressure | Click a plate for the whole plate, or drag a patch (kPa). |
| `W` | Line load | Drag a line or click an edge (kN/m). |
| `P` | Point load | Click to place (kN). |
| `I` | Force | In-plane point force. Click to place; drag to set its direction. |
| `T` | Edge traction | In-plane load qx, qy (kN/m) along a line or edge. |
| `S` | Section cut | Drag a line across the plate. |
| `E` | Element | Click inside an element to interrogate it; drag to move the point. |
| `N` `A` `D` `B` | Text, arrow, dimension, box | Annotations. |
| `X` | Delete | Click an item to delete it. |

**General keys**

| Key | Action |
|---|---|
| `Esc` | Cancel, then deselect, then return to the Select tool. |
| `Enter` or double-click | Close a polygon. |
| `Del` | Delete the selection. |
| `Ctrl+Z` / `Ctrl+Y` | Undo / redo. |
| `F` | Fit view. |
| `M` | Open or close the stiffness matrix. |
| `Shift` | Draw horizontal or vertical lines; supports the whole outline with Edge support. |
| `Ctrl+S` | Save or open a model. |
| `?` | Welcome guide. |

---

## Results

**Out of plane (bending)**

| Quantity | Units | Notes |
|---|---|---|
| w | mm | Deflection, positive in the direction of positive pressure (down). |
| βx, βy | mrad | Rotations of the plate normal. |
| Mx, My, Mxy | kNm/m | Bending and twisting moments; sagging is positive. |
| M1, M2 | kNm/m | Principal moments. |
| Qx, Qy, \|Q\| | kN/m | Transverse shear forces and their resultant. |
| Mx\*, My\* (bottom and top) | kNm/m | Wood–Armer design moments for orthogonal x/y reinforcement. |
| σb | MPa | Surface bending stress, 6Mx/t². |

**In plane (membrane)**

| Quantity | Units | Notes |
|---|---|---|
| ux, uy, \|u\| | mm | Displacements. |
| Nx, Ny, Nxy | kN/m | Membrane forces; tension is positive. |
| σx, σy, τxy | MPa | Stresses. |
| σ1, σ2, τmax | MPa | Principal stresses and maximum shear stress. |
| σvm | MPa | Von Mises stress. |

**Display options**

- **Averaged:** nodal averaging smooths the results. Untick it to see the jumps between elements, which are a direct measure of discretisation error.
- **Display box:** sets the scales for deformation, cut diagrams and vectors, the number of contour bands (or smooth shading), opacity and the colour palette.
- **Colour scale:** diverging when a result changes sign, with zero at the centre and each side running to its own extreme. Otherwise sequential.
- **About this result:** a collapsible box under the colour legend explains the selected result with a diagram: what it represents, its sign and units, and how to use it in design.
- **Info panel:** shows maximum deflection, span-to-deflection ratio, total load against total reaction (an equilibrium check), mesh size and solve time.

---

## Section cuts

A cut samples results along a straight line, from A to B. In the side panel you can:

- plot the cut quantities:
  - out of plane: w, Mn, Mt, Mnt, Qn;
  - in plane: Nn, Nt, Nnt, σn, σt, τnt, un, ut;
- move a position slider (or hover a chart) to read every quantity at one point;
- choose which quantity is drawn as a diagram on the canvas;
- read the **resultants across the cut**, integrated over the part of the cut that lies on the plate:
  - out of plane: ∫Mn ds (bending moment), ∫Mnt ds (twisting moment), ∫Qn ds (shear force), and the average Mn;
  - in plane: ∫Nn ds (normal force), ∫Nnt ds (shear force), ∫Nn(s − s̄) ds (in-plane moment about the centre of the on-plate length), and the mean σn;
- copy the samples as CSV.

The normal **n** points to the left of A → B, and **t** runs from A to B. The resultants are what a free body cut at the line must carry, so you can check them against statics. The examples and the verification suite do exactly that.

---

## Element interrogation

Press `E` and click inside an element. The side panel (like Frame Pusher's member interrogation) shows, at the point you clicked:

- the **parent element** in natural coordinates (r, s) with its Gauss points and, for bending, the MITC4 tying points, beside the **real element** shaded by the interpolated field; drag the point in either view or on the plate;
- the four **shape functions** Nᵢ = ¼(1 + rᵢr)(1 + sᵢs) as small contour maps with their values, the derivatives ∂Nᵢ/∂x, ∂Nᵢ/∂y and the nodal DOFs;
- the **interpolation** written out: w = ΣNᵢwᵢ (and βx, βy, or u, v) with numbers, and the partition-of-unity check;
- the **Jacobian** J, det J and J⁻¹;
- out of plane: **curvatures and moments** (κ = ∂β, M = Dκ) compared with the averaged contour values, and the **MITC4 assumed shear**: strains at the four tying points, their interpolation, Q = κGtγ, and the locking-prone direct value for comparison;
- in plane: **strains** split into the bilinear part and the incompatible-mode part (with the condensed mode amplitudes α), and N = Dε.

## Stiffness-matrix inspector

Press `M` or click **Stiffness matrix**. The panel follows Frame Pusher's layout. It can be dragged, maximised (⤢), collapsed (▾) or **popped out into its own window** (⧉; dock it back with ⇲ or by closing the window). If the browser blocks pop-ups, maximise it instead.

- **Navigate:** scroll to zoom about the cursor, drag to pan, click a cell to pin it. **Fit** resets the zoom; **Read** zooms until values (or equations) can be read. Only the visible part is drawn, so even large matrices zoom down to single entries.
- **Cell text:** values, element equations, or both. With equations, each cell lists the elements that contribute and their bending and shear parts (out of plane), or the K<sub>cc</sub> part and the condensation term (in plane).
- **Element stiffness equation behind the selected cell:** for each contributing element, the local entry, the symbolic integrand (for example D[N<sub>a,x</sub>N<sub>b,x</sub> + ½(1−ν)N<sub>a,y</sub>N<sub>b,y</sub>] + κGt(γ̄<sub>xz,a</sub>γ̄<sub>xz,b</sub> + γ̄<sub>yz,a</sub>γ̄<sub>yz,b</sub>)), a table of the four Gauss-point terms, their sum and a check against the element matrix.
- **Build K element by element:** step (◀ ▶), drag or play (▶▶) the assembly; the latest element's entries are outlined in yellow and the element is highlighted on the plate.

- **System:** out-of-plane K (3 DOFs per node: w, βx, βy) or in-plane K (2 DOFs per node: u, v).
- **Show:** free DOFs (K<sub>ff</sub>, the equations that are solved) or all DOFs before supports are applied.
- **Order:** solver order (reverse Cuthill–McKee) or node numbering. Compare the two to see how renumbering narrows the band.
- **Colour:** log |K<sub>ij</sub>| with sign, linear, or sparsity pattern only. A size setting (small to extra large) sets the fitted view.
- **Hover an entry** to read K<sub>ij</sub> with its units and the row and column DOFs. The panel lists each element term that adds up to the entry, and highlights those elements and nodes on the plate. Click to pin an entry.
- **Hover a node on the plate** to highlight its rows and columns in the matrix.
- **Vectors:** u and F beside the matrix (plus R when showing all DOFs), so K u = F can be read directly.
- **Assemble:** step or play through assembly one element at a time. The next element's footprint is outlined in the matrix and the element is highlighted on the plate.
- **Statistics:** number of equations, non-zeros, density, half-bandwidth, skyline storage, factorisation cost, diagonal range and symmetry error.
- **Condition number:** an estimate of λmax/λmin (power and inverse iteration), with the number of significant digits lost.
- **Element matrix:** pick an element on the plate to see its 8 × 8 or 12 × 12 matrix, coloured by magnitude. For bending you can view the total, bending part or shear part. Its eigenvalues show the three rigid-body modes and no spurious mechanisms.

---

## Examples

Open them from the **Examples** button, the gallery button in the tool bar, or the drop-down. Each loads with its supports, loads and a section cut, and most include a note giving the textbook value to compare against.

| Group | Example | What it shows |
|---|---|---|
| Slabs | Two-way slab, simply supported | 6 m square, 200 mm, 10 kPa; compare with Timoshenko & Woinowsky-Krieger. |
| | Two-way slab, clamped edges | Hogging at the edges and the reduced centre moment. |
| | Rectangular slab, b/a = 2 | Load sharing between the short and long spans. |
| | Flat slab on 16 columns | 3 × 3 bays of 6 m; panel moments and Wood–Armer design moments. |
| | Cantilever balcony | Root moment from pressure plus a balustrade line load. |
| | L-shaped slab with an opening | Re-entrant corner effects and moment trajectories. |
| | Circular tank cover | Boundary-fitted mesh; compare with Roark. |
| | Raft on elastic ground | Six column loads on Winkler springs. |
| | Slab with a partition wall | A line load ridge in the moment field. |
| | Skew slab, 30° | Load into the obtuse corners and rotating principal moments. |
| In-plane | Deep transfer beam | Non-linear stress across the depth; compression arch and tension tie. |
| | Shear wall with doorways | The cut recovers the base shear and overturning moment exactly. |
| | Plate with a hole in tension | Stress concentration of about 3 (Kirsch). |
| | Deep cantilever | Timoshenko beam deflection and exact cut resultants. |
| | Dapped beam end | Tension at the re-entrant corner. |
| Verification | Navier plate, plate strip = beam, point load on Winkler ground | Closed-form set-ups. |

---

## Verification

**Verification** in the top bar runs the full suite in the browser. Each check builds a model, solves it with the same code the app uses, and compares a result with its textbook value. At release, all 69 checks pass.

| Group | Checks | Reference |
|---|---|---|
| Element checks | Exactly 3 rigid-body modes and positive stiffness otherwise, for distorted QM6 and MITC4 elements; symmetry; membrane patch test on a distorted mesh (σx, σy, extension, Poisson contraction exact); twist patch test. | Patch tests |
| Rectangular plates | SS square: w = 0.00406 qa⁴/D, Mx = 0.0479 qa², reaction = qa². SS b/a = 2: w, Mx, My. SS square under a point load: w = 0.01160 Pa²/D. Clamped square: centre w and M, and the mid-edge moment. | Timoshenko & Woinowsky-Krieger, Tables 8, 23, 35 |
| Thick plate | t/a = 0.1, hard simple supports: w = 0.004273 qa⁴/D; soft supports are more flexible. | Navier series with shear term |
| One-way action | Strip with ν = 0: 5qL⁴/384D and qL²/8; cut resultants at L/2 and L/4. Cantilever strip: tip deflection including shear, root moment, cut moment. | Beam theory, statics |
| Circular plates | Simply supported and clamped, UDL: centre deflection and moment. | Roark |
| Elastic ground | Large plate under a point load: w = P/8√(kD); ground reaction = P. | Hertz |
| In-plane | Deep cantilever: tip deflection (Timoshenko beam), fibre stress, cut shear, moment and axial force. Plate with a hole: stress concentration (Howland/Heywood) and Kirsch σy. Deep beam equilibrium. | Timoshenko & Goodier, beam theory |
| Solver | Mechanism detection; stability on three columns; RCM shrinks the skyline. | — |
| Examples | Every example: reactions balance the applied loads. | Equilibrium |

Typical errors are 0.1–0.8% on deflections and moments. Section-cut resultants match statics to rounding. The slow-converging checks have wider tolerances, stated in the table.

---

## Method

**Two problems on one mesh.** A flat, linear, small-deflection plate carries out-of-plane and in-plane loads independently. Plate Pusher solves each as its own system on the same mesh and shows whichever has loads.

**Bending: MITC4 Reissner–Mindlin plate.**
- Four-node quadrilateral with 3 DOFs per node: w, βx, βy.
- Curvatures κ = ∂β; moments M = D κ, with D = Et³/12(1−ν²).
- Transverse shear uses the Bathe–Dvorkin assumed strain, tied at the edge mid-points. This removes shear locking, so thin plates converge to Kirchhoff theory and thick plates include shear deformation (κ = 5/6).
- 2 × 2 Gauss integration.

**Membrane: QM6 incompatible-mode quadrilateral.**
- Four nodes with 2 DOFs per node, plane stress.
- Wilson's incompatible modes (1 − r², 1 − s²) with Taylor's correction, so the element passes the patch test and is accurate in in-plane bending.
- The modes are condensed out inside each element, giving an 8 × 8 matrix.

**Mesh.**
- A structured grid follows every horizontal and vertical edge, support, load and refinement zone.
- Cells are kept where their centre lies on the plate.
- Boundary nodes are moved onto curved or sloping edges; a move is undone if it would badly distort an element.
- Elements with identical shapes share one stiffness matrix.

**Solver.**
- Nodes are renumbered by reverse Cuthill–McKee, then solved by skyline (envelope) Cholesky factorisation.
- A zero or negative pivot signals a mechanism, which is reported with the DOF where it was found.
- Reactions are K u − F at the restrained and spring DOFs.

**Recovery.**
- Stress resultants are computed at the Gauss points, extrapolated to the corners and averaged at the nodes (optional).
- Principal values, Wood–Armer moments and von Mises stress are derived from these.

**Supports.**
- *Simple, soft* restrains w only.
- *Simple, hard* also restrains the rotation along the edge (horizontal or vertical edges only). This is the Navier condition and matters for thick plates.
- *Clamped* restrains w and both rotations.
- Point supports act at the nearest mesh node.
- Springs and elastic ground add stiffness to the matrix diagonal.

---

## Sign conventions

- z and w are positive in the direction of positive pressure, so gravity loads and the resulting deflections are positive (down).
- Sagging moments are positive. Mx bends about the y axis: it spans in x and governs bars running in x. Mxy is the twisting moment.
- Qx = ∂Mx/∂x + ∂Mxy/∂y.
- βx, βy are rotations of the normal (u = zβx, v = zβy). For a thin plate βx = −∂w/∂x.
- Membrane forces and stresses are positive in tension.
- Wood–Armer: bottom M\* ≥ 0, top M\* ≤ 0.

---

## Limitations

- Linear elastic, isotropic material with constant thickness and small deflections.
- No coupling between bending and membrane action: no large deflection and no eccentric stiffeners.
- Point loads and point supports are singular. The peak moment there (and, in Mindlin theory, the shear deflection) grows as the mesh is refined. Read values a short distance away, or spread the load over a patch.
- Re-entrant corners and the corners of openings are stress singularities in an elastic model too.
- Clamped-edge moments converge slowly from below (about 10% low on a 40 × 40 mesh). The stress at a hole on a snapped boundary is within about 5%.
- Refinement zones refine a band right across the plate, not only locally.
- Concrete cracking, creep and reinforcement are not modelled.

---

## Saving and opening models

**Save / open** (`Ctrl+S`) shows the model as plain JSON: geometry, supports, loads, cuts, notes, material and mesh.

- **Save:** click **Copy text** and keep the text in a `.json` file. The artifact viewer blocks direct downloads.
- **Open:** choose **Open a file…** or paste the text and click **Load the text below**.
- The current model is also kept in the browser's local storage and restored when you reopen the app.

---

## Source layout and build

| File | Contents |
|---|---|
| `src/engine.js` | Mesher, elements (QM6 and MITC4), assembly, RCM ordering, skyline solver, supports and loads, recovery, point location, section sampling. |
| `src/tests.js` | Verification suite (`runVerification()`). |
| `src/app.js` | Canvas view, tools, snapping, drawing, interaction, contours, 3D view, info panel. |
| `src/panels.js` | Selection pop-up and section-cut side panel. |
| `src/matrix.js` | Stiffness-matrix inspector (virtual canvas, zoom, equations, pop-out). |
| `src/interrogate.js` | Element interrogation panel. |
| `src/describe.js` | Result descriptions and diagrams. |
| `src/content.js` | Examples, gallery, welcome guide, dialogs, start-up. |
| `src/style.css` | Theme (Frame Pusher v2 neon tokens) and layout. |
| `src/body.html` | Page markup. |
| `build.sh` | Concatenates everything into `dist/index.html`. |

**Build:**

```sh
./build.sh          # writes dist/index.html
```

**Headless tests** (Node.js): run the engine and verification suite in a `vm` context.

```sh
node t/run.js       # prints each check and the pass count
```

---

## Roadmap

- Local (graded) mesh refinement instead of full-width bands.
- Orthotropic plates (ribbed slabs, CLT).
- Superconvergent patch recovery for sharper edge moments.
- Reinforcement design from Wood–Armer moments.
- Eccentric beams and coupling between bending and membrane action.
- Load cases and combinations.
- A printable report.
