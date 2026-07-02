# PoplePAC — 3D Molecular Editor

**PoplePAC** is an interactive molecular editor inspired by classic tools like *HyperChem* and *Avogadro*. Built with **Python**, **PySide6**, and **VTK**, it provides a robust environment for building, visualizing, and analyzing chemical structures.

---

## Key Features

### 3D Visualization
- **Rendering Modes:** **Ball & Stick**, **CPK** (Van der Waals), **Licorice**, **Wireframe**.
- **VTK Engine:** Dynamic lighting (key, fill, back lights), gradient background with toolbar color swatch.
- **Smart Labels:** `vtkBillboardTextActor3D` that auto-positions outside atom spheres, always facing the camera.
- **Molecular Axes:** Toggle XYZ axes at the molecular centroid — configurable length and cylinder/cone styling.
- **Dynamic Highlighting:** Hover (cyan) and selection (gold) with emphasized borders.
- **Measurement Visualization:** Distance lines, angle arcs, and dihedral planes with floating 3D labels.
- **ESP Surface:** Electrostatic potential mapped onto electron-density isosurfaces or VDW surfaces with scalar bar, configurable opacity and grid resolution, auto-adapting text color based on background luminance.
- **Molecular Orbital Visualization:** Positive/negative isosurfaces for individual MOs with configurable isovalue, colors, and opacity.
- **Spin Density Visualization:** Alpha − beta electron density rendered as ± isosurfaces alongside MO and ESP modes.
- **Visual Style Control:** Dedicated dock for high-level customization of atoms and labels. Adjust font families (standard VTK Arial, Courier, Times), font sizes, bold styles, colors, and opacity with real-time updates.
- **Interactive Label Movement [Move Mode]:** Unique feature to interactively reposition labels (atom names, measures, coordination symmetry) in 3D space. Click and drag labels with the mouse; displacements are preserved in screen-space regardless of camera rotation or zoom.
- **Contextual UI feedback:** Cursor changes (Pointing Hand on hover, Closed Hand on drag) and automatic color adaptation for maximum readability.
- **One-Click Reset:** Factory reset buttons to restore manual label displacements and styling to original defaults.

### Building Tools
- **Add Atom [A]:** Drag from an atom to create a new bonded atom; click empty space to place an isolated atom; click a bond to cycle bond order (Single → Double → Triple → Remove); click an existing atom to change its element.
- **Aromaticity Detection:** **Shift+Double Click** on any bond within a ring to automatically detect the shortest cycle (BFS) and convert all its bonds to aromatic order (1.5).
- **Bond picking:** Fuzzy bond detection picks bonds even when clicking between atoms in multi-bond regions.
- **Element selector:** Full periodic table — all 118 elements with CPK colors, radii, and default valences.

- **Visual Style:** Dock for real-time appearance customization, label formatting, and interactive displacement toggles.
- **Coordination Chemistry Analysis**
- **Automatic Metal Detection:** Identifies 80+ metal elements (alkali, alkaline earth, transition, lanthanide, actinide, post-transition).
- **Ligand Identification:** Distance-based with configurable covalent radius tolerance, or bond-connectivity-based using explicit molecular graph.
- **Continuous Shape Measures (CShM):** Native implementation using Kabsch alignment + exhaustive permutation search + Hungarian algorithm. No external SHAPE dependency. Validated against SHAPE 2.1 (0.000 Å mean difference on 39 CSD structures).
- **60+ Ideal Polyhedra:** Exact SHAPE 2.1 coordinates for CN=2 through CN=12 (octahedral, tetrahedral, square planar, trigonal bipyramid, square antiprism, muffin, etc.).
- **Coordination Dock:** Metal selector, geometry classification with symmetry labels (e.g., "CSAPR-9 (C4v)"), CShM values, full ranking, M-L distances, and HTML report export with SVG bar charts.
- **Atom Ligands Filtering:** Checkbox to isolate metal + coordinated ligand atoms in the 3D scene, hiding non-coordinated atoms, centroids, and irrelevant bonds for focused analysis.
- **3D Polyhedron Visualization:** VTK Delaunay3D convex hull with configurable opacity, color, and edge styling.

### Geometry Optimization
- **Interactive Auto-Optimize [Auto Checkbox]:** Live energy minimization using RDKit (MMFF94/UFF/UFF-Ln) that runs continuously. Allows users to click and drag atoms in 3D space, which act as temporary fixed points while the rest of the molecule elastically relaxes around the mouse cursor.
- **MMFF94 / MMFF94s / UFF:** RDKit force fields with selective optimization (fixed atoms) and iterative progress reporting.
- **UFF-Ln:** Dedicated lanthanide UFF with distance/angle/VDW constraints using parameters optimized against 1658 CSD structures. Supports Eu, Gd, Tb, Dy.
- **MOPAC:** PM7, PM6, AM1, RM1, PM3, MNDO, MINDO/3 semi-empirical methods.
- **xTB:** GFN2-xTB, GFN1-xTB, GFN0-xTB, GFN-FF with ALPB implicit solvent (13 solvents), charge/multiplicity control, and selective optimization.

### Measurement & Analysis
- **Measure Tool [E]:** Distance (2 atoms), Angle (3 atoms), Dihedral (4 atoms) with editable values in the tool panel.
- **Centroids [Ctrl+K]:** Virtual points at the center of mass of a selection.
- **Point Group Detection:** Automatic Schoenflies point group displayed in the status bar and available via Build > Analyze Symmetry.

### Fragment Library
- **51 fragments** across 9 categories — alkyl, aromatic, functional groups, betadiketones, coordination ligands, MOF linkers, organometallic nodes, biomolecules, solvents.
- **PM6-optimized geometries** with MOL2 Sybyl atom types and Mulliken charges.
- **Anchor-based insertion** with automatic valence management, overlap removal, and smart bond detection.
- **Independent fragment manipulation:** Right-drag to rotate, Shift+Right-drag to translate, Ctrl+Right-drag to spin.
- **Interactive ghost preview** with snapping, collision detection, and anchor cycling (R/T/Z keys).

### File I/O
- **Import:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.mmcif`, `.cml`, `.out`, `.orcout` (ORCA/MOPAC) — via OpenBabel with automatic bond perception.
- **Export:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.smi`, `.pdbqt`, `.gro`, `.tmol`.
- **Save (Ctrl+S):** Saves directly to current file; Save As (Ctrl+Shift+S) with format chooser.
- **Recent Files:** Last 10 opened files in File > Open Recent.
- **Drag & Drop:** Drop files directly into the 3D viewport.
- **Background Loading:** Large files (>2000 atoms) load in background thread with progress dialog and switch to optimized read-only mode.
- **Auto-Save:** Saves molecule state every 5 minutes. Crash recovery dialog on next startup.

### Quantum Output Viewers
- **MOPAC Viewer:** Real-time output streaming, settings editor (charge, multiplicity, keywords), wavefunction parsing from `.aux` files, MO/ESP/Spin Density visualization, and export of calculation files.
- **ORCA Viewer:** Wavefunction parsing from `.out` / `.orcout` files (basis set + MO coefficients + unrestricted alpha/beta), MO/ESP/Spin Density visualization, and GBW metadata detection.
- **Excited States Analysis (ORCA TD-DFT):** Fragment-based characterization of singlet and triplet excited states. Decomposes MO contributions by ligand fragment with absorption spectrum plotting (Gaussian/Lorentzian convolution, wavelength/energy/eV units), interactive stick tooltips showing transition weights and orbital labels, CSV export of convolved spectrum, and HTML/text report generation.
- **Structure Alignment & RMSD:** Toggle between Input/Output geometry with one click. Merge (RMSD) mode aligns initial and final structures via Kabsch algorithm and displays the aligned overlay with RMSD label directly in the 3D scene.
- **Embedded Search Bar:** Collapsible Find bar with keyword highlighting (yellow matches, red active) and Previous/Next navigation in all quantum output panels.

### User Experience
- **Undo/Redo:** Full command history across all editing operations.
- **Quick Element Substitution:** Hover an atom and type Shift+letters (e.g., Shift+Cl for chlorine).
- **Dirty Flag:** Window title shows `*` when there are unsaved changes. Prompts on close/new/clear only when dirty.
- **Multiple Windows:** File > New opens an independent new PoplePAC window.
- **Save Image:** Capture the VTK viewport as PNG/JPEG from the Surface dock.
- **Keyboard Shortcuts:** Single-key tool switching (N/S/A/D/M/E/I) and standard shortcuts (Ctrl+Z/Y/C/V/I/H/G/K/L/0).

---

## Keyboard Shortcuts

| Key | Action |
| :--- | :--- |
| **N** | Navigate Tool |
| **S** | Select Tool |
| **A** | Add Atom Tool |
| **D** | Delete Tool |
| **M** | Move Tool |
| **E** | Measure Tool |
| **I** | Insert Fragment |
| **Ctrl+Z / Y** | Undo / Redo |
| **Ctrl+C / V** | Copy / Paste |
| **Ctrl+S** | Save |
| **Ctrl+Shift+S** | Save As |
| **Ctrl+N** | New Window |
| **Ctrl+O** | Open File |
| **Ctrl+I** | Optimize Geometry |
| **Ctrl+H** | Add Hydrogens |
| **Ctrl+G** | Guess Bonds |
| **Ctrl+K** | Create Centroid |
| **Ctrl+L** | Toggle Labels |
| **Ctrl+0** | Reset Camera |
| **Ctrl+Shift+Y** | Toggle Molecular Axes |
| **Delete** | Remove Selection |
| **Escape** | Clear Selection |
| **Shift+Letter** | Quick Element Substitution |

---

## Supported Formats

**Import:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.mmcif`, `.cml`, `.out`, `.orcout` (ORCA/MOPAC)

**Export:** `.xyz`, `.mol`, `.mol2`, `.sdf`, `.pdb`, `.cif`, `.smi`, `.pdbqt`, `.gro`, `.tmol`

---

## Project Architecture

- `poplepac/core` — Molecule/Atom/Bond/Centroid data models, periodic table (118 elements), fragment library, optimization engines (MMFF94, UFF, UFF-Ln, MOPAC, xTB), electronic structure (wavefunction, density, ESP, spin density), CShM coordination analysis, Cython acceleration.
- `poplepac/commands` — Command pattern for undo/redo across all editing operations.
- `poplepac/tools` — Interactive tools: navigate, select, add atom, delete, move, measure, insert fragment.
- `poplepac/coordination` — Metal detection, ligand identification, CShM computation (Kabsch+Hungarian+exhaustive), ideal polyhedron database (60+ SHAPE 2.1 geometries), geometry classification, Cython exhaustive search kernel.
- `poplepac/view` — VTK scene management, 3D rendering (atoms, bonds, labels, orbitals, ESP surfaces, measurement overlays, polyhedra).
- `poplepac/ui` — PySide6 GUI, main window, docks (Tool Options, Properties, Coordination Analysis, Orbital/Surface, MOPAC, ORCA, xTB), fragment dialog, diagram editor.
- `poplepac/io` — File I/O via OpenBabel and RDKit, ORCA/MOPAC output parsing, cube file I/O.

---

## License
This project is for educational and research purposes in molecular modeling.
